---
name: diagnose
description: Use when diagnosing bugs, performance issues, or unexpected behavior that may span multiple packages, services, or layers. Triggers include 504s, auth failures, data mismatches, slow queries, or any symptom crossing service boundaries.
---

# Diagnose

Dispatch parallel agents across all affected layers, synthesise findings into a root cause analysis, and propose a fix — but don't implement until approved.

**Core principle:** Evidence from all layers BEFORE hypotheses. Cross-boundary bugs hide at service interfaces.

## When to Use

- Bug might span multiple packages, services, or layers — use this skill
- Bug is clearly in one file/function — use systematic debugging instead
- You already know the root cause — just fix it

## Workflow

1. **Gather symptom** — if not already provided, ask: what's happening, when did it start, who's affected.

2. **Map the platform** — understand the project structure before investigating. Check for monorepo config (`turbo.json`, `pnpm-workspace.yaml`, `nx.json`), list `apps/`, `packages/`, `services/`. For multi-repo setups, ask the user which repos are involved.

3. **Classify affected layers** — map the symptom to likely layers:

   | Symptom | Start here |
   |:--------|:-----------|
   | 504/timeout | DB config, connection pools, request chain |
   | 401/403 | Auth module, middleware, guards |
   | Wrong data displayed | API serialization, frontend types |
   | Slow page load | Service queries, DB indexes |
   | Post-deploy breakage | Migrations, env vars, config |
   | Works for some users | Permission checks, role definitions |
   | Type errors at runtime | Shared types, build output |

4. **Dispatch parallel Explore agents** — one per affected layer, all in a single message with `run_in_background: true`. Each agent should search for relevant code paths, error handling, config, recent git changes, and how that layer connects to others. Be specific about the symptom in each prompt.

5. **Synthesise** — trace the request path across layers, cross-reference agent findings, identify the boundary where things break.

6. **Form hypothesis** — state the root cause, which layer it's in, file:line references, and supporting evidence. Rank multiple hypotheses by likelihood.

7. **Propose fix — DO NOT IMPLEMENT.** Present:
   - Root cause (one sentence)
   - Changes required (layer, file:line, what and why)
   - Order of operations (which layer first, build order)
   - Risk assessment (impact, rollback, testing)

   **Stop here.** Wait for user approval.

## Red Flags

| Thought | Reality |
|:--------|:--------|
| "I know it's in one layer" | Cross-boundary bugs hide at interfaces. Check all layers. |
| "Let me check one service first" | Parallel agents are faster. Dispatch all at once. |
| "I'll fix it and see if it works" | Evidence first, propose fix, get approval. |
| "The error message says X" | Error messages describe symptoms, not causes. Trace the flow. |
