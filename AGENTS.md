# AGENTS.md

## Repository Goal

This repository exists to refine best practices for using Claude Code and AI agents on production software repositories with multiple human developers.

The focus is practical agentic engineering: workflows, guardrails, coordination patterns, review practices, and validation habits that help teams ship small, safe, maintainable changes without creating chaos.

## Repository Map

- `playbook/` contains the playbook entry point (`README.md`) and all chapter files: operating model, GitHub coordination, worktrees, validation, PR hygiene, safety, production risk, and multi-human collaboration.
- `training/` contains the curriculum, exercises, facilitator guide, rubric, setup instructions, and end-to-end delivery workflow for teaching the playbook.
- `training/acme-orders-practice-repo.md` documents the companion practice repo and how it supports the training.

## Companion Acme Practice Repo

- The hands-on training repo lives outside this workspace at `C:\Repos\acme-orders-agentic-training`.
- GitHub repo: `https://github.com/jamescl604/acme-orders-agentic-training`.
- GitHub Project: `Acme Orders Delivery`, user project `https://github.com/users/jamescl604/projects/1`.
- The Acme repo is a deliberately production-like TypeScript service used for cohort exercises, not a toy snippet collection.
- Its GitHub setup includes labels, quarterly release milestones, a two-week `Sprint` iteration field, a lean Project view set, PR template, CODEOWNERS, and CI.
- The live Project views are intentionally pragmatic: `Status Board`, `Triage`, `Ready`, `Current Sprint`, `Schedule`, `Review Queue`, and `Next Release`.
- The `Schedule` view is a roadmap using Sprint start/end dates so teams can see booked work across upcoming sprints.
- Reset/setup behavior for future cohorts is maintained in `C:\Repos\acme-orders-agentic-training\scripts\setup-github.ps1`.

## Current Workflow Model

- Use GitHub-native state wherever possible: issues, assignees, labels, milestones, linked PRs, reviewers, checks, releases, and Project status.
- Use a Project `Sprint` iteration field only because the training scenario assumes real two-week sprint planning.
- Use release milestones for quarterly release planning, currently modeled as `2026 Q3 Release`, `2026 Q4 Release`, and `2027 Q1 Release`.
- Keep saved Project views lean. Prefer ad hoc filters over permanent views for occasional questions like area, high-risk, next sprint, unassigned, or unscheduled work.
- For in-room assignment, use the `Ready` view filtered to scoped unassigned work and assign the volunteer in the native `Assignees` cell.

## Guidance for Future Agent Sessions

- Treat this repo as a playbook and working notes space for production-oriented agentic engineering.
- Preserve the emphasis on multiple developers, shared repositories, GitHub coordination, worktrees, CI, and human review.
- Prefer concrete, repeatable practices over abstract AI commentary.
- Keep recommendations grounded in real engineering constraints: ownership, scope control, merge safety, review bandwidth, test coverage, and operational risk.
- When editing docs, keep the tone pragmatic, direct, and useful to teams adopting Claude Code or similar coding agents.
- Avoid presenting agents as replacements for engineering judgment. Humans own product, architecture, risk, review, and merge decisions.

## Working Style

- Read the existing playbook before making substantial changes.
- Keep changes focused and easy to review.
- Do not introduce broad restructures unless the user explicitly asks for them.
- Prefer small documentation additions that can become team norms, templates, or checklists.
- If adding examples, make them production-repo oriented rather than toy examples.
