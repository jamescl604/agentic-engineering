# Multi-Human Team Collaboration

When multiple developers use Claude Code in the same repo, the main risks are:

- file collisions
- duplicated work
- broad overlapping refactors
- stale branches
- AI-generated diffs moving faster than review
- unclear assignment and accountability

## Team Rule

> One backlog item -> one accountable assignee -> one branch -> one worktree -> one PR.

The assigned human is accountable for:

- scope
- branch/worktree
- Claude instructions
- reviewing the diff
- resolving conflicts
- opening the PR
- responding to review
- final quality

## File Ownership

Before starting, record likely files/modules.

Example:

```text
Owner: James
Issue: #1234 CSV import row validation
Branch: feature/1234-csv-import-row-validation
Worktree: myrepo-feature-1234-csv-import-row-validation

Scope:
- src/import/validation/*
- tests/import/validation/*

Avoid:
- src/import/ui/*
- src/api/customer/*
- package.json

Coordination Notes:
Import UI work is happening separately. Avoid src/import/ui/*.
```

## Treat These Files as High Risk

Require explicit human approval before Claude edits:

- package manifests and lock files
- database migrations
- API schemas/contracts
- generated clients
- route registries
- global config
- auth/security middleware
- CI/CD workflows
- shared fixtures
- design system components

See [Safety and permissions](safety-and-permissions.md) for the broader production permission model.

## Collision Avoidance

Before implementation, ask Claude:

```text
Inspect this issue and repo.

Identify:
1. likely files to change
2. shared or high-collision files
3. other active work likely to conflict
4. whether this should be split
5. safest implementation order

Do not edit files.
```

Before PR, ask:

```text
Review this branch for collision risk.

Check:
- shared files changed
- broad refactors
- formatting-only changes
- lock file changes
- generated files
- migration ordering
- likely merge conflicts with main

Do not edit files.
```
