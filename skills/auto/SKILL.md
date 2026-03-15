---
name: auto
description: Use when the user invokes /auto to signal full autonomous mode. Overrides confirmation prompts and plan approval gates — CLAUDE.md becomes the sole guardrail.
---

# Auto Mode

Full autonomy. Stop asking, start doing. CLAUDE.md is the guardrail — follow it precisely.

## Override

This skill **overrides** all pause-and-confirm behavior, including any rules about asking before committing or pushing:

- **Plans**: Execute immediately, don't present for approval.
- **Commits and pushes**: Allowed freely.
- **File creation/deletion**: Allowed without asking.
- **Destructive git operations**: Still confirm — genuinely irreversible.
- **External-facing actions** (PRs, issues, messages): Still confirm — affects other people.

## How to Work

1. Read the request. Understand the scope.
2. Gather context silently — don't narrate exploration or ask questions you can answer by reading code.
3. Implement. Run tests/type-checks/lints to verify.
4. Commit and push when solid.
5. Report what you did — briefly.

## Scope Discipline

Freedom to execute, not freedom to expand scope.

- Fix the bug, not the three adjacent issues you noticed.
- Build the feature, not the "nice-to-have" extensions.
- Out-of-scope discoveries go in your summary, not into silent fixes.

## Still Stop If

- Force push or `reset --hard`
- Deleting a branch with unmerged work
- Modifying CI/CD or infrastructure config
- Posting externally (PRs, issues, messages)
- Something feels off — unexpected test failure, wrong data, broken migration
