---
name: pr
description: Use when the user invokes /pr or asks to create a pull request. Generates a well-structured PR from branch commits, pushes, and creates via gh CLI.
---

# PR

Create a clean pull request from the current branch.

## Override

This skill **overrides** any rules about not pushing. You have explicit permission to push and create the PR.

## Workflow

1. **Understand the branch** — check current branch, detect base branch (`git remote show origin | grep 'HEAD branch'`), read all commits since divergence (`git log --oneline $(git merge-base HEAD origin/main)..HEAD`), and get the full diff stat. If on main/master, stop and tell the user to create a feature branch.

2. **Generate content** — analyse ALL commits on the branch, not just the latest:
   - **Title**: under 70 characters, conventional commit style (`feat: ...`, `fix: ...`)
   - **Body**: summary bullets (what and why), list of meaningful changes grouped by intent, test plan checklist

3. **Push and create** — `git push -u origin $(git branch --show-current)`, then `gh pr create`.

4. **Report** — show the PR URL, title, commit count, and files changed.

## Rules

- Write as a human developer — no AI phrasing
- Focus on "why" not "what" — the diff shows the what
- Don't list every file — group by intent
- Include `Closes #123` if there's a related issue
- Don't create drafts unless asked
- Don't assign reviewers unless asked
- Don't modify code — this skill only creates the PR
