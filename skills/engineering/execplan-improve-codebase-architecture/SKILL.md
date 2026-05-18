---
name: execplan-improve-codebase-architecture
description: ExecPlan-aware architecture review and refactoring workflow for coupling, seams, locality, module depth, testability, and AI navigability. Use to find architecture candidates before scope is accepted, to author accepted refactor scope into EXECPLAN.md, or to implement only refactors already accepted by EXECPLAN.md.
---

# ExecPlan Improve Codebase Architecture

Use this skill to find and execute architecture improvements without turning
newly discovered friction into unapproved implementation scope.

## Core Invariant

Architecture candidates are not implementation scope until accepted in
`docs/execplans/<plan-name>/EXECPLAN.md`.

`CONTEXT.md` and `docs/adr/*` are durable planning memory. Use them to
understand language and prior decisions, not to derive hidden requirements.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md`.
- During discovery or authoring, read durable vocabulary when relevant: if
  `CONTEXT-MAP.md` exists, read it first and use it to find the relevant
  context file; else read root `CONTEXT.md` when present; else proceed without
  durable context.
- Read relevant `docs/adr/*` and existing `EXECPLAN.md` if present.
- During implementation, read `EXECPLAN.md` and `implementation-prompt.md`
  before reading broader code.
- Inspect callers, callees, tests, and entrypoints for any proposed seam.

## Allowed Writes

- Discovery: do not implement; return candidates.
- Authoring: update `EXECPLAN.md`; update durable docs only for accepted
  durable vocabulary or decisions.
- Handoff preparation: do not make new architecture decisions.
- Implementation: change code only for refactors accepted by `EXECPLAN.md`;
  update `progress.md`, `decision-log.md`, and
  `discoveries-retrospective.md`.
- Never: broaden a refactor because additional friction was discovered.

Architecture work may identify stable domain terms, but it does not create or
update `CONTEXT.md` directly unless the user explicitly asks. Route unresolved
or durable vocabulary through `execplan-grill-with-docs`.

## Architecture Vocabulary

Use these terms consistently:

- `Module`: anything with an interface and implementation.
- `Interface`: everything callers must know, including invariants, ordering,
  error modes, configuration, and performance.
- `Implementation`: what is inside the module.
- `Seam`: where behavior can vary without editing that place.
- `Adapter`: concrete thing satisfying an interface at a seam.
- `Depth`: leverage at the interface.
- `Locality`: change and verification concentrate in one place.

Avoid vague substitutions like component, service, API, or boundary when the
precise term is module, interface, seam, or adapter.

## ExecPlan Content Owned

For accepted architecture work, make sure `EXECPLAN.md` captures:

- purpose and non-goals
- accepted refactor scope
- behavior that must not change
- target seams, interfaces, adapters, or module boundaries
- files and callers likely affected
- domain language and ADR consequences
- implementation order
- validation checks for preserved behavior
- drift rules

## Discovery Output

During architecture discovery, do not design the new interface yet. First
return candidates only, then ask which candidate to explore or author into
`EXECPLAN.md`.

When reviewing architecture, return numbered candidates with:

- candidate name
- files and surfaces
- problem
- proposed direction
- expected depth or locality benefit
- behavior preservation risk
- validation strategy
- ADR or context conflicts
- recommendation: accept into ExecPlan, defer, or reject

Use these checks:

- Deletion test: if deleting the module removes little complexity, it is
  probably shallow.
- One adapter means a hypothetical seam; two adapters means a real seam.
- Test through the interface that callers use.
- Prefer replacing shallow tests with deeper interface tests once coverage
  exists.

## Implementation Rules

During implementation:

- preserve user-visible behavior unless `EXECPLAN.md` accepts behavior change
- keep refactor steps small enough for focused checks
- record newly found opportunities in `discoveries-retrospective.md`
- stop if the accepted seam does not exist or the code shape materially differs
  from the plan

## Stop Conditions

Stop when:

- the refactor would change unaccepted behavior
- implementation requires changing an ADR without user acceptance
- no viable validation check exists for preserved behavior
- the target architecture in `EXECPLAN.md` no longer matches the repository

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Candidates found or refactor slices completed
- Preserved behavior checks run, or not run with reason
- New opportunities deferred
- Drift or blockers
