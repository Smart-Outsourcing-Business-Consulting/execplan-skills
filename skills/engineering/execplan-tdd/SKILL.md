---
name: execplan-tdd
description: ExecPlan-aware test-driven development for accepted behavior changes. Use during ExecPlan authoring to define public behavior slices, verification seams, allowed checks, and test gaps; use during implementation for one vertical red-green-refactor slice at a time within EXECPLAN.md scope.
---

# ExecPlan TDD

Use this skill to implement accepted behavior through focused feedback loops.

## Core Invariant

`docs/execplans/<plan-name>/EXECPLAN.md` is the accepted executable
specification and implementation source of truth.

Run only checks allowed by `EXECPLAN.md`, `implementation-prompt.md`,
repository-local instructions, or explicit user instruction.

`CONTEXT.md` and `docs/adr/*` are durable planning memory. They are not hidden
implementation scope. If they conflict with `EXECPLAN.md`, stop and report
drift.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md`.
- During authoring, read `EXECPLAN.md` if it exists and inspect likely public
  behavior seams.
- During implementation, read `EXECPLAN.md` and `implementation-prompt.md`
  before editing.
- Read durable docs only for domain language and accepted decisions already
  summarized in `EXECPLAN.md`.

## Allowed Writes

- Authoring: update `EXECPLAN.md` with behavior slices and verification policy.
- Handoff preparation: do not write tests or implementation.
- Implementation: write tests and code only for accepted behavior slices;
  update `progress.md`, `decision-log.md`, and
  `discoveries-retrospective.md`.
- Never: add unaccepted behavior because it is convenient to test.

## ExecPlan Content Owned

Make sure `EXECPLAN.md` identifies:

- public behavior to verify
- public interfaces or user-visible flows that expose behavior
- behavior slices and priority order
- what is required in this pass
- what is explicitly deferred
- what is only a design note or possible later semantic
- critical paths and edge cases
- checks allowed without permission
- checks requiring explicit permission
- checks that must not be run
- known test gaps and why they are acceptable

If no viable verification strategy exists, stop and ask to revise the ExecPlan
before implementation.

For behavior work, `implementation-prompt.md` must name the next slice or
slice group and require: red check, green check, lifecycle update, and stop
before the next slice unless the user explicitly says to continue. A broad
"implement this plan" prompt is not sufficient for behavior work.

## Test Philosophy

Tests should verify behavior through public interfaces, not implementation
details.

Avoid tests that:

- mock internal collaborators
- assert private methods, call order, or implementation shape
- query storage directly when a public interface can observe behavior
- fail on a refactor that preserves behavior

Mock only at system boundaries such as external APIs, time, randomness, file
systems, or databases when a real boundary is impractical.

## Slice Granularity

Use the named behavior slices in `EXECPLAN.md`. A slice is a coherent behavior
group, not one enum value, signal name, branch, field, or tiny helper method.

Do not split one accepted behavior group into separate stops for every small
case unless the ExecPlan or user explicitly asks for that.

### Self-Contained Example

If `EXECPLAN.md` says:

- Slice 2: Download selection
- Accepted behavior: emit signals for no candidate source, outside window,
  generation imminent, active attempt, retry exhausted, expired before
  download, and malformed candidate payload

Then the TDD slice is **Download selection**, not seven separate slices.

A good loop is:

1. Add focused tests for several representative download-selection outcomes.
2. Confirm the grouped red run fails for the expected missing behavior.
3. Implement the minimal source hooks for the accepted download-selection
   outcomes.
4. Run the same focused check.
5. Fix edge cases discovered inside that same slice, such as inventory sources
   affecting no-candidate-source.
6. Stop before the next ExecPlan slice, such as Dispatch.

It is acceptable for a grouped slice red run to have multiple expected
failures. Red/green applies to the accepted behavior group, not necessarily to
one assertion, one signal, or one branch.

Split smaller only when:

- the behavior group is too large to verify in one focused check
- failures cannot be understood together
- implementation touches unrelated ownership areas
- the ExecPlan explicitly subdivides the group
- the user asks to stop after each individual behavior

## Implementation Workflow

Use vertical slices. Do not write all tests first and all implementation later.

For each behavior slice:

1. Pick the next accepted behavior group from `EXECPLAN.md`.
2. Write one failing focused check or a grouped focused check for that slice.
3. Confirm it fails for the expected reason.
4. Write the smallest implementation that makes it pass.
5. Run the focused check.
6. Update `progress.md`.
7. Fix edge cases discovered inside the same slice when they belong to the
   same behavior group and ownership area.
8. Stop before the next accepted slice unless the user explicitly asks you to
   continue.

After green, refactor only while checks are passing. Record material test seam
decisions in `decision-log.md` and testability discoveries in
`discoveries-retrospective.md`.

## Lifecycle Vocabulary

Use precise lifecycle labels:

- `Implemented`: accepted behavior completed in this pass.
- `Deferred by accepted scope`: explicitly out of this pass.
- `Discovered optional future work`: useful later context, not required scope.
- `Blocked`: user decision, permission, or missing seam prevents completion.
- `Out of scope`: must not be implemented in this pass.

Do not call optional future semantics "remaining gaps" unless the active
ExecPlan explicitly makes them required for completion.

## Stop Conditions

Stop when:

- a correct public behavior seam does not exist
- the only available test would verify implementation details
- the required check is not allowed
- passing the test would require unaccepted scope
- observed behavior contradicts the accepted ExecPlan

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Behavior slices implemented
- Checks run and results
- Checks not run and why
- Remaining test gaps
- Drift or blockers
