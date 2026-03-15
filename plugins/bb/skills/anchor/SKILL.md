---
name: anchor
description: Use when starting complex tasks, during long sessions, or when you sense context is getting large. Checkpoints your understanding so it survives context compression and prevents scope drift.
---

# Anchor

Checkpoint your state so you don't lose track. Context compression will erase your earlier reasoning — anchors survive it.

## When to Anchor

- **Start of a complex task** — before writing any code
- **After a major milestone** — feature complete, moving to next step
- **Before dispatching agents** — record what you need from them
- **When the conversation feels long** — if you've made 20+ tool calls, anchor
- **After receiving agent results** — distill findings before they compress away

## How

Create a task with `TaskCreate` titled `ANCHOR: {what you're working on}` containing:

```
## Task
[What the user asked for — exact scope, no embellishment]

## Decisions
[Key decisions made so far — tech choices, approach, things ruled out]

## Done
[What's been completed — files changed, features implemented]

## Remaining
[What's left to do]

## Files Touched
[List of files you've modified this session]

## Watch Out
[Anything you discovered that matters — edge cases, gotchas, user preferences mentioned]
```

## Rules

- **One anchor at a time.** Update the existing anchor, don't create new ones. Use `TaskUpdate` to replace content.
- **Be terse.** Anchors are for your future self, not the user. File paths and bullet points, not prose.
- **Scope is sacred.** Before starting any new work, re-read your anchor. If what you're about to do isn't in "Remaining", stop and check with the user.
- **Files Touched is critical.** This is how `/bb:commit` and `/bb:sweep` know what's yours. Keep it current.
- **Don't anchor trivial tasks.** Single-file fixes don't need checkpointing.

## Scope Check

Before doing anything substantial, ask yourself:

1. Re-read the anchor's "Task" section
2. Is what I'm about to do within that scope?
3. If no — mention it to the user, don't silently expand

This is the anchor's second job: preventing drift. You will be tempted to fix adjacent issues. The anchor reminds you what you were actually asked to do.
