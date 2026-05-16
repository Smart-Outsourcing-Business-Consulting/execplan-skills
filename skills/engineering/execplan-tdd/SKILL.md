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
- critical paths and edge cases
- checks allowed without permission
- checks requiring explicit permission
- checks that must not be run
- known test gaps and why they are acceptable

If no viable verification strategy exists, stop and ask to revise the ExecPlan
before implementation.

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

## Implementation Workflow

Use vertical slices. Do not write all tests first and all implementation later.

For each behavior slice:

1. Pick one accepted behavior from `EXECPLAN.md`.
2. Write one failing test or equivalent allowed check.
3. Confirm it fails for the expected reason.
4. Write the smallest implementation that makes it pass.
5. Run the focused check.
6. Update `progress.md`.
7. Repeat only for the next accepted behavior.

After green, refactor only while checks are passing. Record material test seam
decisions in `decision-log.md` and testability discoveries in
`discoveries-retrospective.md`.

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
