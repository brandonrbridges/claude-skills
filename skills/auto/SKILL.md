---
name: auto
description: Use when the user invokes /auto to signal full autonomous mode. Overrides confirmation prompts and plan approval gates — CLAUDE.md becomes the sole guardrail.
---

# Auto Mode

Full autonomy. Stop asking, start doing. The project's CLAUDE.md is the guardrail — follow it precisely.

## Override

This skill **overrides** all default pause-and-confirm behavior, including any global CLAUDE.md rules about asking before committing or pushing:

- **Plans**: Execute immediately. Do not present plans for approval — just work.
- **Commits and pushes**: Allowed freely (use /commit conventions for format and workflow).
- **File creation/deletion**: Allowed without asking.
- **Destructive git operations**: Still confirm — these are genuinely irreversible.
- **External-facing actions** (PR creation, issue comments, messages to others): Still confirm — these affect other people.

## What Changes

| Default Behavior | Auto Mode |
|-----------------|-----------|
| "Here's my plan, shall I proceed?" | Just proceed. |
| "Should I commit these changes?" | Commit and push. |
| "I'll need to create a new file..." | Create it. |
| "Want me to run the tests?" | Run them. |
| "Can you clarify what you mean by...?" | Read the code and figure it out. |
| "Should I refactor this while I'm here?" | Use judgment — if CLAUDE.md says to, do it. |

## What Stays the Same

The project's CLAUDE.md is not optional. Auto mode means you move fast **within** the project's established patterns — every coding standard, architectural convention, and testing rule still applies. Read and follow them.

**Auto mode = trust in execution, not trust to ignore standards.**

## How to Work

1. Read the request. Understand the scope.
2. If you need context, gather it silently — don't narrate your exploration. Don't ask clarifying questions you can answer yourself by reading code.
3. Implement the solution. Use best judgment on approach.
4. Run tests/type-checks/lints to verify.
5. Commit and push when the work is solid.
6. Report what you did — briefly.

## Scope Discipline

Auto mode gives you freedom to execute, not freedom to expand scope. Stay within what was asked:

- Fix the bug, not the three adjacent issues you noticed.
- Build the feature, not the "nice-to-have" extensions.
- If you discover something important but out of scope, **mention it in your summary** — don't silently fix it.

The user trusts you to move fast. Repay that trust by being predictable in what you touch.

## Red Flags — Still Stop

Even in auto mode, pause if:

- You're about to **force push** or **reset --hard**
- You're about to **delete a branch** with unmerged work
- You're about to **modify CI/CD** or **infrastructure config**
- You're about to **post externally** (PRs, issues, messages)
- Something feels **off** — a test fails unexpectedly, a migration looks wrong, data doesn't match

Trust is earned by knowing when to stop, not by never stopping.
