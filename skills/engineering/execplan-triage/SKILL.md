---
name: execplan-triage
description: ExecPlan-aware intake and classification for vague ideas, bug reports, architecture concerns, rejected work, mixed requests, and unclear next steps. Use to choose direct small edit, ExecPlan authoring, handoff preparation, implementation, diagnosis, prototype, architecture review, or out-of-scope before writing or changing code.
---

# ExecPlan Triage

Use this skill to classify work before it becomes an implementation contract.

## Core Invariant

Do not create a separate agent brief as an implementation contract.

When work is ready for an agent, capture the accepted executable specification
in `docs/execplans/<plan-name>/EXECPLAN.md`.

Issue bodies, comments, chat history, `CONTEXT.md`, ADRs, and discovery notes
are planning inputs, not hidden implementation scope.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md` if the
  request may enter the ExecPlan workflow.
- Read durable vocabulary only enough to classify the request: if
  `CONTEXT-MAP.md` exists, read it first and use it to find the relevant
  context file; else read root `CONTEXT.md` when present; else proceed without
  durable context.
- Read relevant `docs/adr/*` and `doc/discovery/*` only enough to classify the
  request.
- Inspect code only enough to classify confidently.
- Read an existing `EXECPLAN.md` when the request references one.

## Allowed Writes

- Classification: no writes required.
- Authoring handoff: may create or update `EXECPLAN.md` only when the request
  is ready for ExecPlan authoring.
- Handoff preparation: may recommend refreshing `implementation-prompt.md`; do
  not write it unless the user asked for handoff preparation.
- Implementation: do not implement from triage.
- Never: create a parallel implementation contract outside `EXECPLAN.md`.

## States

Classify the request into one state:

- `direct-small-edit`
- `needs-clarification`
- `needs-diagnosis`
- `needs-grilling`
- `needs-architecture-review`
- `needs-prototype`
- `ready-for-execplan`
- `ready-for-handoff`
- `ready-for-implementation`
- `out-of-scope`

Categories:

- `bug`
- `enhancement`
- `architecture`
- `cleanup`
- `documentation`
- `out-of-scope`

## Routing Rules

- Bug or regression without repro: `needs-diagnosis`.
- Fuzzy business behavior: `needs-grilling`.
- Mixed mechanism and catalogue ownership: `needs-grilling`.
- A request that might turn optional future semantics into required work:
  `needs-grilling`.
- Coupling, seams, locality, or testability concern: `needs-architecture-review`.
- Narrow logic, state, UI, or design uncertainty: `needs-prototype`.
- Accepted plan without fresh current-state snapshot: `ready-for-handoff`.
- Accepted plan with fresh handoff: `ready-for-implementation`.
- Clear narrow change that does not need the harness: `direct-small-edit`.

## ExecPlan Content Owned

When triage reaches `ready-for-execplan`, identify the plan path and the
minimum accepted scope to author. Do not invent detailed implementation steps
unless the user asked you to author the ExecPlan.

Also identify whether the task is:

- a reusable mechanism
- a concrete catalogue of cases/events/rules
- both, requiring an explicit boundary

If that boundary is unclear, route to `needs-grilling` before authoring or
handoff.

## Out Of Scope

Use `out-of-scope` as a classification outcome when the requested work should
not proceed. Do not create a durable rejection record unless the target
repository already defines one and the user asks you to update it.

If an out-of-scope decision affects future implementation, summarize the
implementation-relevant consequence in `EXECPLAN.md` when that task arises.

## Stop Conditions

Stop when:

- classification depends on a user business decision
- the request conflicts with accepted scope or durable rejection
- implementation would begin before the correct phase is selected
- a referenced ExecPlan is stale, blocked, or missing

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Category
- State
- Recommended next skill or phase
- ExecPlan path, if applicable
- Drift or blockers
