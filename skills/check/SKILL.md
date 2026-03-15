---
name: check
description: Use when the user invokes /check or wants to verify codebase health before committing, deploying, or creating a PR. Runs type-check, lint, and build, then reports all issues with options to fix.
---

# Check

Run every quality gate — type-check, lint, build — and surface everything at once.

## Workflow

1. **Detect commands** — read `package.json` for `type-check`/`typecheck`, `lint`, and `build` scripts. If monorepo, use root commands.

2. **Run all in parallel** — use `run_in_background: true` for each. Do NOT stop on first failure. Collect everything.

3. **Report** — present a structured report with pass/fail status per check, then list every error and warning with `file:line` references. Don't truncate. If 50+ issues, summarise by category with counts then list all.

4. **Offer three options:**

| Option | What happens |
|:-------|:-------------|
| **Fix everything** | Resolve all errors and warnings, verify with a re-run, commit as `chore: fix type errors, lint errors, and warnings` |
| **Fix build-blockers only** | Fix type errors and lint errors only (skip warnings), verify, commit as `fix: resolve type and lint errors` |
| **Proceed anyway** | Do nothing, return control to user |

Wait for the user to choose. Do not proceed without input.

## Rules

- Try `pnpm lint --fix` first for auto-fixable issues before manual fixes
- Use `/bb:commit` conventions for fix commits
- This skill does NOT run tests — that's a separate concern
- If a command doesn't exist, skip it and note it in the report
- For monorepos, report which packages failed, not just aggregate counts
