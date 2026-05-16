---
name: execplan-diagnose
description: ExecPlan-aware diagnosis for bugs, regressions, flaky behavior, and performance issues. Use during ExecPlan authoring to capture reproduction, feedback loop, instrumentation bounds, allowed checks, and regression strategy in EXECPLAN.md; use during implementation only inside accepted diagnostic scope.
---

# ExecPlan Diagnose

Use this skill to turn unclear failures into an evidence-backed diagnosis loop
inside the ExecPlan harness.

## Core Invariant

`docs/execplans/<plan-name>/EXECPLAN.md` is the accepted executable
specification and implementation source of truth.

`CONTEXT.md` and `docs/adr/*` are durable planning memory. They are not hidden
implementation scope. If they conflict with `EXECPLAN.md`, stop and report
drift.

Run only checks allowed by `EXECPLAN.md`, `implementation-prompt.md`,
repository-local instructions, or explicit user instruction.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md`.
- During authoring, read the target `EXECPLAN.md` if it exists.
- During handoff or implementation, read `EXECPLAN.md` and
  `implementation-prompt.md`.
- Read relevant `CONTEXT.md`, `docs/adr/*`, and `doc/discovery/*` only as
  background for domain language and prior decisions.

## Allowed Writes

- Authoring: update `EXECPLAN.md`; update durable docs only for accepted
  durable vocabulary or decisions.
- Handoff preparation: update `implementation-prompt.md` only if diagnosing
  current-state inconsistency is explicitly part of handoff.
- Implementation: update code only within accepted diagnostic scope; keep
  `progress.md`, `decision-log.md`, and `discoveries-retrospective.md`
  current.
- Never: silently expand implementation scope, add unapproved checks, or turn
  durable docs into new requirements.

## ExecPlan Content Owned

For bug or performance work, make sure `EXECPLAN.md` captures:

- user-visible symptom
- expected correct behavior
- reproduction steps, or the exact missing information
- feedback loop to build or run
- allowed checks and checks requiring permission
- instrumentation bounds and cleanup rules
- likely affected surfaces
- regression check strategy, or why no correct regression seam exists
- stop conditions for missing access, missing artifacts, non-determinism, or
  unsafe checks

If no feedback loop can be built or authorized, record the blocker in
`EXECPLAN.md` and do not create an implementation handoff.

## Implementation Workflow

1. Build the accepted feedback loop.
2. Reproduce the failure.
3. Form ranked falsifiable hypotheses.
4. Instrument one hypothesis at a time.
5. Fix only the accepted cause.
6. Add or run the accepted regression check.
7. Clean up temporary instrumentation.

Record material hypothesis changes, plan-compatible decisions, and blockers in
`decision-log.md`. Record evidence-backed discoveries and final cause in
`discoveries-retrospective.md`.

## Stop Conditions

Stop instead of guessing when:

- the observed failure differs materially from the described failure
- the required feedback loop is not allowed
- useful diagnosis requires external access not granted by the user
- the fix would require scope not accepted in `EXECPLAN.md`
- code reality contradicts durable docs in a way not summarized in
  `EXECPLAN.md`

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Reproduction result
- Proven or leading hypothesis
- Checks run, or not run with reason
- Drift or blockers
