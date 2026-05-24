---
name: execplan-handoff
description: Compact the current conversation into a temporary handoff document for another agent to pick up. Use when the current session may hit context limits, be paused, or be continued later.
argument-hint: "What will the next session be used for?"
---

# ExecPlan Handoff

Write a handoff document summarizing the current conversation so a fresh agent
can continue the work.

## Handoff Location

Store handoff files under:

`/tmp/codex-handoffs/`

Create that directory if it does not exist.

The handoff file name must always include the session identifier and a
descriptive handoff name or focus:

`<session-id>-<slugified-description>.md`

Examples:

- session id: `019e4d4a-ff58-7023-9e19-e24c55783792`
- description: `check compatibility windows rsync`
- filename:
  `019e4d4a-ff58-7023-9e19-e24c55783792-check-compatibility-windows-rsync.md`

If the active session identifier can be determined from the prompt, UI text,
screenshot, user-provided text, environment, or other visible context, use it.

If the active session identifier cannot be determined automatically, ask the
user for it before continuing. Do not invent one.

If the descriptive handoff name or focus cannot be determined from the skill
arguments, prompt, user-provided text, or other visible context, ask the user
for it before continuing. Do not invent one.

Slugify the descriptive name or focus before constructing the filename:

- lowercase the description
- replace whitespace and underscores with hyphens
- remove characters unsafe for filenames
- reject or remove path separators
- collapse repeated hyphens
- trim leading and trailing hyphens

Never write outside `/tmp/codex-handoffs/`. Treat any path separators in the
session identifier or slugified description as invalid filename content, not as
directories.

Before writing, compute the target file path under `/tmp/codex-handoffs/`,
check whether that exact file already exists, and read it if it does.

## Document Header

Put these fields at the top:

```markdown
# Handoff

Working directory: <working directory at the moment of execplan-handoff skill activation>
Created: <creation date and time>
```

## Content Rules

Do not duplicate content already captured in other artifacts such as PRDs,
plans, ExecPlans, ADRs, issues, commits, diffs, or implementation-state files.
Reference them by path or URL instead.

If an ExecPlan exists, be ExecPlan-aware:

- reference `docs/execplans/<plan-name>/EXECPLAN.md` for accepted scope
- reference durable vocabulary only as planning memory: if `CONTEXT-MAP.md`
  exists, read it first and use it to find the relevant context file; else read
  root `CONTEXT.md` when present; else proceed without durable context
- reference `docs/adr/*` only as durable planning memory
- reference `progress.md`, `decision-log.md`, and
  `discoveries-retrospective.md` if they already contain relevant continuity
  details
- do not convert those files into hidden requirements
- do not create `CONTEXT.md`; `execplan-grill-with-docs` is the normal producer
  of durable vocabulary during authoring
- do not update ExecPlan lifecycle files unless the user separately asked for
  that lifecycle action

When summarizing implementation continuity, preserve scope language:

- required behavior in this pass
- explicitly deferred behavior
- discovered optional future work
- blockers
- out-of-scope work

Do not summarize optional future semantics as "remaining gaps" or automatic
follow-up work unless the accepted ExecPlan says they are required.

Suggest the skills to be used, if any, by the next session.

If the user passed arguments, treat them as a description of what the next
session will focus on and tailor the handoff accordingly.

## Suggested Structure

Use only the sections that help the next session continue efficiently:

```markdown
# Handoff

Working directory: <working directory at the moment of execplan-handoff skill activation>
Created: <creation date and time>

## Next Session Focus
<user-provided focus, if any>

## Current State
<brief summary of what has happened and where things stand>

## Relevant Artifacts
<paths or URLs to PRDs, plans, ExecPlans, ADRs, issues, commits, diffs, or other files>

## Open Decisions or Blockers
<questions, risks, or drift conditions the next session must handle>

## Suggested Skills
<skills the next session should consider>
```
