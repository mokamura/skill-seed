Review skill candidates accumulated in `~/.claude/skills/skill-seed/candidates.md`.

## Steps

1. Read `~/.claude/skills/skill-seed/candidates.md`
2. If there are no candidates (heading only), inform the user and exit
3. Check `~/.claude/skills/` and `~/.claude/commands/` for existing skills/commands and flag any duplicate candidates
4. Present each candidate to the user:
   - Name, context, summary, reason, type
   - Point out similarities with existing skills if any
5. Use AskUserQuestion to confirm disposition for each candidate:
   - **Approve**: Create the skill or command, then remove from candidates.md
   - **Reject**: Remove from candidates.md
   - **Defer**: Leave in candidates.md
6. For approved candidates:
   - skill: Create `~/.claude/skills/{name}/SKILL.md`
   - command: Create `~/.claude/commands/{name}.md`
   - Present the proposed name, summary, and steps to the user for confirmation before creating
7. After reviewing all candidates, display a summary:
   - Count of approved, rejected, and deferred
   - List of created skill/command paths
