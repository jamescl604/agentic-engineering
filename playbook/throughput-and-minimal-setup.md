# Throughput and Minimal Setup

Agentic engineering moves the bottleneck from typing code to:

- scoping good work
- managing WIP
- reviewing diffs
- validating behavior
- resolving integration conflicts
- deciding what to merge

A team with 20 agents and poor review discipline will go slower than a team with 3 well-scoped Claude Code workstreams and fast review.

## Right-Sizing the Process

Not every change needs the full workflow. Match the process to the risk.

| Change Type | What You Need | What You Can Skip |
|---|---|---|
| Typo fix, comment update | Branch, PR, CI | Plan-first, issue, validation evidence, rollout notes |
| Small isolated bug fix | Branch, PR, CI, focused test, brief validation note | Formal issue template, collision analysis, rollout planning |
| Normal feature or refactor | Issue, plan-first, branch/worktree, PR, validation evidence, review | Rollout checklist, release evidence |
| High-risk or production-risk change | Everything: issue, plan, worktree, validation, review, rollout, rollback, release evidence | Nothing |

Rules of thumb:

- If the change is smaller than the issue template, skip the template.
- If the plan would take longer than the implementation, skip the plan.
- If a senior engineer could review the diff in under 60 seconds, a short PR description is fine.
- If the change touches only one file and has no behavioral impact, don't create ceremony around it.

The goal is to match the weight of the process to the weight of the risk. A one-line docs fix going through an 8-step workflow is a signal that the team adopted the playbook too literally.

## Lightweight Team Rhythm

### Backlog Refinement

Goal: turn vague ideas into agent-ready issues.

Do:

- split large work
- define scope/out-of-scope
- identify collision risk
- add acceptance criteria
- add validation expectations

### Daily Planning

Goal: choose safe parallel work.

Do:

- pick from Ready for Agent
- avoid too many items in the same area or shared contract
- assign one accountable assignee per issue
- create worktrees for selected work

### Implementation

Goal: execute bounded work.

Do:

- one worktree per issue
- plan-first Claude workflow
- focused tests first
- human diff review

### Review

Goal: convert generated changes into trusted changes.

Do:

- linked PR
- CI
- CODEOWNERS for risky files
- small PRs
- fast feedback

### Cleanup

Goal: keep local and GitHub state clean.

Do:

- remove merged worktrees
- delete branches
- close/update issues
- update `CLAUDE.md` only for durable lessons

## Picking Parallel Work

Good parallel set:

```text
Feature A: backend import validation
Feature B: settings page UI
Bug C: auth timeout logging
```

Risky parallel set:

```text
Feature A: import validation backend
Feature B: import preview UI
Feature C: import API rewrite
```

Parallelize across separate modules. Serialize shared contracts, schema changes, auth, infrastructure, and broad refactors.

## Merge Risk Order

Usually merge in this order:

1. docs-only
2. tests-only
3. isolated bug fixes
4. isolated features
5. API contract changes
6. database migrations
7. broad refactors

Avoid merging two branches that modify the same contract without deliberate reconciliation.

## Recommended Minimal Setup

If you want the simplest version that still manages the top risks, do this:

1. Add a good `AGENTS.md`.
2. Add a focused `CLAUDE.md` when Claude Code is the primary tool.
3. Create a GitHub Project with:
   - Status
   - Assignees
   - Labels
   - Milestone
   - Linked pull requests
   - Reviewers
4. Create an agent-ready issue template.
5. Create a Claude-assisted PR template.
6. Use one worktree per issue.
7. Use one Claude session per worktree.
8. Require plan-first for non-trivial changes.
9. Treat shared files as high risk.
10. Require validation evidence.
11. Protect `main`.
12. Keep PRs small.

Use labels for area and priority rather than custom Project fields. Use milestones and GitHub Releases for release targeting instead of duplicating release state in the Project.

Everything else can be added later when pain justifies it.

## Adopting in an Existing Repo

Most teams are not starting from scratch. They have an existing codebase, established CI, existing issues, and developers who already have workflows. Adopting agentic engineering incrementally works better than trying to restructure everything at once.

### Week 1: Foundations

1. Add an `AGENTS.md` with repo goal, architecture summary, coding standards, and high-risk files.
2. Add a `CLAUDE.md` with setup, build, test, and lint commands.
3. Identify 3-5 files or directories that require human approval before agent edits (migrations, auth, CI, API contracts, dependency manifests).
4. Pick one low-risk issue for a pilot agent workflow.

Do not reorganize the backlog, change the Project layout, or introduce new labels yet.

### Week 2: First Workflow

5. Run the pilot issue through the agent workflow: plan-first prompt, scoped implementation, validation evidence, PR with honest notes.
6. Have the team review the PR together. Discuss what was useful and what felt ceremonial.
7. Add a PR template section for agent-assisted changes (summary, scope, validation, risk).
8. If the team uses branches already, try one worktree to compare.

### Week 3-4: Team Adoption

9. Add `ready-for-agent` and `high-risk` labels when the team agrees on what they mean.
10. Introduce the agent-ready issue template for new work. Do not backfill existing issues.
11. Set a personal WIP limit for each developer doing agent-assisted work.
12. Start requiring validation evidence in agent-assisted PRs.

### After the First Month

Evaluate what's working. Common signals that more structure is needed:

- PRs are getting rejected for scope creep → add collision pre-checks and stricter scope prompts.
- Agent PRs are too large to review → enforce smaller issues and tighter scope.
- Merge conflicts are increasing → serialize work on shared contracts and add coordination notes.
- High-risk files are being edited without review → add CODEOWNERS for those paths.
- Release tracking is unclear → add Ready to Merge / Ready for Release statuses.

Common signals that you have too much structure:

- Trivial changes are going through the full issue template → use right-sizing guidance.
- Developers are skipping the process because it's too heavy → reduce required fields.
- The Project board has views nobody uses → remove them.
