# bb

A Claude Code plugin for developers who want their agent to move fast, commit clean, and diagnose problems properly.

## Install

```
/install-plugin brandonrbridges/claude-skills
```

All commands are prefixed with `bb:` — e.g. `/bb:commit`, `/bb:auto`.

## Skills

### `/bb:auto` — Autonomous mode

Switches Claude to full autonomy. No more "shall I proceed?" or "should I commit?". CLAUDE.md becomes the sole guardrail — every coding standard and convention still applies, you just stop getting asked permission for every action.

Still pauses for genuinely dangerous operations (force push, deleting branches, posting externally).

### `/bb:commit` — Smart commits

Commits only the files you changed this session using conventional commit format. Compares against the git status snapshot from session start so it never touches files that aren't yours.

Rebases on remote and pushes. No AI attribution, no "Co-Authored-By", no "as requested" — commits read like a human wrote them.

### `/bb:clean-tree` — Organise and commit everything

For when your working tree is a mess — maybe multiple agents ran, maybe changes piled up. Reads every dirty file, groups them by intent into logical commits, orders them by dependency (shared packages first, then API, then frontend), and pushes.

Matches your existing commit style from `git log`.

### `/bb:preflight` — Codebase health check

Runs type-check, lint, and build in parallel. Reports every error and warning, then offers three options:

1. **Fix everything** — all errors and warnings, committed as a clean slate
2. **Fix build-blockers only** — just type errors and lint errors
3. **Proceed anyway** — carry on knowing what's broken

### `/bb:pr` — Create a pull request

Generates a PR from your branch commits. Reads the full diff (not just the latest commit), writes a human-readable title and summary, pushes, and creates via `gh`.

### `/bb:investigate` — Cross-codebase bug diagnosis

For bugs that cross service boundaries. Dispatches parallel Explore agents across all affected packages/services simultaneously, synthesises findings, and proposes a fix with evidence — but doesn't implement until you approve.

Works with any project structure: monorepos, multi-repo setups, or single repos with multiple layers.

## Philosophy

These skills share a few opinions:

- **No AI fingerprints.** Commits, PRs, and code should read like a human wrote them.
- **Move fast within guardrails.** Autonomy means speed, not recklessness. Project standards always apply.
- **Surface everything, then let the user decide.** Preflight and investigate show the full picture before acting.
- **Logical grouping over mechanical grouping.** Commits are organised by intent, not by file type.

## License

MIT
