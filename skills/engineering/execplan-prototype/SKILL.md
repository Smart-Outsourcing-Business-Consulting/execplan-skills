---
name: execplan-prototype
description: ExecPlan-aware throwaway prototyping for narrow logic, state, UI, data-shape, or design questions. Use only when EXECPLAN.md explicitly allows a prototype, names the question, location, run command, cleanup rule, and capture path; stop when prototypes would become hidden implementation scope.
---

# ExecPlan Prototype

Use this skill to answer one narrow question with throwaway code while keeping
the durable decision in the ExecPlan.

## Core Invariant

A prototype is throwaway code that answers one named question. It is not
implementation scope unless `EXECPLAN.md` explicitly says so.

`docs/execplans/<plan-name>/EXECPLAN.md` is the accepted executable
specification and implementation source of truth.

## Required Reads

- Always read repo-local instructions and the applicable `PLANS.md`.
- Read `EXECPLAN.md` before creating prototype files.
- During implementation, also read `implementation-prompt.md`.
- Read durable docs only to understand vocabulary or prior decisions relevant
  to the prototype question.

## Allowed Writes

- Authoring: update `EXECPLAN.md`; create prototype files only when the user or
  ExecPlan explicitly allows a spike.
- Handoff preparation: do not create prototypes.
- Implementation: create or modify prototype files only within the location and
  cleanup rule accepted by `EXECPLAN.md`; update `decision-log.md` or
  `discoveries-retrospective.md` with the result.
- Never: let prototype behavior become production behavior without an accepted
  plan update.

## ExecPlan Content Owned

Before prototype work starts, `EXECPLAN.md` must state:

- question the prototype answers
- whether prototype code is allowed
- prototype location
- run command
- allowed checks
- cleanup rule: delete, keep temporarily, or absorb
- capture path for the answer
- stop conditions

If any item is missing, ask to update `EXECPLAN.md` first.

## Branch Selection

- Logic or state question: build a tiny interactive terminal prototype.
- UI or design question: build several switchable variations.

Rules for both branches:

- one command to run
- no persistence by default
- no production polish
- no broad abstractions
- surface relevant state after each action or variant switch

## Capture Rules

The prototype answer is the durable output.

- During authoring, capture the answer in `EXECPLAN.md`, `CONTEXT.md`, or an
  ADR as appropriate.
- During implementation, capture the answer in `decision-log.md` or
  `discoveries-retrospective.md`, then delete or absorb the prototype according
  to `EXECPLAN.md`.

## Stop Conditions

Stop when:

- prototype permission, location, cleanup, or capture path is missing
- the prototype requires persistence, network access, credentials, or broad
  environment mutation not allowed by `EXECPLAN.md`
- the prototype starts becoming production implementation
- the result reveals a missing business or architecture decision

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- Prototype question
- Prototype path and run command, if created
- Answer learned
- Cleanup status
- Capture location
- Drift or blockers
