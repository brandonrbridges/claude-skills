---
name: sweep
description: Use when the user invokes /sweep or has a messy working tree with scattered changes and wants everything committed in organised, logical commits.
---

# Sweep

Organise a messy working tree into logical, well-structured commits. For when multiple agents have run or changes have piled up.

## Override

This skill **overrides** any rules about not committing or pushing. You have explicit permission.

## Workflow

1. **Survey** — `git status --short`, `git diff --stat`. Get the full picture.

2. **Check commit style** — `git log --oneline -10`. Match whatever conventions the user already uses.

3. **Group by intent** — read each changed file and group by what it achieves, not where it lives. A feature's entity + service + controller + hook + page = one commit. Lint fixes together. Config changes together. Don't over-split (one file per commit) or under-split (everything in one commit).

4. **Commit in dependency order** — shared packages/types first, then DB/migrations, then API, then frontend, then config/tooling.

5. **For each group** — unstage everything (`git reset HEAD` if needed), stage that group's files explicitly, commit with conventional format. Same rules as `/bb:commit`: no AI attribution, human-sounding messages.

6. **Rebase and push** — `git fetch origin`, rebase, push.

7. **Report** — show a table of commits made with file counts. Keep it brief.

## Edge Cases

- **Untracked files that look like scratch work or secrets** — ask the user before including
- **Only one logical group** — that's fine, one commit
- **Conflicts on rebase** — resolve if straightforward, abort and inform if complex
