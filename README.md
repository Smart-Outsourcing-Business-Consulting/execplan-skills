# ExecPlan Harness Skills

This is a fork of `mattpocock/skills` adapted for a Codex ExecPlan harness.
The harness contract is summarized here so the repository is portable across
developer machines and teams.

For repositories that use this harness,
`docs/execplans/<plan-name>/EXECPLAN.md` is the accepted executable
specification and implementation source of truth.

`implementation-prompt.md` is a per-pass current-state handoff snapshot.
`progress.md`, `decision-log.md`, and `discoveries-retrospective.md` are
implementation-state files for continuity between agents.

`CONTEXT.md` and `docs/adr/*` are durable planning memory. They can inform
ExecPlan authoring, but they are not hidden implementation scope. Any
implementation-relevant consequence from those documents must be summarized in
`EXECPLAN.md`.

## Active Skills

Install and use only these ExecPlan-aware engineering skills for the harness:

- [`execplan-grill-with-docs`](./skills/engineering/execplan-grill-with-docs/SKILL.md)
- [`execplan-diagnose`](./skills/engineering/execplan-diagnose/SKILL.md)
- [`execplan-prototype`](./skills/engineering/execplan-prototype/SKILL.md)
- [`execplan-improve-codebase-architecture`](./skills/engineering/execplan-improve-codebase-architecture/SKILL.md)
- [`execplan-triage`](./skills/engineering/execplan-triage/SKILL.md)
- [`execplan-tdd`](./skills/engineering/execplan-tdd/SKILL.md)
- [`execplan-zoom-out`](./skills/engineering/execplan-zoom-out/SKILL.md)

## Install Policy

Codex installs should copy only the `execplan-*` skills from
`skills/engineering/` into the developer's Codex skills directory. When
`CODEX_HOME` is set, that usually means `$CODEX_HOME/skills`; otherwise it is
commonly `$HOME/.codex/skills`.

The original upstream engineering skills may remain in this fork as reference
material, but they are not part of the active global harness and should not be
installed globally for this workflow.

## Lifecycle Fit

- ExecPlan authoring uses planning and clarification skills to create or revise
  `EXECPLAN.md`.
- Handoff preparation refreshes only `implementation-prompt.md` after the
  ExecPlan is accepted and unblocked.
- Implementation changes code only within accepted ExecPlan scope and updates
  `progress.md`, `decision-log.md`, and `discoveries-retrospective.md`.

If durable docs, repository state, or implementation discoveries contradict or
expand the accepted ExecPlan, the agent must stop and report drift instead of
silently expanding scope.
