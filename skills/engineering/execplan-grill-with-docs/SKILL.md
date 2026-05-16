---
name: execplan-grill-with-docs
description: ExecPlan-aware grilling for ambiguous business intent, overloaded domain terms, edge cases, inverse behavior, durable decisions, CONTEXT.md updates, and ADR candidates. Use during ExecPlan authoring to turn clarified intent into EXECPLAN.md; use during implementation only to identify missing domain decisions and then stop.
---

# ExecPlan Grill With Docs

Use this skill to clarify intent and documentation without letting scattered
docs become hidden implementation scope.

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

- Authoring: update `EXECPLAN.md`; update `CONTEXT.md` only for accepted stable
  vocabulary; update or create ADRs only for accepted durable decisions.
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
- domain terms with actionable consequences
- edge cases and inverse or unapply behavior
- durable-doc consequences
- verification expectations
- stop conditions and blocking questions

## Process

1. Identify the ambiguity.
2. Inspect code or durable docs before asking the user.
3. Ask one focused question at a time when judgment is needed.
4. Include a recommended answer with the question.
5. Write each resolved implementation rule into `EXECPLAN.md`.
6. Write durable vocabulary to `CONTEXT.md` only when it should outlive this
   task.
7. Write ADRs only for hard-to-reverse, surprising tradeoff decisions.

## Durable Doc Rules

Use `CONTEXT.md` for stable domain language, not implementation helper names,
test names, or one-off task details.

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

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Questions resolved
- `EXECPLAN.md` updates made or needed
- Durable doc updates made or needed
- Drift or blockers
