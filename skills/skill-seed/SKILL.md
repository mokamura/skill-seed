---
name: skill-seed
description: Record reusable patterns discovered during work as skill candidates. Reference for evaluation criteria and recording format.
---

# Skill Seed

## Overview

Accumulate reusable patterns found during work into candidates.md.
Do not suggest to the user or create skills — only record.
Review is done via the `/review-skill-candidates` command.

## Evaluation Criteria

Record when **2 or more** of the following 4 conditions are met:

| Condition | Description |
|-----------|-------------|
| Reusability | Pattern is applicable across different projects |
| Repetition | Same pattern executed 2+ times |
| Complexity | Requires multiple steps or specialized knowledge |
| Stability | Procedure unlikely to change frequently |

## Anti-patterns

Do NOT record:
- Trivial operations that take 1-2 lines
- Patterns specific to a particular API or library with no generality
- Procedures that change frequently
- Patterns already covered by an existing skill or command

## Candidate Format

Append to `candidates.md` using this format:

```markdown
### {YYYY-MM-DD}: {kebab-case-name}
- **context**: {what task was being performed when this pattern was detected}
- **summary**: {what this pattern does in one line}
- **reason**: {why this is a skill candidate — mention which criteria are met}
- **type**: skill | command
```

## Deduplication

Before appending, check existing candidates in candidates.md for duplicate names or summaries.
If a similar candidate exists, merge by adding to the existing candidate's reason field instead of creating a new entry.

## File Locations

- Candidate storage: `~/.claude/skills/skill-seed/candidates.md`
- Review: `/review-skill-candidates` command
