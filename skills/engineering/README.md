# Engineering

## ExecPlan Harness Skills

Use these skills in repositories that follow the Codex ExecPlan harness, where
`docs/execplans/<plan-name>/EXECPLAN.md` is the implementation source of
truth.

The `execplan-handoff` skill is for temporary conversation continuity; it
does not replace ExecPlan lifecycle files.

`execplan-grill-with-docs` is the normal producer of durable vocabulary. Other
ExecPlan skills read `CONTEXT.md` or `CONTEXT-MAP.md` when present, but do not
create them.

- **[ascii-visual-modeling](./ascii-visual-modeling/SKILL.md)** - Terminal-friendly ASCII diagrams for difficult engineering explanations, specs, diagnosis notes, handoffs, state machines, ownership boundaries, and control/data flow.
- **[execplan-adopt-repo](./execplan-adopt-repo/SKILL.md)** — Repository setup/adoption for `AGENTS.md`, optional durable memory, and ExecPlan folder conventions without old issue-tracker setup.
- **[execplan-handoff](./execplan-handoff/SKILL.md)** — Temporary conversation handoff for context-limit, pause, or later-continuation cases, with ExecPlan-aware artifact references and `/tmp/codex-handoffs/` storage.
- **[execplan-grill-with-docs](./execplan-grill-with-docs/SKILL.md)** — ExecPlan-aware grilling for clarifying business intent, domain language, edge cases, and durable decisions while keeping `EXECPLAN.md` as the implementation contract.
- **[execplan-diagnose](./execplan-diagnose/SKILL.md)** — Diagnosis loop that captures repro, feedback loop, instrumentation bounds, and regression checks in `EXECPLAN.md`.
- **[execplan-prototype](./execplan-prototype/SKILL.md)** — Throwaway prototyping with explicit ExecPlan question, location, cleanup, and capture rules.
- **[execplan-improve-codebase-architecture](./execplan-improve-codebase-architecture/SKILL.md)** — Architecture review and deepening workflow where only accepted candidates become ExecPlan scope.
- **[execplan-triage](./execplan-triage/SKILL.md)** — Solo-developer intake/classification workflow that routes vague requests to diagnosis, grilling, architecture review, prototyping, handoff, or ExecPlan authoring.
- **[execplan-tdd](./execplan-tdd/SKILL.md)** — Vertical red-green-refactor slices governed by `EXECPLAN.md` and permissioned verification.
- **[execplan-zoom-out](./execplan-zoom-out/SKILL.md)** — Codebase mapping for authoring, handoff, and implementation orientation without creating hidden scope.
