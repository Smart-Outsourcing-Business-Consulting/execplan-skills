Skills live under `skills/engineering/`.

Do not add non-engineering skill buckets such as `productivity/`, `misc/`,
`personal/`, `in-progress/`, or `deprecated/`. This repository is scoped to
skills for AI-assisted software engineering.

Every skill in `skills/engineering/` must have a reference in the top-level
`README.md`, a reference in `skills/engineering/README.md`, and an entry in
`.claude-plugin/plugin.json`.

Each skill entry must link the skill name to its `SKILL.md`.

Codex is the only supported agent for now. Claude compatibility metadata may be
kept in sync, but Claude runtime behavior is not currently supported.
