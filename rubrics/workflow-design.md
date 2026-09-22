# Workflow design

Design a workflow for the best quality at the minimum token cost.

## Id

workflow-design

## Scope

How a workflow is shaped and which method each step uses.

## Load when

Designing a workflow, changing one, or checking that an orchestrator handed work off in a token-efficient way.

## Do not load when

Grading the subject of the work. Load the rubric for that subject instead.

## Applies to

Workflow designs and orchestrator handoffs.

## Criteria

### Modular steps

Each step is input, process, or output.

| Level | Meaning |
| --- | --- |
| Missing | The work is one blob. Input, process, and output cannot be separated. |
| Partial | Steps exist, but a step mixes concerns or cannot be rerun on its own. |
| Met | Every step is input, process, or output, and each can be understood and rerun on its own. |

### Minimum effective method

Each step uses the lightest method that still does that step well. Steps may combine methods. Heavier methods need a reason the lighter one cannot do the job.

1. Deterministic code
2. Typesafe AI Jev decision making, using the 3-primitive structure
3. Local GPU
4. Subscription-based model, the lowest-cost model that does the job well
5. Pay-per-use tokens

| Level | Meaning |
| --- | --- |
| Missing | A heavier method or extra tokens are used where a lighter method would do the job. |
| Partial | Methods are named, but a heavier method has no reason the lighter one fails. |
| Met | Each step uses the lightest method that does that step well, and any heavier method is justified. |

### Repetition becomes a workflow

Work that repeats is captured as a workflow.

| Level | Meaning |
| --- | --- |
| Missing | The same task is redescribed from scratch each time. |
| Partial | A workflow exists, but the repeated task still needs a fresh full prompt. |
| Met | The repeated task runs as the workflow. The orchestrator supplies only that run's input. |

### Orchestrator handoff

The orchestrator delegates the next step. The handoff is the task plus paths to inputs.

| Level | Meaning |
| --- | --- |
| Missing | The orchestrator does the heavy work itself, or the handoff pastes source material that can be read from a path. |
| Partial | The work is delegated, but the handoff repeats content or keeps token-heavy work on the orchestrator. |
| Met | The next step is delegated. The handoff is instructions plus paths. |

## Evidence

The steps, the method chosen for each step, and the reason for any heavier method.

## Out of scope

Whether the workflow's subject-matter output is good. Which other grading methods apply to that output. The names of the 3 primitives, until they are written down.
