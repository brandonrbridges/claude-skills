# Changelog

All notable changes to this project will be documented in this file.

## [1.1.0] - 2026-03-15

### Changed
- Renamed `/bb:clean-tree` to `/bb:sweep` — shorter, more memorable
- Renamed `/bb:preflight` to `/bb:check` — simpler, says what it does
- Renamed `/bb:investigate` to `/bb:diagnose` — signals structured bug-finding
- Refined all skill prompts — smaller, more focused, less hand-holding

## [1.0.0] - 2026-03-15

### Added
- `/bb:auto` — Full autonomous mode with CLAUDE.md as sole guardrail
- `/bb:commit` — Smart conventional commits, agent-only files, rebase + push
- `/bb:sweep` — Organise messy working trees into logical grouped commits
- `/bb:check` — Run type-check, lint, build with fix options
- `/bb:pr` — Generate and create pull requests from branch commits
- `/bb:diagnose` — Cross-codebase bug diagnosis with parallel agents
- `/bb:help` — List all available commands
