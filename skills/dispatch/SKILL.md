---
name: dispatch
description: Use when a task has multiple independent workstreams that benefit from parallel agents. Orchestrates a structured team of scouts, implementers, a reviewer, and a validator for maximum throughput.
---

# Dispatch

Orchestrate a team of parallel agents to break down and execute complex tasks. Each agent has a clear role — no overlap, no wasted context.

## The Team

| Role | Agent Type | Purpose |
|:-----|:-----------|:--------|
| **Scout** | `Explore` | Gather context — find relevant files, patterns, conventions, dependencies |
| **Implementer** | `general-purpose` | Execute a specific, independent piece of work |
| **Reviewer** | `superpowers:code-reviewer` | Review completed work against the plan and standards |
| **Validator** | `general-purpose` | Run type-check, lint, build — report pass/fail |

## Workflow

### Phase 1: Reconnaissance

Dispatch **scouts** in parallel to gather context. One scout per area of the codebase you'll need to touch.

```
Agent(subagent_type="Explore", run_in_background=true)
```

**Scout prompt template:**
```
Explore [AREA] in this project. I need to understand:
- File structure and naming conventions
- Existing patterns for [RELEVANT_PATTERN]
- Dependencies and imports that affect [SCOPE]
- Any existing code that does something similar

Report: file paths, key patterns, and anything I should reuse or avoid.
```

**Wait for all scouts to return.** Synthesise findings into a single understanding. Don't proceed until you have the full picture.

### Phase 2: Plan

Using scout findings, break the task into **independent** pieces of work. Each piece must be:

- **Self-contained** — no dependencies on other implementers' output
- **Clearly scoped** — one feature, one fix, one module
- **Testable** — you can verify it worked in isolation

Write the plan. Anchor it (`/bb:anchor`).

### Phase 3: Execute

Dispatch **implementers** in parallel — one per independent piece of work. Use worktree isolation for changes that touch the same files.

```
Agent(subagent_type="general-purpose", isolation="worktree", run_in_background=true)
```

**Implementer prompt template:**
```
Implement [SPECIFIC_TASK].

Context from scouts:
- [Relevant file paths and patterns]
- [Conventions to follow]
- [Dependencies to use]

Requirements:
- [Exact specification of what to build/change]
- Follow existing project patterns
- Write code only, no tests unless asked

Report: what you changed, file paths, any issues encountered.
```

**Critical rules:**
- Never dispatch an implementer without scout context
- Never have two implementers touching the same files (use worktrees if unavoidable)
- Each implementer gets only the context it needs — don't dump everything

### Phase 4: Review

After implementers return, dispatch the **reviewer**:

```
Agent(subagent_type="superpowers:code-reviewer")
```

The reviewer checks:
- Does each change match the plan?
- Are project conventions followed?
- Any bugs, security issues, or missing edge cases?
- Do the pieces fit together?

### Phase 5: Validate

Dispatch the **validator** to run quality gates:

```
Agent(subagent_type="general-purpose")
```

```
Run the following commands and report results:
- pnpm type-check
- pnpm lint
- pnpm build
Report pass/fail for each with any error details.
```

### Phase 6: Synthesise

You (the main agent) now:
1. Merge worktree branches if applicable
2. Fix any issues raised by reviewer or validator
3. Update the anchor with final state
4. Report to the user what was done

## When NOT to Dispatch

- **Task is sequential** — step 2 depends on step 1's output. Just do it yourself.
- **Task is small** — single file change, quick fix. Agents have overhead.
- **You don't understand the task yet** — dispatch scouts first, not implementers.
- **Files overlap** — if two pieces of work touch the same files heavily, do them sequentially.

## Agent Count Guidelines

| Task Size | Scouts | Implementers | Total Agents |
|:----------|-------:|-------------:|-------------:|
| Small (2-3 files) | 1 | 0 — do it yourself | 1 |
| Medium (1 feature) | 1-2 | 2-3 | 3-5 |
| Large (multi-feature) | 2-3 | 3-5 | 5-8 |

More agents isn't better. Each agent burns context and adds synthesis overhead. Use the minimum needed.

## Common Mistakes

| Mistake | Fix |
|:--------|:----|
| Dispatching implementers without scout context | Always scout first |
| Two agents editing the same file | Use worktree isolation or serialise |
| Giant prompts with full codebase dumps | Give each agent only what it needs |
| Skipping the reviewer | Parallel work drifts. Review catches divergence. |
| Not anchoring before dispatch | You'll forget what you asked for when results return |
