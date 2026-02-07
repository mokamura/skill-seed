# skill-seed

[日本語](README_ja.md)

Silently collects reusable patterns discovered during Claude Code sessions and lets you batch-review them later to turn into skills or commands.

## Why

During a Claude Code session you often notice patterns worth turning into a skill, but an immediate suggestion would break your flow. Doing nothing means you forget. skill-seed solves this by:

1. Claude **silently appends** patterns to `candidates.md` as it works
2. You run `/review-skill-candidates` whenever you're ready
3. Only approved candidates become skills or commands

## Installation

### 1. Add the following snippet to your CLAUDE.md

Append the following to `~/.claude/CLAUDE.md` (global) or your project's `CLAUDE.md`:

````markdown
## Skill Discovery

When you notice a pattern matching any of the triggers below during work, append it to `~/.claude/skills/skill-seed/candidates.md`.
Do NOT suggest to the user. Silently append only.

**Triggers** (1 or more):
- Completed a multi-step workflow that could be reused across projects
- Repeated the same procedure/pattern 2+ times
- Implemented a workaround or complex procedure

**Append format**:
```
### {YYYY-MM-DD}: {kebab-case-name}
- **context**: {what task was being performed when this pattern was detected}
- **summary**: {what this pattern does in one line}
- **reason**: {why this is a skill candidate}
- **type**: skill | command
```

**Rules**:
- Read candidates.md before appending to check for duplicates
- Do not record trivial operations (1-2 lines) or project-specific patterns
- Do not create the skill — only accumulate
- Review happens when the user runs `/review-skill-candidates`
````

### 2. Copy the skill

```bash
cp -r skills/skill-seed ~/.claude/skills/skill-seed
```

### 3. Copy the command

```bash
cp commands/review-skill-candidates.md ~/.claude/commands/review-skill-candidates.md
```

## Usage

### Collecting candidates (automatic)

Just work as usual. When Claude detects a pattern matching any of these triggers, it appends an entry to `~/.claude/skills/skill-seed/candidates.md`:

- A multi-step workflow that could be reused across projects
- The same procedure repeated 2+ times
- A workaround or complex procedure was implemented

### Reviewing candidates (manual)

```
/review-skill-candidates
```

Review each candidate one by one — approve, reject, or defer.
Approved candidates are created under `~/.claude/skills/` or `~/.claude/commands/`.

## File Structure

```
skill-seed/
├── CLAUDE.md                              # Project guidance for contributors
├── skills/
│   └── skill-seed/
│       ├── SKILL.md                       # Evaluation criteria & format definition
│       └── candidates.md                  # Candidate accumulation file
└── commands/
    └── review-skill-candidates.md         # /review-skill-candidates command
```

## Evaluation Criteria

A pattern is recorded when it meets **2 or more** of the following 4 conditions:

| Condition | Description |
|-----------|-------------|
| Reusability | Applicable across different projects |
| Repetition | Same pattern executed 2+ times |
| Complexity | Requires multiple steps or specialized knowledge |
| Stability | Procedure unlikely to change frequently |

## License

MIT
