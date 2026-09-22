# Autonomous worker node

AWN-Spec v1.0. The code-and-Jev method.

## Id

autonomous-worker-node

## Scope

A stateless worker that accepts one task, runs a pluggable processing slot, and sends the result to an address the caller supplied.

## Load when

The step is an autonomous worker node, or you are grading one. This is the method that combines deterministic code with TypeSafe Jev.

## Do not load when

The step is only deterministic code, only a local GPU, or only a language model. Those methods have their own rubrics. Do not load this to grade the shape of a workflow. Use rubrics/rubric-2.1-workflow-design.md for that.

## Applies to

Modules built to AWN-Spec v1.0.

## Assumptions

1. The product is a standardized design for an autonomous worker node.
2. The processing stage is a slot. Jev rubrics, deterministic filters, or multi-step logic can be exchanged inside the slot without changing the outer shell.
3. The contract is asynchronous. The caller provides an ingestion address and a destination address. The node keeps no job state after the result is sent.

The caller passes the task. The node does not read ambient or global state. Judgment criteria, thresholds, and keys belong to the module. They are not a global the caller reaches into.

## Criteria

### Stateless execution

Each task carries the context the slot needs. The node does not keep that context after the result is sent.

| Level | Meaning |
| --- | --- |
| Missing | The node stores job context between tasks, or it reads a value it was not given. |
| Partial | The task carries some context, but the node still depends on leftover state. |
| Met | The task envelope is complete. After the result is sent, the node holds nothing from that job. |

### Asynchronous ingress and egress

The sender submits work and disconnects. The node delivers the finished artifact to the address in the task.

| Level | Meaning |
| --- | --- |
| Missing | The caller waits on the ingress connection for the result, or the destination is implied. |
| Partial | Work is queued, but the destination is fixed inside the node, or the caller must stay connected. |
| Met | Ingress validates the envelope, acknowledges, and releases the sender. The result goes only to `target_address`. |

### Envelope contract

Every module accepts a task envelope and emits a result envelope with the fields in this rubric.

| Level | Meaning |
| --- | --- |
| Missing | Payloads are ad hoc. |
| Partial | Some fields exist, but identity, destination, or execution status is missing or renamed. |
| Met | The task and result envelopes match the fields below. |

### Pluggable processing slot

The network shell is the same for every module. Only the slot changes.

| Level | Meaning |
| --- | --- |
| Missing | The judgment is mixed into the socket code. |
| Partial | A slot exists, but changing the judgment requires editing ingress or egress. |
| Met | Ingress, queue, and egress stay fixed. A new module implements the processing trait and nothing else in the shell. |

### Judgment phase

The slot checks the payload, asks typed Jev questions, then gates on confidence.

| Level | Meaning |
| --- | --- |
| Missing | The slot asks for open-ended prose instead of a typed question. |
| Partial | Jev is used, but Choice, Score, and Noul are collapsed into one question, or confidence does not gate the result. |
| Met | The slot sanitizes the payload, evaluates the needed Choice, Score, and Noul questions in parallel, gates on confidence, then assembles the artifact. |

### Black box and local bind

Internal criteria and keys are not recoverable from the binary. Ingress is local unless a caller is explicitly authenticated.

| Level | Meaning |
| --- | --- |
| Missing | Prompts, thresholds, or keys are plaintext in the binary, or ingress accepts non-local connections with no check. |
| Partial | The binary is stripped, but criteria strings are still readable, or the bind is broader than loopback without an auth check. |
| Met | The release binary is stripped and optimized as specified below. Criteria strings are not plaintext in the binary. Ingress is loopback or a user-scoped UNIX socket, unless an explicit auth token is checked. |

## Task envelope

```json
{
  "$schema": "https://specs.local/awn/v1/task_envelope.json",
  "task_id": "task_98f12bc4-3450-482a-a5f1-392daef0d844",
  "sender_id": "orchestrator_agent_03",
  "target_address": "http://127.0.0.1:9095/events/agent_sink",
  "priority": 1,
  "payload": {
    "context_type": "DOMAIN_SPECIFIC_IDENTIFIER",
    "data": {
      "raw_input_1": "value_a",
      "numerical_parameter": 42.5,
      "unstructured_notes": "Example input state to be evaluated"
    }
  }
}
```

- `task_id` (string, UUIDv4): tracing and deduplication.
- `sender_id` (string): the agent, multiplexer, or orchestrator that issued the task.
- `target_address` (URI): where the result is sent. HTTP or HTTPS, a UNIX socket (`unix:///tmp/sink.sock`), or local TCP (`tcp://127.0.0.1:9099`).
- `priority` (integer, optional, default 1): 1 is real-time, 2 is batch.
- `payload` (object): the only job context the slot may use.

## Result envelope

```json
{
  "$schema": "https://specs.local/awn/v1/result_envelope.json",
  "task_id": "task_98f12bc4-3450-482a-a5f1-392daef0d844",
  "module_id": "AWN-ROUTING-GATEWAY-01",
  "sender_id": "orchestrator_agent_03",
  "execution_status": "COMPLETED",
  "error_details": null,
  "metrics": {
    "queue_ms": 1,
    "processing_ms": 118,
    "total_ms": 119
  },
  "result": {
    "decision": "PROCEED",
    "confidence": 0.942,
    "rubric_outputs": {
      "safety_noul": 0.98,
      "tactical_choice": "OPTIMIZE_IN_PLACE",
      "risk_score": 1.4
    },
    "custom_data": {}
  }
}
```

- `execution_status`: `COMPLETED`, `FALLBACK`, `REJECTED`, or `ERROR`.
- `metrics`: queue, processing, and total time.
- `result.confidence`: aggregate certainty from 0 to 1.
- `result.rubric_outputs`: the typed Jev answers the slot chose to keep.
- `result.custom_data`: other outputs from the slot.

## Processing slot

The shell deserializes the task, takes the payload, and calls one trait. A new module implements this and does not change the shell.

```rust
#[async_trait::async_trait]
pub trait WorkerProcessor: Send + Sync + 'static {
    fn module_id(&self) -> &'static str;

    async fn process(&self, payload: serde_json::Value) -> Result<ProcessOutput, ProcessError>;
}

pub struct ProcessOutput {
    pub decision: String,
    pub confidence: f32,
    pub rubric_outputs: serde_json::Value,
    pub custom_data: serde_json::Value,
}
```

The slot runs in this order:

1. Schema sanitation. Reject a payload that lacks the fields this module requires.
2. Jev evaluation. Ask Choice, Score, and Noul questions in parallel against the payload. Budget about 70ms to 300ms.
3. Confidence gate. At or above the high threshold, commit the decision. Below the low threshold, take the module's safe fallback.
4. Assemble the result envelope and hand it to egress.

Jev is TypeSafe's System One model. Choice picks one option. Score places the state on ordered levels. Noul is the probability a statement is true. See https://docs.typesafe.ai/primitives.md.

```
Caller
  │  task envelope
  ▼
Ingress (TCP 127.0.0.1 or UNIX socket)
  │  validate, acknowledge, release the caller
  ▼
Queue
  ▼
Processing slot
  │  sanitize → Jev (Choice, Score, Noul) → confidence gate
  ▼
Egress
  │  result envelope
  ▼
target_address (HTTP, UNIX socket, or local TCP)
```

## Black box

1. Release build, symbols stripped (`strip = true`), fat link-time optimization (`lto = "fat"`), and one codegen unit (`codegen-units = 1`).
2. Evaluation prompts, Jev criteria, and fallback thresholds do not appear as plaintext in the binary. They are unprotected only in memory for the request that needs them.
3. Default bind is loopback (`127.0.0.1`) or a UNIX socket under `/run/awn/` with mode `0600`. A non-local bind requires a verified auth token.

## Module matrix

Name a module `AWN-[DOMAIN]-[NAME]-[REV]`.

| Parameter | What to specify | Example |
| --- | --- | --- |
| Module ID | `AWN-[DOMAIN]-[NAME]-[REV]` | `AWN-CNC-TOOLPATH-SAFETY-01` |
| Ingress | TCP port or UNIX socket | `/run/awn/toolpath_safety.sock` |
| Expected payload | Required fields inside `payload` | Vector coordinates, feeds, cut depth |
| Jev primitives | Which of Choice, Score, and Noul this slot asks | Noul for collision risk, Score for deflection |
| Confidence cutoff | Auto-commit versus fallback | `>= 0.88` commits, below that flags a person |
| Default fallback | Safe result on timeout or Jev failure | `HOLD_AND_ALERT` |

## Evidence

The task and result envelopes, the processing trait implementation, the Jev question types used, the confidence gate, the bind address, and the release build settings.

## Out of scope

Whether the domain decision is the right one. That is a subject rubric. The shape of the surrounding workflow. Methods other than this worker node.
