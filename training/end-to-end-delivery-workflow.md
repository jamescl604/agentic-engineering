# End-to-End Delivery Workflow

Agentic engineering is not finished when a PR opens. A production contributor should be able to work a backlog item from intake through release, verification, and cleanup.

Use this workflow for Acme Orders training and adapt it to the production repo.

## Lifecycle

```text
Backlog item
-> refinement
-> Ready for Agent
-> assignee assigned
-> branch and worktree created
-> plan-first implementation
-> local validation
-> PR opened
-> review and CI
-> ready to merge
-> release readiness
-> release
-> post-release verification
-> cleanup and learning capture
```

## 1. Intake and Refinement

Start from a vague request, bug report, roadmap item, or support pain.

Before implementation, the accountable assignee should make the work agent-ready:

- goal
- scope
- out of scope
- acceptance criteria
- likely files/modules
- files/modules to avoid
- coordination or risk notes when relevant
- suggested checks
- release or rollout expectations
- suggested branch/worktree

Update GitHub state:

- `Status`: Ready for Refinement, then Ready for Agent when complete
- labels: readiness, area, priority, and `high-risk` when needed
- milestone: release target or none

## 2. Ownership and Work Start

When a learner starts work:

- assign the GitHub issue to the accountable owner
- set `Status` to In Progress
- confirm WIP limit allows the work

The assignee is accountable for agent instructions, scope control, validation, review response, and release follow-through.

## 3. Planning Before Editing

The first Claude Code prompt should ask for inspection and plan only.

The plan should identify:

- files likely to change
- files to avoid
- tests to add or update
- validation commands
- coordination or risk notes
- high-risk files or approval gates
- release or rollout impact

Do not approve implementation until the plan fits the issue scope.

## 4. Implementation and Local Validation

During implementation:

- keep changes narrow
- avoid unrelated cleanup
- run focused validation first
- broaden validation based on risk
- record exact commands and results

If validation fails, decide whether the failure is in scope before expanding the task.

## 5. PR, Review, and CI

Open a draft PR early when collaboration or visibility helps.

Before marking ready for review:

- PR summary is factual
- issue is linked
- scope and out of scope are clear
- validation evidence is recorded
- risks, rollout, or coordination notes are stated when relevant
- rollout and rollback notes are present when relevant

Update GitHub state:

- `Status`: In Review
- linked PR is visible from the issue or Project item

Review is not a formality. The human reviewer should check correctness, risk, test coverage, validation claims, and operational impact.

## 6. Ready to Merge

A PR is ready to merge only when:

- CI is green or failures are explicitly accepted by the responsible human
- required reviews are complete
- CODEOWNERS or sensitive-area reviewers approved when required
- issue acceptance criteria are met
- Project status, labels, milestone, and linked PR are current
- release impact is understood

Update the GitHub Project:

- `Status`: Ready to Merge

Do not merge if rollout, migration, or verification details are missing for a change that needs them.

## 7. Release Readiness

For low-risk changes, release readiness may be simple:

- merged to main
- CI passed
- included in normal deployment
- no special rollout needed

For production-risk changes, confirm:

- release owner
- release target
- feature flag state
- migration order
- monitoring signals
- rollback path
- support or incident owner
- customer or internal communication if needed

Update GitHub state:

- `Status`: Ready for Release
- milestone or release notes identify the release target

## 8. Release and Post-Release Verification

After release:

- confirm deployment completed
- check expected monitoring signals
- verify the acceptance criteria in the released environment when appropriate
- watch for regressions in the affected area
- record release evidence in the issue or PR

Examples of release evidence:

```text
Released:
- deployed in release 2026.06.14.2
- CI passed before merge
- smoke check completed in staging
- production dashboard checked for fulfillment retry errors
- no rollback needed
```

Update GitHub state:

- `Status`: Released or Done, depending on team convention

## 9. Cleanup and Learning Capture

After release verification:

- close or update the issue
- move project item to Done
- remove local worktree
- delete merged branch when appropriate
- capture durable lessons in `AGENTS.md`, `CLAUDE.md`, docs, or templates only when the lesson should apply again
- note follow-up issues instead of hiding unfinished work

Cleanup is part of throughput. Stale branches, stale project state, and undocumented follow-ups slow the next person down.
