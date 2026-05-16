# Engineering

## ExecPlan Harness Skills

Use these variants in repositories that follow the global Codex ExecPlan
harness, where `docs/execplans/<plan-name>/EXECPLAN.md` is the implementation
source of truth.

- **[execplan-grill-with-docs](./execplan-grill-with-docs/SKILL.md)** — ExecPlan-aware grilling for clarifying business intent, domain language, edge cases, and durable decisions while keeping `EXECPLAN.md` as the implementation contract.
- **[execplan-diagnose](./execplan-diagnose/SKILL.md)** — Diagnosis loop that captures repro, feedback loop, instrumentation bounds, and regression checks in `EXECPLAN.md`.
- **[execplan-prototype](./execplan-prototype/SKILL.md)** — Throwaway prototyping with explicit ExecPlan question, location, cleanup, and capture rules.
- **[execplan-improve-codebase-architecture](./execplan-improve-codebase-architecture/SKILL.md)** — Architecture review and deepening workflow where only accepted candidates become ExecPlan scope.
- **[execplan-triage](./execplan-triage/SKILL.md)** — Solo-developer intake/classification workflow that routes vague requests to diagnosis, grilling, architecture review, prototyping, handoff, or ExecPlan authoring.
- **[execplan-tdd](./execplan-tdd/SKILL.md)** — Vertical red-green-refactor slices governed by `EXECPLAN.md` and permissioned verification.
- **[execplan-zoom-out](./execplan-zoom-out/SKILL.md)** — Codebase mapping for authoring, handoff, and implementation orientation without creating hidden scope.

The original upstream engineering skills are kept only as source-reference
material in this fork. They are not the active harness skills and should not be
installed globally for this workflow.
