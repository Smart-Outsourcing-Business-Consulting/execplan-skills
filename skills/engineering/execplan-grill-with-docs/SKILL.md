---
name: execplan-grill-with-docs
description: ExecPlan-aware grilling for ambiguous business intent, overloaded domain terms, edge cases, inverse behavior, durable decisions, CONTEXT.md updates, and ADR candidates. Use during ExecPlan authoring to turn clarified intent into EXECPLAN.md; use during implementation only to identify missing domain decisions and then stop.
---

# ExecPlan Grill With Docs

Use this skill to clarify intent and documentation without letting scattered
docs become hidden implementation scope.

This is an adversarial readiness check. Its job is to try to disprove that the
plan is ready, not to make the plan look ready.

## Core Invariant

`docs/execplans/<plan-name>/EXECPLAN.md` is the accepted executable
specification and implementation source of truth.

`CONTEXT.md` and `docs/adr/*` are durable planning memory. They can inform
authoring, but they are not hidden implementation scope.

If durable docs conflict with or add requirements beyond `EXECPLAN.md`, stop
and report drift. Do not reinterpret the task from those docs.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md`.
- Read the target `EXECPLAN.md` if it exists.
- Read relevant `CONTEXT.md`, `docs/adr/*`, and `doc/discovery/*` when they
  affect vocabulary, business rules, or prior decisions.
- Inspect code when a question can be answered from the repository.

## Allowed Writes

- Authoring: update `EXECPLAN.md`; update or lazily create `CONTEXT.md` only
  for accepted stable vocabulary; update or create ADRs only for accepted
  durable decisions.
- Handoff preparation: do not use this skill except to report a newly found
  missing decision.
- Implementation: do not change code or plan scope through this skill. If a
  domain rule is missing, stop and report the blocker.
- Never: leave implementation-relevant requirements only in chat, ADRs,
  `CONTEXT.md`, discovery notes, issue comments, or prototypes.

## ExecPlan Content Owned

Make sure every resolved implementation-relevant point is represented in
`EXECPLAN.md`:

- business objective
- accepted scope and non-goals
- what this plan owns, and what adjacent plans own
- what is required in this pass
- what is explicitly deferred
- what is only a design note or possible later semantic
- domain terms with actionable consequences
- mechanism-vs-catalogue boundaries, when one plan provides reusable mechanism
  and another plan provides concrete operational cases
- interface contracts and architecture boundaries
- edge cases and inverse or unapply behavior
- durable-doc consequences
- verification expectations
- stop conditions and blocking questions

## Mandatory Adversarial Inventory

Before declaring "no blocking questions", build an inventory of material
business rules, naming rules, scope boundaries, lifecycle rules, interface
contracts, verification claims, and durable-doc consequences.

Classify every item in a table:

```md
| Rule or decision | Evidence source | Explicit or inferred? | Needs user decision? | Recommended answer | Consequence if wrong |
| --- | --- | --- | --- | --- | --- |
```

Use `Explicit or inferred?` to prevent guesses from becoming accepted rules.
Use `Needs user decision?` to prevent silently resolving business semantics.
Use `Consequence if wrong` to force failure-mode thinking.

The agent must actively try to disprove readiness. It may only say "no
blocking questions" after every material business rule, naming rule, scope
boundary, lifecycle rule, and interface contract has been classified as
explicitly accepted, evidence-answerable, non-blocking, or intentionally
deferred.

If a rule is inferred and implementation would rely on it, it is a blocking
question unless the user explicitly accepts it or it is rewritten as deferred
non-scope.

## Readiness Checks

Before handoff or implementation readiness, verify that `EXECPLAN.md` separates:

- required behavior for this pass
- explicitly deferred behavior
- optional design notes and possible future semantics
- owned mechanism versus concrete catalogue, if applicable
- adjacent-plan ownership and non-goals
- lifecycle terms such as implemented, deferred, optional future work, blocked,
  and out of scope

Do not let "resolve when", "later", "eventually", "should", "may", or
"future" language become implementation scope unless the ExecPlan explicitly
lists it under required behavior for this pass.

## Process

1. Identify the ambiguity.
2. Inspect code or durable docs before asking the user.
3. Build the mandatory adversarial inventory table.
4. Ask one focused question at a time when judgment is needed.
5. Include a recommended answer with the question.
6. Write each resolved implementation rule into `EXECPLAN.md`.
7. Write durable vocabulary to `CONTEXT.md` only when it should outlive this
   task.
8. Write ADRs only for hard-to-reverse, surprising tradeoff decisions.

## Durable Doc Rules

Use `CONTEXT.md` for stable domain language, not implementation helper names,
test names, or one-off task details.

This skill is the producer of `CONTEXT.md` vocabulary during ExecPlan
authoring. Consumer skills may read `CONTEXT.md` when it exists, but they
should not create it as a side effect of implementation, handoff, diagnosis, or
codebase mapping.

Create `CONTEXT.md` lazily. If the repository has no `CONTEXT.md` or
`CONTEXT-MAP.md`, create a root `CONTEXT.md` only when the grilling process
resolves the first stable project-specific domain term that should outlive the
current plan. Do not create an empty placeholder.

Do not add `CONTEXT.md` entries for:

- generic programming, framework, or testing terms
- implementation helper, service, test, or fixture names
- one-off plan decisions that belong only in `EXECPLAN.md`
- optional future semantics that are not accepted vocabulary
- scope boundaries whose only consequence is this plan's included or excluded
  behavior

For every accepted durable term, record both the term and its implementation
consequence. Also summarize that consequence in `EXECPLAN.md` when the current
plan depends on it.

Use ADRs sparingly. All three should be true:

- hard to reverse
- surprising without context
- result of a real tradeoff

When a durable doc matters to the task, summarize its implementation
consequence in `EXECPLAN.md`.

## Stop Conditions

Stop when:

- a blocking business rule remains unresolved
- durable docs conflict with the accepted plan
- the user rejects a scope boundary required for a coherent plan
- the plan would require hidden assumptions to become executable
- the plan mixes required behavior with optional future semantics in a way an
  implementation worker could treat as required scope
- the plan fails to distinguish mechanism ownership from catalogue ownership
  where that distinction affects implementation

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Adversarial inventory table and classifications
- Questions asked and resolved
- `EXECPLAN.md` updates made or needed
- Durable doc updates made or needed
- Drift or blockers
