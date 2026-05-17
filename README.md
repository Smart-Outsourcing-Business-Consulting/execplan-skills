# ExecPlan Skills

This repository hosts Codex-supported skills for AI-assisted software
engineering, centered on the ExecPlan harness. The harness contract is
summarized here so the repository is portable across developer machines and
teams.

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

## Supported Agent

Codex is the only supported agent today. Claude compatibility metadata may exist
so the repository can become multi-agent later, but the supported install and
runtime path is Codex.

## Skills

### ExecPlan Harness

- [`execplan-grill-with-docs`](./skills/engineering/execplan-grill-with-docs/SKILL.md)
- [`execplan-diagnose`](./skills/engineering/execplan-diagnose/SKILL.md)
- [`execplan-prototype`](./skills/engineering/execplan-prototype/SKILL.md)
- [`execplan-improve-codebase-architecture`](./skills/engineering/execplan-improve-codebase-architecture/SKILL.md)
- [`execplan-triage`](./skills/engineering/execplan-triage/SKILL.md)
- [`execplan-tdd`](./skills/engineering/execplan-tdd/SKILL.md)
- [`execplan-zoom-out`](./skills/engineering/execplan-zoom-out/SKILL.md)

### General Engineering

- [`diagnose`](./skills/engineering/diagnose/SKILL.md)
- [`grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md)
- [`improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md)
- [`prototype`](./skills/engineering/prototype/SKILL.md)
- [`setup-engineering-skills`](./skills/engineering/setup-engineering-skills/SKILL.md)
- [`tdd`](./skills/engineering/tdd/SKILL.md)
- [`to-issues`](./skills/engineering/to-issues/SKILL.md)
- [`to-prd`](./skills/engineering/to-prd/SKILL.md)
- [`triage`](./skills/engineering/triage/SKILL.md)
- [`zoom-out`](./skills/engineering/zoom-out/SKILL.md)

## Installation

Prefer the `skills` CLI because it handles the Codex global destination and can
install from GitHub-hosted skill repositories.

From GitHub:

```bash
npx skills@latest add Smart-Outsourcing-Business-Consulting/execplan-skills --global --agent codex
```

From a local checkout:

```bash
npx skills@latest add . --global --agent codex
```

Restart Codex after installing or updating skills.

Manual fallback: copy the desired directories from `skills/engineering/` into
`${CODEX_HOME:-$HOME/.codex}/skills`.

## Roadmap

- A Codex plugin for this skill bundle is in the works
  ([#1](https://github.com/Smart-Outsourcing-Business-Consulting/execplan-skills/issues/1)).
  It should follow the current OpenAI Codex plugin conventions documented in
  <https://developers.openai.com/codex/plugins/build>, package the existing
  `skills/engineering/` skills without duplicating their source, and keep Codex
  as the only supported runtime until multi-agent support is explicitly added.

Claude compatibility is a likely future direction, but it is not part of the
current supported workflow. When that work happens, the repository should define
which skills are shared across agents, how Claude-specific install metadata is
kept in sync, and whether any behavior differs between Codex and Claude.

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
