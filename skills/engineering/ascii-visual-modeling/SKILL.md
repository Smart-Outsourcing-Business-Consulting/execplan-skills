---
name: ascii-visual-modeling
description: Use when explaining, diagnosing, planning, reviewing, or specifying a difficult engineering issue, architecture, workflow, state machine, ownership boundary, dependency graph, retry/error path, data/control flow, or idea in a terminal/chat context. Also use when authoring ExecPlans, specs, discovery notes, or handoff docs that would benefit from a concise ASCII diagram.
---

# ASCII Visual Modeling

Use this skill to turn a difficult engineering explanation into a small
terminal-friendly visual model before or alongside prose.

The model is not decoration. It should expose structure that prose tends to
hide: owners, boundaries, flow, state, authority, coupling, failure paths, and
open decisions.

## When To Use

Use an ASCII model when the task involves one or more of:

- multiple components, owners, processes, services, workers, queues, jobs, or
  storage locations
- a state machine, lifecycle, retry policy, branch, timeout, fallback, or
  failure mode
- an architectural boundary, dependency direction, responsibility split, or
  authority question
- a spec, ExecPlan, discovery note, implementation prompt, handoff, diagnosis,
  or review where the reader must reconstruct flow from text
- a confusing idea where a picture would make the first explanation shorter
  and less ambiguous

Skip it for small edits, simple yes/no answers, or explanations that are
already clearer as a sentence or table.

## Core Rules

- Put diagrams in fenced `text` code blocks.
- Prefer ASCII characters only: `+`, `-`, `|`, `>`, `v`, `[ ]`, `( )`.
- Keep diagrams compact enough for a terminal, aiming for 80 columns and never
  relying on wide formatting to be understandable.
- Model one idea per diagram. Use two small diagrams instead of one crowded
  map.
- Label edges with the meaning of the transition, request, event, or guard.
- Label boxes with ownership or authority when that is the point.
- Mark uncertainty explicitly with `?` or `TBD`; do not make a guess look
  accepted.
- Pair the diagram with a short explanation of what it proves, what it omits,
  and any remaining question.
- Do not let a diagram replace normative text, acceptance criteria, test
  expectations, or drift/blocker rules.
- When the plan or code changes, update or remove stale diagrams.

## Choosing A Model

Use the smallest model that clarifies the issue.

### Component Boundary

Use when the question is ownership, authority, allowed dependency direction, or
where a responsibility should live.

```text
+--------------------+        request/event        +--------------------+
| Owner A            | ---------------------------> | Owner B            |
| - owns policy      |                              | - owns execution   |
| - stores intent    | <--------------------------- | - returns evidence |
+---------+----------+        normalized result     +---------+----------+
          |
          | durable state
          v
+--------------------+
| Shared Store        |
| - source of truth   |
+--------------------+
```

### Sequence Or Control Flow

Use when the reader needs to see order, handoff points, or where data changes
shape.

```text
incoming request
  -> validate contract
  -> persist intent
  -> publish work
  -> worker claims message
  -> execute side effect
  -> store result
  -> notify caller
```

### State Machine

Use when behavior depends on states, guards, terminal states, or retries.

```text
[pending]
   |
   +--> dispatch accepted --------------------> [running]
   |                                             |
   |                                             +--> success --> [succeeded]
   |                                             |
   |                                             +--> retryable failure
   |                                                     |
   |                                                     v
   |                                               [waiting_retry]
   |
   +--> validation rejected -------------------> [blocked]
```

### Decision Tree

Use when a rule branches on guards and the inverse behavior matters.

```text
candidate found?
  |
  +-- no  -> create from source evidence
  |
  +-- yes
        |
        +-- same identity?  -> merge facts
        |
        +-- conflict?       -> stop and surface drift
```

### Failure Containment

Use when explaining why a failure does or does not cross a boundary.

```text
[Worker owns side effect]
        |
        +--> before side effect fails  -> retry original message
        |
        +--> after side effect fails   -> require idempotency/compensation
                                         before replay is allowed
```

## ExecPlan And Spec Use

When writing an ExecPlan or spec:

- Add diagrams only where they reduce ambiguity in accepted scope.
- Keep the accepted behavior in prose or tables after the diagram.
- Use diagrams to make boundaries, state transitions, data flow, and stop
  conditions easy to audit.
- If a diagram comes from discovery history, restate only the implementation
  consequence needed by the active plan.
- If a diagram reveals a missing business, architecture, schema, or security
  decision, stop and ask instead of filling the gap visually.

## Review Checklist

Before finalizing, check:

- Can the reader explain the main boundary or flow from the diagram alone?
- Are all labels stable domain terms rather than vague words like "stuff" or
  "magic"?
- Are uncertain parts marked instead of implied?
- Does the surrounding text say what the diagram means for implementation?
- Would the diagram still fit and read correctly in a terminal?
