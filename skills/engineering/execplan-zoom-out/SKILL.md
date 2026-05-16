---
name: execplan-zoom-out
description: ExecPlan-aware codebase mapping for unfamiliar modules, affected files, callers, data flow, current-state handoff, stale implementation prompts, and drift risks. Use during authoring to inform EXECPLAN.md, during handoff to refresh implementation-prompt.md, or during implementation to resolve orientation gaps without creating new scope.
---

# ExecPlan Zoom Out

Use this skill to go up one layer of abstraction before authoring, preparing,
or implementing an ExecPlan.

## Core Invariant

`docs/execplans/<plan-name>/EXECPLAN.md` is the accepted executable
specification and implementation source of truth.

`CONTEXT.md` and `docs/adr/*` are durable planning memory. Use their language
and decisions to orient the map, but do not derive hidden implementation scope
from them.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md`.
- During authoring, read `EXECPLAN.md` if it exists and inspect relevant code.
- During handoff preparation, read `EXECPLAN.md`, current repo state, and any
  existing `implementation-prompt.md`.
- During implementation, read `EXECPLAN.md` and `implementation-prompt.md`
  before using the map to fill orientation gaps.
- Read durable docs only for vocabulary and prior decisions relevant to the
  map.

## Allowed Writes

- Authoring: update `EXECPLAN.md` only with implementation-relevant map
  conclusions accepted into scope.
- Handoff preparation: update `implementation-prompt.md` with current-state
  facts only.
- Implementation: update implementation-state files only if the map reveals
  progress, decisions, discoveries, or drift; do not change code from this
  skill alone.
- Never: convert newly discovered files or ideas into hidden scope.

## ExecPlan Content Owned

During authoring, the map may inform these `EXECPLAN.md` areas:

- affected surfaces
- implementation strategy
- expected files and entrypoints
- accepted seams or boundaries
- verification policy
- drift rules

Current repository state that may change before implementation belongs in
`implementation-prompt.md`, not stable `EXECPLAN.md` scope.

## What To Produce

Give a concise map of:

- relevant domain concepts using `CONTEXT.md` vocabulary when available
- relevant modules, commands, routes, models, jobs, UI surfaces, or scripts
- important callers and callees
- data or control flow at the level needed for the task
- stable seams and boundaries
- likely affected files or components
- current-state facts for `implementation-prompt.md`
- contradictions between code, durable docs, and the active ExecPlan

## Stop Conditions

Stop when:

- the map reveals missing scope or a plan contradiction
- handoff preparation requires a new product, architecture, schema, security,
  or scope decision
- implementation would need files or behavior not accepted in `EXECPLAN.md`
- durable docs contradict the accepted plan

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Map
- Affected surfaces
- Current-state facts
- Recommended plan or handoff updates
- Drift or blockers
