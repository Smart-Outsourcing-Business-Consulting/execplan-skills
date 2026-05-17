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

Codex is the supported agent for this repository.

## Skills

- [`ascii-visual-modeling`](./skills/engineering/ascii-visual-modeling/SKILL.md)
- [`handoff`](./skills/engineering/handoff/SKILL.md)
- [`execplan-adopt-repo`](./skills/engineering/execplan-adopt-repo/SKILL.md)
- [`execplan-grill-with-docs`](./skills/engineering/execplan-grill-with-docs/SKILL.md)
- [`execplan-diagnose`](./skills/engineering/execplan-diagnose/SKILL.md)
- [`execplan-prototype`](./skills/engineering/execplan-prototype/SKILL.md)
- [`execplan-improve-codebase-architecture`](./skills/engineering/execplan-improve-codebase-architecture/SKILL.md)
- [`execplan-triage`](./skills/engineering/execplan-triage/SKILL.md)
- [`execplan-tdd`](./skills/engineering/execplan-tdd/SKILL.md)
- [`execplan-zoom-out`](./skills/engineering/execplan-zoom-out/SKILL.md)

## Installation

Prefer the `skills` CLI because it handles the Codex global destination and can
install from GitHub-hosted skill repositories.

For Codex, keep the installed ExecPlan skill copies in `$HOME/.agents/skills`.
Do not also keep duplicate `execplan-*` copies under
`${CODEX_HOME:-$HOME/.codex}/skills`, because Codex can load both roots and show
the same skills twice.

From GitHub:

```bash
npx skills@latest add Smart-Outsourcing-Business-Consulting/execplan-skills --global --agent codex
```

From a local checkout:

```bash
npx skills@latest add . --global --agent codex
```

Restart Codex after installing or updating skills so the active skill inventory
is reloaded.

Manual fallback: copy the desired directories from `skills/engineering/` into
`$HOME/.agents/skills`, replacing the old installed copies.

## Updating Installed Skills

When this repository changes, update the installed copies and restart Codex.
This repository is the source of truth; installed skill directories are runtime
copies.

If you installed from GitHub, rerun:

```bash
npx skills@latest add Smart-Outsourcing-Business-Consulting/execplan-skills --global --agent codex
```

If you installed from a local checkout, update the checkout first:

```bash
git pull --ff-only
npx skills@latest add . --global --agent codex
```

If `npx skills@latest add` refuses to replace existing installed directories,
remove the old `execplan-*` directories from `$HOME/.agents/skills` first, then
rerun the command.

If you installed manually, copy the updated `execplan-*` directories from
`skills/engineering/` into `$HOME/.agents/skills`, replacing the old copies.

After any update path, make sure duplicate ExecPlan copies do not exist under
`${CODEX_HOME:-$HOME/.codex}/skills`, then restart Codex.

## Lifecycle Fit

- ExecPlan authoring uses planning and clarification skills to create or revise
  `EXECPLAN.md`.
- Handoff preparation refreshes only `implementation-prompt.md` after the
  ExecPlan is accepted and unblocked.
- Implementation changes code only within accepted ExecPlan scope and updates
  `progress.md`, `decision-log.md`, and `discoveries-retrospective.md`.

The `handoff` skill is separate from ExecPlan lifecycle handoff preparation:
it writes temporary `/tmp/codex-handoffs/` conversation handoffs for
context-limit, pause, or later-continuation cases.

If durable docs, repository state, or implementation discoveries contradict or
expand the accepted ExecPlan, the agent must stop and report drift instead of
silently expanding scope.
