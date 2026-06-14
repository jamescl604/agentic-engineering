# Facilitator Guide

This guide helps a team lead, manager, or senior engineer run the training with new employees.

## Recommended Cohort Size

Use cohorts of 2 to 5 learners.

This is large enough to practice collaboration and review, but small enough that feedback stays focused.

## Setup

Before training starts:

1. Create or simulate the Acme Orders repo.
2. Add `AGENTS.md` and `CLAUDE.md` to the practice repo.
3. Add the sample backlog as issues or issue documents.
4. Create or simulate a GitHub Project with the training fields.
5. Decide which commands count as validation in the practice repo.
6. Define the simulated release path and release evidence expected.
7. Assign one facilitator as reviewer, release owner, and safety approver.

The practice repo does not need to be complex. It needs enough surface area for domain code, tests, API shape, jobs, config, high-risk files, Project state, and release tracking.

## Suggested Schedule

### Day 0: Setup and Workflow Access

- Stage 0
- Exercise 0

### Day 1: Orientation and Issue Shaping

- Stage 1
- Stage 2
- Exercises 1 and 2

### Day 2: Plan-First Implementation

- Stage 3
- Exercises 3 and 4

### Day 3: PR Hygiene and Review

- Stage 4
- Exercises 5 and 9

### Day 4: Collaboration and Safety

- Stage 5
- Stage 6
- Exercises 6, 7, and 8

### Day 5: High-Throughput Simulation

- Stage 7
- Exercise 10

### Day 6: Release and Cleanup

- Stage 8
- Exercise 11
- Rubric review

For part-time onboarding, run the same sequence over two weeks.

## Facilitator Review Prompts

Use these prompts during feedback:

- What did you intentionally keep out of scope?
- Which files did you expect to change, and why?
- Which files would require approval before editing?
- What validation actually ran?
- What did you choose not to run?
- What GitHub state changed, and why?
- Is this Ready to Merge, Ready for Release, Released, or Done?
- What would make this PR hard to review?
- What could collide with another developer's work?
- What production risk remains after tests pass?
- What release evidence will prove the change landed safely?
- What cleanup remains after release?
- What should wait because review capacity is limited?

## Common Coaching Moments

### Learner starts coding too early

Ask them to stop and produce an issue, scope, and plan first.

### Learner writes a broad prompt

Ask them to add files in scope, files to avoid, approval gates, and validation expectations.

### Learner overclaims validation

Ask them to rewrite the validation note using only commands actually run.

### Learner ignores collision risk

Ask them to list likely changed files and compare against other active work.

### Learner wants to change dependencies

Ask for an approval request and read-only investigation plan before edits.

### Learner treats PR approval as done

Ask them to walk the item through release readiness, release evidence, issue updates, Project updates, branch cleanup, and worktree cleanup.

## Graduation Review

The final review should include:

- one agent-ready issue written by the learner
- one plan-first prompt
- one small implementation artifact
- one PR description
- one validation evidence note
- one peer review
- one safety or rollout assessment
- one end-to-end delivery log
- one Project state transition record
- one release evidence note
- completed rubric scores

Do not graduate learners based only on speed. Graduate them when they can move quickly while preserving scope, evidence, reviewability, and safety.
