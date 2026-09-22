# template.md

A reference of premade templates. A user does not reteach a model the characteristics a final product is designed to. The AI doing the work loads the template for the task. A person, including a CEO-level architect, can read the same files and see what the product is held to.

## What this is

Categorized templates for how a final product is designed: structure, grading, and workflows. Each file is one scope. Load the file the task needs. Leave the others unloaded.

## Why you need it

The characteristics of a finished product should be stated once and reused. Without this repo, every session starts by explaining those characteristics again. The model then drifts, and two runs of the same product are not held to the same bar.

## What it replaces

Teaching the model again, in each conversation, which characteristics the final product is designed to have.

## How to use it

1. Find the category for the task. The index below is the map.
2. Load that template. Fill its slots for this product. Do not paste unrelated templates into the same context.
3. Grade only by the methods named in **How you are graded**. A rubric is one method. Cite `rubrics/<id>.md` only when a method uses that rubric, and load that file then.
4. For a workflow, follow [rubrics/workflow-design.md](rubrics/workflow-design.md). Each step is input, process, or output. Use the lightest method that does the step well:
   1. Deterministic code
   2. TypeSafe Jev, using Choice, Score, and Noul
   3. Local GPU
   4. The lowest-cost subscription model that does the job well
   5. Pay-per-use tokens

Jev is TypeSafe's System One model. It answers typed questions about a state. Choice picks one option from a list. Score places the state on ordered levels. Noul is the probability that a statement is true. The official reference is [TypeSafe primitives](https://docs.typesafe.ai/primitives.md).

## Ideas

- [ideas/draft-idea.md](ideas/draft-idea.md) — idea-draft section template

## Rubrics

- [rubrics/index.md](rubrics/index.md) — catalog of scoped rubrics. Load a rubric only when its scope matches the work.
- [rubrics/rubric.md](rubrics/rubric.md) — template for one rubric scope
- [rubrics/workflow-design.md](rubrics/workflow-design.md) — rubric for designing a workflow
- [rubrics/autonomous-worker-node.md](rubrics/autonomous-worker-node.md) — code-and-Jev autonomous worker node
- [rubrics/deterministic-code.md](rubrics/deterministic-code.md) — framework for the deterministic-code method
- [rubrics/local-gpu.md](rubrics/local-gpu.md) — framework for the local-GPU method
- [rubrics/subscription-model.md](rubrics/subscription-model.md) — framework for the subscription-model method
- [rubrics/pay-per-use.md](rubrics/pay-per-use.md) — framework for the pay-per-use method
