<div align="center">

# bb

**Developer workflow skills for [Claude Code](https://github.com/anthropics/claude-code)**

Move fast. Commit clean. Diagnose properly.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Plugin Version](https://img.shields.io/badge/version-1.0.0-green.svg?style=flat-square)](.claude-plugin/plugin.json)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-8B5CF6.svg?style=flat-square)](https://github.com/anthropics/claude-code)

</div>

---

## Install

```
/install-plugin brandonrbridges/claude-skills
```

All commands are prefixed with `bb:`. Run `/bb:help` for a quick reference.

---

## Commands

| Command | Purpose |
|:--------|:--------|
| [`/bb:auto`](#bbauto) | Full autonomous mode — stop asking, start doing |
| [`/bb:commit`](#bbcommit) | Conventional commits, agent-only files, rebase + push |
| [`/bb:clean-tree`](#bbclean-tree) | Organise a messy tree into logical grouped commits |
| [`/bb:preflight`](#bbpreflight) | Type-check, lint, build — report issues with fix options |
| [`/bb:pr`](#bbpr) | Create a pull request from branch commits |
| [`/bb:investigate`](#bbinvestigate) | Diagnose bugs across services and packages |

---

## Skills

### `/bb:auto`

Switches Claude to full autonomy. No more "shall I proceed?" — CLAUDE.md becomes the sole guardrail. Every coding standard and convention still applies, you just stop getting asked permission for every action.

Still pauses for genuinely dangerous operations: force push, deleting branches, posting externally.

### `/bb:commit`

Commits only the files you changed this session using conventional commit format. Compares against the git status snapshot from session start so it never touches files that aren't yours.

Rebases on remote and pushes. No AI attribution, no `Co-Authored-By`, no "as requested" — commits read like a human wrote them.

### `/bb:clean-tree`

For when your working tree is a mess — maybe multiple agents ran, maybe changes piled up. Reads every dirty file, groups them by intent into logical commits, orders them by dependency (shared packages first, then API, then frontend), and pushes.

Matches your existing commit style from `git log`.

### `/bb:preflight`

Runs type-check, lint, and build in parallel. Reports every error and warning, then asks what you want to do:

| Option | What happens |
|:-------|:-------------|
| **Fix everything** | All errors and warnings resolved, committed as a clean slate |
| **Fix build-blockers only** | Type errors and lint errors fixed, warnings left alone |
| **Proceed anyway** | Nothing changes — you continue with awareness of what's broken |

### `/bb:pr`

Generates a PR from your branch commits. Reads the full diff — not just the latest commit — writes a human-readable title and summary, pushes, and creates via `gh`.

### `/bb:investigate`

For bugs that cross service boundaries. Dispatches parallel Explore agents across all affected packages and services simultaneously, synthesises findings, and proposes a fix with evidence — but doesn't implement until you approve.

Works with any project structure: monorepos, multi-repo setups, or single repos with multiple layers.

---

## Philosophy

> These skills are opinionated. They share a point of view about how agents should work with your code.

**No AI fingerprints.** Commits, PRs, and code should read like a human wrote them.

**Move fast within guardrails.** Autonomy means speed, not recklessness. Project standards always apply.

**Surface everything, then let the user decide.** Preflight and investigate show the full picture before acting.

**Logical grouping over mechanical grouping.** Commits are organised by intent, not by file type.

---

## Contributing

If you've got a skill that fits the philosophy — opinionated workflow discipline, no AI fingerprints, surface-then-decide — PRs are welcome.

## Versioning

This plugin follows [semver](https://semver.org). Bump the version in `.claude-plugin/plugin.json` and add an entry to [`CHANGELOG.md`](CHANGELOG.md) when updating skills.

---

<div align="center">

MIT License — [Brandon Bridges](https://github.com/brandonrbridges)

</div>
