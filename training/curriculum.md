# Curriculum

This curriculum builds the learner in stages. Each stage adds one new source of production complexity: setup, repo context, scoped implementation, validation, collaboration, safety, release, and throughput.

## Stage 0: Setup and Workflow Prerequisites

Goal: make sure the learner can participate in the full workflow before touching implementation tasks.

Read:

- [Setup and prerequisites](setup-and-prerequisites.md)
- [GitHub coordination](../playbook/github-coordination.md)

Practice:

- verify local tools and GitHub CLI authentication
- clone or open the Acme Orders practice repo
- confirm access to the training GitHub Project or simulated project board
- inspect issue fields, project fields, and CI status
- create a branch and worktree in the practice repo

Exit criteria:

- learner can complete the setup checklist
- learner can explain how GitHub Issues, Projects, branches, PRs, CI, release state, and cleanup fit together

## Stage 1: Orientation

Goal: understand what agentic engineering is and what humans still own.

Read:

- [Agentic engineering overview](../playbook/agentic-engineering-overview.md)
- [Safety and permissions](../playbook/safety-and-permissions.md)

Practice:

- explain why humans own scope, architecture, risk, review, and merge decisions
- identify three examples of work that should not be agent-primary
- describe the difference between confidence and validation evidence

Exit criteria:

- learner can describe the playbook in their own words
- learner can identify when to stop and ask a human

## Stage 2: Repo Reading and Task Framing

Goal: learn to inspect before editing.

Read:

- [Claude Code operating model](../playbook/claude-code-operating-model.md)
- [GitHub coordination](../playbook/github-coordination.md)
- [End-to-end delivery workflow](end-to-end-delivery-workflow.md)
- [Acme Orders practice repo](acme-orders-practice-repo.md)

Practice:

- inspect the Acme Orders structure
- turn a vague request into an agent-ready issue
- define scope, out of scope, likely files, files to avoid, coordination notes, and suggested checks
- update GitHub state: Project status, assignees, labels, milestone, and linked PRs

Exit criteria:

- learner can produce an issue that another engineer could safely hand to Claude Code
- learner can keep the Project state aligned with the issue state

## Stage 3: Plan-First Implementation

Goal: use an agent as an engineering executor, not an uncontrolled code generator.

Read:

- [Quick reference](../playbook/quick-reference.md)
- [Validation and evidence](../playbook/validation-and-evidence.md)

Practice:

- write a plan-first prompt
- ask for implementation steps before editing
- approve, narrow, or reject the plan
- implement one small Acme Orders change
- record validation evidence

Exit criteria:

- learner can keep the agent inside scope
- learner can distinguish commands run from commands recommended

## Stage 4: Git Isolation and PR Hygiene

Goal: make small changes easy to review.

Read:

- [Worktrees and parallel work](../playbook/worktrees-and-parallel-work.md)
- [Pull request hygiene](../playbook/pull-request-hygiene.md)

Practice:

- create a branch and worktree for one Acme Orders issue
- update the issue and Project item when work starts
- produce a focused diff
- draft a PR summary using only facts from the diff and validation output
- identify risk and follow-up work

Exit criteria:

- learner can produce a reviewable PR-sized change with honest validation notes

## Stage 5: Collaboration and Collision Control

Goal: coordinate with other humans using the same repo.

Read:

- [Multi-human collaboration](../playbook/multi-human-collaboration.md)
- [Throughput and minimal setup](../playbook/throughput-and-minimal-setup.md)

Practice:

- triage three Acme Orders issues for collision risk
- split one broad request into sub-issues
- identify which issues can run in parallel and which should serialize
- review another learner's planned file scope before they implement

Exit criteria:

- learner can increase throughput without creating merge chaos

## Stage 6: Safety, Rollout, and Production Risk

Goal: learn when a technically small change is operationally risky.

Read:

- [Safety and permissions](../playbook/safety-and-permissions.md)
- [Rollout and production risk](../playbook/rollout-and-production-risk.md)

Practice:

- classify Acme Orders changes by production risk
- require approval for high-risk files
- draft a rollout checklist for a feature-flagged change
- draft a rollback note for a migration-like change

Exit criteria:

- learner can explain why safety is a throughput multiplier, not a slowdown

## Stage 7: High-Throughput Simulation

Goal: operate like a production team member under realistic WIP, review, and collision constraints.

Practice:

- run two independent Acme Orders workstreams with separate worktrees
- review one peer PR before starting another implementation task
- record validation evidence for each PR
- keep a personal WIP limit
- decide what not to start because review capacity is constrained

Exit criteria:

- learner can deliver multiple small, safe, reviewed changes without losing control of scope or quality

## Stage 8: End-to-End Release and Cleanup

Goal: carry a backlog item beyond PR approval through release verification and cleanup.

Read:

- [End-to-end delivery workflow](end-to-end-delivery-workflow.md)
- [Rollout and production risk](../playbook/rollout-and-production-risk.md)

Practice:

- move an Acme Orders issue through the Project lifecycle from Backlog to Done
- record merge readiness and release readiness separately
- draft release evidence for a low-risk change
- draft rollout and rollback notes for a production-risk change
- update follow-up issues instead of hiding unfinished work
- clean up branch and worktree after completion

Exit criteria:

- learner can work one backlog item end to end through release, verification, project updates, and cleanup

## Graduation

A learner is ready for supervised production-repo agentic work when they can pass the [Assessment rubric](rubric.md), complete one end-to-end workflow simulation, and explain tradeoffs without prompting.
