# Developer guide

[Back to README](../README.md)

## Layout

```
.
├── SKILL.md
├── README.md
├── CHANGELOG.md
└── docs/
    ├── USER_GUIDE.md
    └── DEVELOPER_GUIDE.md
```

`SKILL.md` is the product. The rest explains how to install, use and edit it.

## How the skill works

The frontmatter `name` is the invocation name. The `description` is what the agent matches against user requests, so keep the trigger phrases in it. The body holds the method, the rules, the output format and one worked example.

There is no code and no build step. A skill is instructions.

## Editing

- Keep `SKILL.md` under 60 lines. Length dilutes the rules that matter.
- Keep the method generic. Project-specific advice belongs in the project, not in the skill.
- When you change behavior or triggers, update the user guide and the changelog in the same commit.
- The example uses round numbers so it can be checked by hand.

## Testing

1. Copy the repository to your agent's skills directory, e.g. `~/.claude/skills/performance-envelope` or `~/.kimi-code/skills/performance-envelope`.
2. Start a new session so the skill list reloads.
3. Ask a trigger question about a program with known measurements.
4. Check the reply for the table, the stated machine assumptions and exactly one lever.

## Conventions

- Apply `unslop` to every file.
- Sentence case headings.
- No em dashes.

## Releasing

Bump the changelog with a new version heading and a summary of the user-visible change. Nothing else is required.
