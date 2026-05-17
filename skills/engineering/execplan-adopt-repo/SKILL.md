---
name: execplan-adopt-repo
description: ExecPlan-native repository setup and adoption for new or existing repos. Use to create or review repo-local AGENTS.md, choose docs/execplans layout, optionally seed CONTEXT.md or docs/adr only from evidence-backed durable terms/decisions, and align a repo with the ExecPlan harness without issue-tracker labels or old triage setup.
---

# ExecPlan Adopt Repo

Use this skill to make a repository ready for the ExecPlan harness without
creating hidden workflow dependencies.

## Core Invariant

Repo adoption prepares the repository for future ExecPlan work. It is not
product implementation, issue-tracker setup, or a substitute for an accepted
`EXECPLAN.md`.

`AGENTS.md` should contain project context, safety rules, accepted checks, and
repo-specific boundaries. It should not redefine the ExecPlan lifecycle.

`CONTEXT.md`, `docs/adr/*`, and `doc/discovery/*` are durable background
memory. Create or update them only when there is evidence-backed language or a
durable decision that should outlive one task.

## Required Reads

- Read existing repo-local instructions such as `AGENTS.md`, `CLAUDE.md`, or
  equivalent files.
- Read `README.md`, package or project metadata, and docs that explain domain,
  runtime, checks, deployment, or safety constraints.
- Inspect top-level directories and important module, command, model, route,
  job, schema, migration, test, and UI-surface names.
- Read existing `CONTEXT.md`, `docs/adr/*`, `doc/discovery/*`, and
  `docs/execplans/*` when present.
- Do not run tests, installs, network commands, migrations, or application
  commands unless the user explicitly asks.

## Allowed Writes

- New or existing repo adoption: create or update repo-local `AGENTS.md`.
- Optional durable memory: create or update `CONTEXT.md` only with stable
  domain language; create or update ADRs only for accepted durable decisions.
- Optional planning directories: create `docs/execplans/` or `docs/adr/` only
  when useful for the repo.
- Never: create `implementation-prompt.md`, `progress.md`,
  `decision-log.md`, or `discoveries-retrospective.md` during adoption.
- Never: create an ExecPlan unless the user also asks to author a specific
  plan.

## Adoption Outputs Owned

`AGENTS.md` should stay lean and project-specific:

- project context
- framework, runtime, package, and deployment notes
- safety rules
- allowed focused checks
- checks requiring explicit permission
- generated files, secrets, migrations, external services, and production data
  boundaries
- statement that substantial work uses `docs/execplans/<plan-name>/`
- statement that lifecycle rules come from the shared ExecPlan harness

`CONTEXT.md` should be a durable domain glossary, not a repo summary.

Good entry:

```md
**Invoice run**:
A scheduled billing operation that groups payable invoice items into one
processing batch.
_Avoid_: payment job, billing sweep
```

Bad entry:

```md
**Controller**:
A class that handles HTTP requests.
```

## Context Seeding Rules

Do not create an empty `CONTEXT.md` by default.

Create or update `CONTEXT.md` only when stable domain language is evident from
at least one of:

- user-facing docs
- existing ExecPlans
- database or domain models
- repeated business nouns and verbs in modules, commands, tests, or UI labels
- accepted user explanation during the session

For each candidate term, record the evidence source mentally before writing.
If the meaning is ambiguous or important, ask the user before making it
durable.

If repo convention requires `CONTEXT.md` to exist but no durable terms are
evident, create a minimal file:

```md
# Context

No stable domain terms have been recorded yet.
```

## Process

1. Classify the task as new-repo setup, existing-repo adoption, or repo-local
   instruction review.
2. Inspect only enough code and docs to identify project context, safety rules,
   checks, and durable vocabulary candidates.
3. Draft or revise `AGENTS.md` so it is project-specific and defers lifecycle
   semantics to the shared ExecPlan harness.
4. Decide whether `CONTEXT.md` should be absent, created minimally, or seeded
   with evidence-backed terms.
5. Decide whether `docs/adr/` or `docs/execplans/` should be created.
6. Stop and ask before recording ambiguous business terms or durable
   decisions.

## Stop Conditions

Stop when:

- repo-local instructions intentionally conflict with the shared harness
- safety rules cannot be inferred and mistakes would be high-risk
- creating `CONTEXT.md` would require guessing domain meaning
- adoption would require running unapproved commands
- the user request drifts into authoring or implementing a specific ExecPlan

## Output Template

End with:

- Phase assumed
- Files read
- Files changed
- `AGENTS.md` summary
- `CONTEXT.md` action: absent, unchanged, created minimal, or seeded
- ADR or ExecPlan directories created, if any
- Checks run, or not run with reason
- Drift or blockers
