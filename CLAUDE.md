# CLAUDE.md

## Project Overview

Claude Code plugin (`bb`) — a collection of opinionated developer workflow skills. Distributed via the Claude Code plugin system.

**Repo:** `brandonrbridges/claude-skills`
**Install:** `/install-plugin brandonrbridges/claude-skills`
**Prefix:** All skills are invoked as `/bb:{skill-name}`

## Structure

```
.claude-plugin/
  plugin.json         # Plugin metadata, version (semver)
skills/
  {skill-name}/
    SKILL.md           # Skill definition (frontmatter: name, description)
README.md              # Public-facing docs
CHANGELOG.md           # All notable changes
```

## Rules

### Every Change Must Update

When adding, renaming, removing, or modifying any skill:

1. **The skill itself** — `skills/{name}/SKILL.md`
2. **`skills/help/SKILL.md`** — update the commands table
3. **`README.md`** — update both the commands table and the skills section
4. **`CHANGELOG.md`** — add entry under the current version
5. **`.claude-plugin/plugin.json`** — bump version
6. **`README.md` badge** — update the version badge to match `plugin.json`

### Versioning

Follow semver strictly:
- **Patch** (1.2.x) — prompt refinements, typo fixes, no behavior change
- **Minor** (1.x.0) — new skill added, skill renamed, notable behavior change
- **Major** (x.0.0) — breaking changes (renamed prefix, removed skills)

### Skill Standards

- Keep skills **under 120 lines**. If it's longer, it's doing too much.
- Frontmatter has only `name` and `description`. Description starts with "Use when..."
- Skills are opinionated and direct — tell the agent what to do, not how to think
- No component API reference dumps — trust the agent to look things up
- No AI attribution in any commit messages

### Commit Convention

Use conventional commits. No `Co-Authored-By`. No AI attribution. Commit and push after changes are complete.
