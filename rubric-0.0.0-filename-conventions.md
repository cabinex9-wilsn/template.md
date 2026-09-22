# Filename conventions

The main rule for names in this repo. Number `0.0.0`.

## Id

filename-conventions

## Scope

How a file is named so its type and its place in the tree are visible at the start of the filename.

## Load when

Adding a file, renaming a file, or citing a file.

## Do not load when

Grading the subject of a template. Load that template's own rubric.

## Applies to

Every markdown file in this repo except `README.md` and `LICENSE`. Those two names stay fixed.

## The number

Each added number is a level deeper.

- A shorter number is the parent.
- A longer number that starts with the parent is inside it.
- `2` is the rubrics catalog. `2.1` is the workflow rubric inside it. `2.1.2` is a file inside that workflow.
- Do not add `.0` to make a parent. Same length means peers. `2.1.0` and `2.1.2` would be peers. `2.1` and `2.1.2` are parent and child.

`0.0.0` is this file. It has no children. It is not a parent of the other files.

## The filename

`{type}-{number}-{slug}.md`

- `type` is the kind of file. It is the first word. A grading file is `rubric`. A section template is `template`.
- `number` is the place in the tree, as above.
- `slug` is the stable short name. It matches the `Id` inside the file.
- The `Id` does not change when the number changes. Cite the filename, not `rubrics/<id>.md`.

## Assigned names

| Number | File | Inside |
| --- | --- | --- |
| 0.0.0 | rubric-0.0.0-filename-conventions.md | repo root |
| 1.1 | ideas/template-1.1-draft-idea.md | ideas |
| 2 | rubrics/rubric-2-index.md | rubrics |
| 2.0 | rubrics/rubric-2.0-template.md | the rubric catalog |
| 2.1 | rubrics/rubric-2.1-workflow-design.md | the rubric catalog |
| 2.1.1 | rubrics/rubric-2.1.1-deterministic-code.md | the workflow |
| 2.1.2 | rubrics/rubric-2.1.2-autonomous-worker-node.md | the workflow |
| 2.1.3 | rubrics/rubric-2.1.3-local-gpu.md | the workflow |
| 2.1.4 | rubrics/rubric-2.1.4-subscription-model.md | the workflow |
| 2.1.5 | rubrics/rubric-2.1.5-pay-per-use.md | the workflow |

`2.1.1` through `2.1.5` follow the method order: deterministic code, code and Jev, local GPU, subscription model, pay-per-use tokens.

## Criteria

### Type first

| Level | Meaning |
| --- | --- |
| Missing | The filename does not start with its type. |
| Partial | The type is present but not the first word, or it does not match the file. |
| Met | The filename starts with `rubric` or `template`, and that word matches the file. |

### Place in the tree

| Level | Meaning |
| --- | --- |
| Missing | The number does not show what the file is inside. |
| Partial | The number is one segment longer or shorter than its real parent, or a trailing `.0` is used to mean parent. |
| Met | The parent has the shorter number. This file adds one segment. An AI can see the subset without loading another file. |

## Evidence

The filename, the folder it is in, and the `Id` inside the file.

## Out of scope

Whether the file's own slots are filled in. `README.md` and `LICENSE`.
