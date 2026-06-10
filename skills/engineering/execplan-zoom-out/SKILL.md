---
name: execplan-zoom-out
description: ExecPlan-aware current-state mapping for stale or missing implementation prompts, dirty-worktree guardrails, unfamiliar affected files, or suspected drift. Use during handoff only when implementation-prompt.md needs repository-state facts that are not already captured, or when the working tree may have changed. Do not use merely because an accepted EXECPLAN.md is being implemented.
---

# ExecPlan Zoom Out

Use this skill to map current repository state when authoring needs affected
surfaces, handoff preparation needs volatile state facts, or implementation
needs orientation inside accepted scope.

## When Not To Use

Do not use this skill to re-derive accepted scope from an already complete
`EXECPLAN.md`.

During handoff preparation, use this skill only to capture volatile
current-state facts such as dirty worktree guardrails, stale or missing
`implementation-prompt.md`, changed files since authoring, unclear entrypoints,
or suspected drift.

If `EXECPLAN.md` is accepted, `implementation-prompt.md` is current, the next
slice is clear, and no repository-state drift is suspected, skip this skill and
proceed with the lifecycle rules in `PLANS.md`.

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
- Read durable vocabulary only for terms relevant to the map: if
  `CONTEXT-MAP.md` exists, read it first and use it to find the relevant
  context file; else read root `CONTEXT.md` when present; else proceed without
  durable context.
- Read ADRs and discovery notes only for prior decisions relevant to the map.

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
- dirty-worktree exclusions and "never touch/stage" paths
- verification policy
- drift rules

Current repository state that may change before implementation belongs in
`implementation-prompt.md`, not stable `EXECPLAN.md` scope.

During handoff preparation, put dirty-worktree guardrails near the top of
`implementation-prompt.md`:

- exact files the worker may edit or stage
- exact files and directories the worker must not touch or stage
- unrelated tracked diffs to preserve
- unrelated untracked files to ignore
- checks allowed without permission and checks requiring permission

Do not rely on a worker rereading scattered status notes to rediscover these
boundaries.

For behavior work, the handoff prompt must also prevent broad one-pass
implementation:

- say "do not implement the whole plan in one pass"
- name the next accepted slice or slice group
- require the TDD loop for that slice: red check, green check, lifecycle
  update, then stop before the next slice unless the user explicitly says to
  continue
- allow grouped red failures inside the slice when the failures belong to the
  same accepted behavior group
- require implementation workers to report if red-first sequencing was not
  preserved

## What To Produce

Give a concise map of:

- relevant domain concepts using `CONTEXT.md` vocabulary when available
- relevant modules, commands, routes, models, jobs, UI surfaces, or scripts
- important callers and callees
- data or control flow at the level needed for the task
- stable seams and boundaries
- likely affected files or components
- current-state facts for `implementation-prompt.md`
- "never touch/stage" dirty-worktree guardrails when preparing handoff
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
