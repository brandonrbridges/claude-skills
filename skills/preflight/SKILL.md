---
name: preflight
description: Use when the user invokes /preflight or wants to check codebase health before committing, deploying, or creating a PR. Runs type-check, lint, and build, then reports all issues with options to fix.
---

# Preflight

Run every quality gate in the project — type-check, lint, build — collect ALL failures, and present a full report with options.

**Core principle:** No surprises. Surface everything at once so the user can make an informed decision.

## Workflow

### 1. Detect Available Commands

Read `package.json` (root and workspace) to find the right commands. Common patterns:

```bash
# Check what's available
cat package.json | grep -E '"(lint|type-check|typecheck|tsc|build|check)"'
```

Typical mappings:

| Check | Common Commands |
|-------|----------------|
| Type-check | `pnpm type-check`, `pnpm tsc --noEmit`, `npx tsc --noEmit` |
| Lint | `pnpm lint`, `pnpm eslint .`, `npx eslint .` |
| Build | `pnpm build`, `npm run build` |

If a monorepo with Turborepo/Nx, use the root commands — they'll fan out to all packages.

### 2. Run All Checks in Parallel

Run type-check, lint, and build concurrently. **Do NOT stop on first failure** — collect everything.

Use the Bash tool with `run_in_background: true` for each check so they run in parallel:

```bash
# Run each independently, capture full output
pnpm type-check 2>&1
pnpm lint 2>&1
pnpm build 2>&1
```

If a command doesn't exist, skip it and note it in the report.

### 3. Present the Report

After all checks complete, present a structured report:

```markdown
## Preflight Report

| Check      | Status | Issues |
|------------|--------|--------|
| Type-check | FAIL   | 12 errors |
| Lint       | FAIL   | 3 errors, 8 warnings |
| Build      | PASS   | — |

### Type Errors (12)
- `src/components/Button.tsx:42` — Property 'onClick' is missing in type...
- `src/hooks/useAuth.ts:15` — Type 'string' is not assignable to...
[...all errors...]

### Lint Errors (3)
- `src/utils/format.ts:8` — 'formatDate' is defined but never used (no-unused-vars)
- `src/api/client.ts:23` — Unexpected any. Specify a different type (@typescript-eslint/no-explicit-any)
[...all errors...]

### Lint Warnings (8)
[...all warnings...]
```

**Rules:**
- Show EVERY error and warning — no truncating
- Group by check type, then by file
- Include file:line references so the user can navigate
- If output is very long (50+ issues), summarize by category with counts, then list all

### 4. Offer Options

After the report, present exactly three options:

```markdown
## What would you like to do?

1. **Fix everything** — I'll fix all type errors, lint errors, and warnings, then commit as a clean-slate preflight fix
2. **Fix build-blockers only** — I'll fix only the errors preventing a successful build (type errors + lint errors), skip warnings
3. **Proceed anyway** — Skip fixes and continue with your current task
```

Wait for the user to choose. Do not proceed without their input.

### Option 1: Fix Everything

- Fix all type errors, all lint errors, and all lint warnings
- Run all three checks again to verify everything passes
- If new issues surface from fixes, fix those too — iterate until clean
- Stage only the files you changed and commit:
  ```
  chore: fix type errors, lint errors, and warnings
  ```
- Use /bb:commit conventions (no AI attribution, conventional commit, rebase + push)

### Option 2: Fix Build-Blockers Only

- Fix type errors and lint errors (things that would fail CI)
- Ignore lint warnings
- Run type-check and build again to verify they pass
- Stage and commit:
  ```
  fix: resolve type and lint errors
  ```
- Use /bb:commit conventions

### Option 3: Proceed Anyway

- Do nothing. Return control to the user
- Mention which checks are failing so they're aware

## Edge Cases

**No package.json found:**
- Look for other build systems (Makefile, Cargo.toml, pyproject.toml)
- If nothing found, tell the user and ask what commands to run

**Monorepo with per-package failures:**
- Report which packages failed, not just aggregate counts
- When fixing, work package by package

**Lint has auto-fixable issues:**
- For Option 1 and 2, try `pnpm lint --fix` first before manual fixes
- Run lint again after auto-fix to see what remains

**Tests:**
- This skill does NOT run tests. Tests are a separate concern.
- If the user asks about tests, suggest running them separately.

## What NOT To Do

- Don't stop at the first failing check — run everything
- Don't silently fix things without presenting the report first
- Don't skip the options prompt — always let the user choose
- Don't run tests (that's out of scope for preflight)
- Don't truncate error output — the user needs the full picture
- Don't treat warnings as blocking unless the project's config treats them as errors
