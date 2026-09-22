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

A name carries three numbers: `folder.group.file`.

- `folder` is the category. `0` is the repo root. `1` is `ideas/`. `2` is `rubrics/`.
- `group` is the subfolder, or `0` when the file sits directly in the category.
- `file` is the file within that group. `0` is the index or the parent of the group. `1` and up are the other files, in the order they are used.

`0.0.0` is this file. It is the root of the scheme: folder 0, group 0, file 0.

## The filename

`{type}-{folder}.{group}.{file}-{slug}.md`

- `type` is the kind of file. It is the first word. A grading file is `rubric`. A section template is `template`.
- `slug` is the stable short name. It matches the `Id` inside the file.
- The `Id` does not change when the number changes. Cite the filename, not `rubrics/<id>.md`.

## Assigned names

| Number | File |
| --- | --- |
| 0.0.0 | rubric-0.0.0-filename-conventions.md |
| 1.0.1 | ideas/template-1.0.1-draft-idea.md |
| 2.0.0 | rubrics/rubric-2.0.0-index.md |
| 2.0.1 | rubrics/rubric-2.0.1-template.md |
| 2.1.0 | rubrics/rubric-2.1.0-workflow-design.md |
| 2.1.1 | rubrics/rubric-2.1.1-deterministic-code.md |
| 2.1.2 | rubrics/rubric-2.1.2-autonomous-worker-node.md |
| 2.1.3 | rubrics/rubric-2.1.3-local-gpu.md |
| 2.1.4 | rubrics/rubric-2.1.4-subscription-model.md |
| 2.1.5 | rubrics/rubric-2.1.5-pay-per-use.md |

Group `2.1` is the workflow. `2.1.0` is the parent. `2.1.1` through `2.1.5` follow the method order: deterministic code, code and Jev, local GPU, subscription model, pay-per-use tokens.

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
| Missing | The filename has no `folder.group.file` number. |
| Partial | The number is present but the file is in a different folder or group than the number says. |
| Met | The number matches this rubric, and the file is in the folder that number names. |

## Evidence

The filename, the folder it is in, and the `Id` inside the file.

## Out of scope

Whether the file's own slots are filled in. `README.md` and `LICENSE`.
