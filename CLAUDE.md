# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Silently accumulates reusable patterns discovered during Claude Code sessions as skill candidates, letting the user batch-review them later. The codebase consists entirely of markdown files — no build, test, or lint tooling exists.

## Architecture

```
CLAUDE.md                              # Snippet to append to global CLAUDE.md
skills/skill-seed/
├── SKILL.md                           # Evaluation criteria & format definition
└── candidates.md                      # Candidate accumulation file (append-only)
commands/
└── review-skill-candidates.md         # /review-skill-candidates command definition
```

**Data flow**: CLAUDE.md instructions → Claude detects patterns during work → silently appends to `candidates.md` → user runs `/review-skill-candidates` for batch review → approved candidates become skills/commands under `~/.claude/`

## Key Design Decisions

- **Silent accumulation**: Never suggest to the user during work. Only append to `candidates.md`
- **Criteria centralized in SKILL.md**: Record when 2+ of 4 conditions are met (reusability, repetition, complexity, stability)
- **Anti-patterns are explicit**: Do not record trivial operations, project-specific patterns, or frequently changing procedures
- **Deduplication**: Read candidates.md before appending; merge into existing candidate's reason field if similar

## Installation Target Paths

Copy files from this repository to the following destinations:

| Source | Destination |
|--------|-------------|
| Contents of `CLAUDE.md` | Append to `~/.claude/CLAUDE.md` |
| `skills/skill-seed/` | `~/.claude/skills/skill-seed/` |
| `commands/review-skill-candidates.md` | `~/.claude/commands/review-skill-candidates.md` |

## Editing Guidelines

- When modifying `SKILL.md`, keep trigger conditions and format in sync with CLAUDE.md
- When changing the candidate format (`### {YYYY-MM-DD}: {kebab-case-name}` + context/summary/reason/type), update all 3 files simultaneously: CLAUDE.md, SKILL.md, review-skill-candidates.md
- When modifying `review-skill-candidates.md`, verify output paths (`~/.claude/skills/` / `~/.claude/commands/`) are correct

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
