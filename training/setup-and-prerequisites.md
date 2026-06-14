# Setup and Prerequisites

This training starts before the first agent prompt. A new employee needs the local tools, repository access, GitHub workflow context, and safety boundaries required to work a backlog item end to end.

## Required Access

Before training, confirm the learner has:

- GitHub account access
- access to the practice repo
- access to the training GitHub Project or simulated project board
- permission to create branches
- permission to open draft PRs
- permission to view CI results
- access to team docs for architecture, testing, release, and incident process
- read access to relevant issue labels, milestones, and project fields

Do not grant production write access just to complete training. Use the Acme Orders practice repo or a sandbox.

## Required Local Tools

Install and verify:

- Git
- GitHub CLI
- Claude Code
- Node.js and npm, if using the suggested Acme Orders shape
- VS Code or the team's standard editor
- any repo-specific package manager or runtime used by the practice repo

Verification commands:

```bash
git --version
gh --version
claude --version
node --version
npm --version
```

If a tool is unavailable, record that in the setup checklist and use a facilitator-approved substitute.

## GitHub CLI Setup

Authenticate and confirm access:

```bash
gh auth login
gh auth status
gh repo view OWNER/acme-orders
```

The learner should be able to:

- view issues
- view project fields or the simulated project board
- create branches locally
- open draft PRs
- inspect PR checks

## Git Setup

Configure identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Confirm the learner can:

- clone the practice repo
- create a branch
- create a worktree
- inspect status and diff
- push a branch to the training remote when appropriate

## GitHub Project Setup

The training project should use the same GitHub-native state used in the production workflow:

| GitHub State | Training Use |
|---|---|
| Status | move work from Backlog through Done |
| Assignees | assign accountable owner and collaborators |
| Labels | mark readiness, area, priority, and high-risk work |
| Milestone | connect issues and PRs to release planning |
| Sprint | assign work to the current two-week iteration |
| Linked pull requests | show implementation and review status |
| Reviewers | show review ownership |

Do not add Project fields for local-only or easily inferred details such as worktree name, branch name, AI usage, PR size, validation commands, or release state. Use labels, milestones, linked PRs, PR templates, checks, and releases for those details.

Recommended training views should expose `Assignees` wherever learners choose or discuss work. Keep the saved views lean: `Status Board`, `Triage`, `Ready`, `Current Sprint`, `Schedule`, `Review Queue`, and `Next Release`. Use `Ready` filtered to `is:open label:"ready-for-agent" no:assignee` when someone volunteers. Assign the volunteer in the `Assignees` cell, then move the item to `In Progress` only when implementation starts. Use `Schedule` as the roadmap view for sprint booking.

Recommended training statuses:

```text
Backlog
-> Ready for Refinement
-> Ready for Agent
-> In Progress
-> In Review
-> Ready to Merge
-> Ready for Release
-> Released
-> Done
```

Use `Done` only after release notes, cleanup, and issue/project updates are complete.

## Sandbox Safety Rules

Even in training, learners should practice production habits:

- no secrets in prompts or commits
- no dependency changes without approval
- no lockfile edits without approval
- no destructive git commands without approval
- no migration-like changes without rollout and rollback notes
- no self-merge for final exercises
- no claiming validation passed unless it was run

## Setup Checklist

The learner should complete this checklist before Stage 1:

```text
Learner:
Practice repo:
Training Project:

Access confirmed:
- GitHub account
- repo read/write sandbox access
- project access
- CI visibility

Tools verified:
- git --version
- gh --version
- claude --version
- runtime/package manager versions

Local workflow verified:
- cloned repo
- created branch
- created worktree
- ran test command or documented substitute

GitHub workflow verified:
- viewed issue
- viewed project fields
- opened or drafted a training PR
- viewed PR checks
```
