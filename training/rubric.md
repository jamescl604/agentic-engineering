# Assessment Rubric

Use this rubric before a learner starts supervised production-repo agentic work.

Score each area from 1 to 4.

| Score | Meaning |
|---:|---|
| 1 | Needs close supervision |
| 2 | Can complete simple tasks with guidance |
| 3 | Ready for supervised production work |
| 4 | Ready to mentor others |

## 1. Operating Model

The learner understands that humans own decisions and agents execute bounded work.

Score 3 evidence:

- explains human ownership of scope, architecture, risk, review, and merge
- can identify when agents should not be the primary actor
- avoids treating agent output as authoritative without review

## 2. Setup and Prerequisites

The learner can get into the workflow without unsafe shortcuts.

Score 3 evidence:

- verifies required local tools and GitHub CLI access
- can clone/open the practice repo and create a branch/worktree
- can view issues, Project status, labels, milestones, PR checks, and CI status
- documents missing access or substitutes instead of bypassing process

## 3. GitHub Project Workflow

The learner can use GitHub Projects as the control layer for work beyond PRs.

Score 3 evidence:

- updates status, assignees, labels, milestone, and linked PRs
- keeps issue state, PR state, and Project state aligned
- distinguishes Ready for Agent, In Progress, In Review, Ready to Merge, Ready for Release, Released, and Done
- does not mark work Done before release verification and cleanup are complete

## 4. Task Framing

The learner can turn vague requests into agent-ready work.

Score 3 evidence:

- defines goal, scope, out of scope, acceptance criteria, likely files, files to avoid, coordination notes, and suggested checks
- splits broad work into PR-sized issues
- asks product or architecture questions instead of guessing

## 5. Plan-First Agent Direction

The learner can direct Claude Code safely before implementation.

Score 3 evidence:

- writes prompts with scope, avoid lists, approval gates, and validation expectations
- reviews the plan before allowing edits
- narrows or rejects unsafe plans

## 6. Git Isolation

The learner can use branches and worktrees without creating local chaos.

Score 3 evidence:

- uses one branch and worktree per backlog item
- checks status and diff before continuing work
- avoids running multiple agent sessions in the same directory
- cleans up merged or abandoned worktrees

## 7. PR Hygiene

The learner can produce reviewable PRs.

Score 3 evidence:

- keeps diffs small
- explains scope and out of scope
- records Claude Code usage accurately
- calls out risks and follow-up work

## 8. Validation Evidence

The learner reports validation honestly.

Score 3 evidence:

- records exact commands run and results
- distinguishes run commands from recommended commands
- discloses skipped checks
- includes relevant failures instead of hiding them

## 9. Collaboration and Collision Control

The learner can work with other humans using agents in the same repo.

Score 3 evidence:

- identifies likely changed files before implementation
- identifies coordination risk
- chooses safe parallel work
- serializes shared contracts, migrations, auth, infrastructure, and broad refactors

## 10. Safety and Permissions

The learner respects production boundaries.

Score 3 evidence:

- asks before dependency, lockfile, migration, auth, CI/CD, deployment, or generated-file changes
- avoids exposing secrets and sensitive data
- asks before destructive commands
- treats MCP write access as high risk

## 11. Rollout and Production Risk

The learner can identify when a change needs rollout planning.

Score 3 evidence:

- identifies production-risk changes
- drafts rollout and rollback notes
- considers observability and support ownership
- does not hide hard rollback risk

## 12. End-to-End Delivery

The learner can carry work from backlog through release and cleanup.

Score 3 evidence:

- moves a backlog item through refinement, ownership, implementation, review, merge readiness, release readiness, release, verification, and cleanup
- records release evidence after simulated deployment
- creates follow-up issues for unfinished work
- cleans up branches and worktrees
- captures durable lessons only when they should apply again

## 13. Throughput Discipline

The learner can move quickly without outrunning review or validation.

Score 3 evidence:

- keeps personal WIP limits
- reviews before starting too much new work
- optimizes for merged, reviewed changes rather than agent session count
- can explain what not to start

## Graduation Standard

Ready for supervised production-repo agentic work:

- score of 3 or higher in every area
- at least one completed low-risk implementation exercise
- at least one completed peer review exercise
- at least one completed backlog-to-release simulation
- no unresolved safety failures

Ready to mentor others:

- score of 4 in at least seven areas
- demonstrated ability to coach another learner through scope, validation, and review
- consistently identifies production risk before implementation
- consistently keeps project state, release state, and cleanup aligned
