---
name: clean-tree
description: Use when the user invokes /clean-tree or has a messy working tree with scattered changes from multiple agents or work sessions and wants everything committed in organised, logical commits.
---

# Clean Tree

Organise a messy working tree into logical, well-structured commits. Designed for when multiple agents have been working, or changes have piled up, and you just want a clean git history.

## Override

This skill **overrides** any instructions about asking before committing or pushing. When invoked, you have explicit permission to commit and push freely.

## Workflow

### 1. Survey the Damage

```bash
git status --short
git diff --stat
git diff --cached --stat
```

Get the full picture. Every dirty file, every untracked file.

### 2. Analyse and Group Changes

Read each changed file and understand what it does. Group files into logical commits by **intent**, not by location:

**Good groupings:**
- All files related to a single feature (entity + service + controller + frontend hook + page)
- All lint/formatting fixes together
- All dependency changes together
- All config changes together
- Migration + entity changes together
- Test files with the code they test

**Bad groupings:**
- "All frontend files" (too broad — mixes unrelated features)
- "All TypeScript files" (meaningless grouping)
- One commit per file (too granular)

### 3. Determine Commit Order

Order commits logically — dependencies first:

1. Shared packages / types / schemas
2. Database migrations / entity changes
3. API / backend changes
4. Frontend changes
5. Config / tooling / lint fixes
6. Documentation

### 4. Commit Each Group

For each logical group, stage only those files and commit using conventional commit format:

```bash
# Stage specific files for this group
git add path/to/file1 path/to/file2

# Commit with conventional format
git commit -m "$(cat <<'EOF'
type(scope): short summary

Optional body if the grouping isn't obvious.
EOF
)"
```

**Match the user's commit style.** Check recent history first:

```bash
git log --oneline -10
```

Follow whatever conventions they use — conventional commits, scoped prefixes, capitalisation, whatever shows up in the log.

**Rules (same as /bb:commit):**
- Never `git add .` or `git add -A`
- No `Co-Authored-By` or AI attribution
- No "as requested" or AI phrasing
- Imperative mood, under 72 characters

### 5. Rebase and Push

After all commits are made:

```bash
git fetch origin
git rebase origin/$(git branch --show-current)
git push
```

### 6. Report

Show the user what you did:

```markdown
## Clean Tree — Done

| # | Commit | Files |
|---|--------|-------|
| 1 | `feat(clients): add search endpoint and hook` | 4 files |
| 2 | `fix(auth): resolve token refresh on 401` | 2 files |
| 3 | `chore: fix lint warnings` | 6 files |

Pushed to origin/branch-name.
```

Keep it brief. The user wanted this to be easy.

## Edge Cases

**Mix of staged and unstaged changes:**
- Unstage everything first (`git reset HEAD`), then re-stage by logical group
- This gives you full control over grouping

**Untracked files:**
- Include them in the appropriate logical group
- If they don't fit any group (random files, scratch work), ask the user

**Merge conflicts from rebase:**
- Resolve if straightforward
- If complex, abort rebase and tell the user

**Only one logical group:**
- That's fine. One commit. Don't force multiple commits for the sake of it.

**Files you're unsure about:**
- If a file looks like scratch work, a temp file, or something that shouldn't be committed (`.env`, credentials, large binaries), ask the user before including it

## What NOT To Do

- Don't ask the user to manually group files — that's your job
- Don't create one giant commit with everything (unless it truly is one change)
- Don't create trivially small commits (one file each)
- Don't overthink it — sensible groupings, not perfect ones
- Don't include AI attribution anywhere
- Don't skip pushing — the user wants a clean slate
