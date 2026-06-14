# GitHub Coordination

GitHub Projects should be the lightweight planning and coordination layer.

Use:

- GitHub Issues for backlog items
- sub-issues for breaking down larger work
- GitHub Projects for planning and visibility
- Pull Requests for review and integration
- GitHub Actions for validation
- CODEOWNERS for sensitive areas
- branch protection for merge discipline

## Recommended Project Views

Keep the Project simple. These views match the kind of saved filters commonly useful in open source and GitHub-native production repos:

| View | Layout | Filter | Purpose |
|---|---|---|---|
| Status Board | board | `is:open` | default flow across Todo, In Progress, and Done |
| Triage | table | `is:open label:"ready-for-refinement"` | issues that need scope, questions, or splitting |
| Ready | table | `is:open label:"ready-for-agent" no:assignee` | scoped work available for someone to claim |
| Current Sprint | table | `sprint:@current` | work committed to the active two-week sprint |
| Schedule | roadmap | `is:open` | planned work across sprints, using Sprint start/end |
| Review Queue | table | `is:open is:pr` | PRs waiting on review, checks, or author response |
| Next Release | table | `is:open milestone:"<current release>"` | work targeted to the current quarterly release |

Avoid creating one view per label or every possible planning question. Use ad hoc filters for `area:*`, `high-risk`, `no:assignee`, `sprint:@next`, and `no:sprint` until the repo has enough volume to justify another saved view. A single roadmap-style `Schedule` view is useful because it answers a different operational question: how much work is booked across upcoming sprints.

## Recommended GitHub State

Use GitHub's native fields first. In open source style workflows, labels and milestones usually carry more useful shared meaning than custom Project fields.

| Field | Purpose |
|---|---|
| Status | Project workflow state |
| Assignees | accountable human and collaborators |
| Labels | area, type, priority, readiness, risk when useful |
| Milestone | release target or planning bucket |
| Sprint | two-week iteration when the team plans in sprints |
| Linked pull requests | implementation and review status |
| Reviewers | review ownership |

Avoid turning Projects into a bureaucracy. If GitHub already exposes the information through labels, milestones, assignees, reviewers, linked PRs, checks, or releases, do not duplicate it in a custom Project field. A `Sprint` iteration field is reasonable when the team actually plans in fixed two-week sprints.

## Fast In-Room Assignment

Use GitHub's native `Assignees` field as the ownership source. Make it visible in the `Status Board`, `Triage`, `Ready`, and `Current Sprint` views.

During triage or sprint planning:

1. Open `Ready` to find scoped, unassigned work someone can pick up.
2. When someone volunteers, click the empty `Assignees` cell and select that person.
3. Keep the item in `Todo` until work actually starts; move it to `In Progress` when the assignee creates the branch/worktree and begins implementation.
4. On `Status Board`, use the assignee avatars on cards to scan who is carrying work.

If the volunteer does not appear in the assignee picker, they usually need repository access. Do not create a separate owner field as a workaround.

Recommended labels:

- `area:domain`, `area:api`, `area:data`, `area:jobs`, `area:config`, `area:docs`, `area:ci`
- `priority:p1`, `priority:p2`, `priority:p3`
- `ready-for-refinement`, `ready-for-agent`
- `high-risk` when explicit human approval is required before implementation

## Simple Status Flow

Most teams should start with four statuses:

```text
Todo -> In Progress -> In Review -> Done
```

Definitions:

- **Todo**: scoped and ready for someone to pick up
- **In Progress**: assigned and actively being worked
- **In Review**: PR open, waiting on review or CI
- **Done**: merged, released, and cleaned up

This is enough for teams without formal release trains, sprint planning, or regulatory tracking.

## Expanded Status Flow

Add more statuses only when the team needs to distinguish workflow states that the simple flow conflates. Common reasons: formal refinement gates, release tracking separate from merge readiness, or multiple teams coordinating releases.

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

When to expand:

- **Ready for Refinement** and **Ready for Agent**: useful when the backlog has many vague items and the team needs a visible refinement gate before agent work begins.
- **Ready to Merge**: useful when merging is blocked by external dependencies, release coordination, or batch deploys rather than review.
- **Ready for Release** and **Released**: useful when the team tracks deployments separately from merge, such as with release trains, staged rollouts, or regulated environments.

Expanded definitions:

- **Backlog**: idea exists
- **Ready for Refinement**: needs scope/splitting
- **Ready for Agent**: clear enough for Claude Code
- **In Progress**: issue is assigned and the accountable assignee has branch/worktree
- **In Review**: PR open
- **Ready to Merge**: CI and review complete
- **Ready for Release**: merged or merge-ready with rollout/release details complete
- **Released**: deployed or shipped to the intended environment
- **Done**: release verified, cleanup complete, or closed intentionally

## Agent-Ready Issue Standard

An issue should be marked **Ready for Agent** only when it has:

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

Bad:

```text
Improve import flow.
```

Good:

```text
Goal:
Add row-level validation errors to CSV customer import before records are committed.

Scope:
- src/import/validation/*
- tests/import/validation/*

Out of Scope:
- UI redesign
- API contract changes
- database schema changes
- dependency changes

Acceptance Criteria:
- missing email produces row-level error
- invalid email produces row-level error
- duplicate customer ID produces row-level error
- valid rows still import successfully
- all row errors are returned before commit

Coordination Notes:
Import UI work is happening separately. Do not edit src/import/ui/*.

Suggested Checks:
- npm test -- import
- npm run lint
- npm run build
```

## Sub-Issues

Use parent issues for epics. Use sub-issues for execution.

Parent:

```text
Epic: CSV import improvements
```

Sub-issues:

```text
#1234 Backend row validation
#1235 Import preview UI
#1236 API error contract
#1237 Integration tests
#1238 Documentation
```

Each sub-issue should have one owner, one branch, one worktree, and one PR.

## GitHub CLI Workflow

View issue:

```bash
gh issue view 1234
```

Create worktree:

```bash
cd ~/work/myrepo-main
git fetch origin
git pull --ff-only
git worktree add ../myrepo-feature-1234-csv-import-validation -b feature/1234-csv-import-validation origin/main
```

Create PR:

```bash
gh pr create \
  --title "Add CSV import row validation" \
  --body-file pr.md \
  --base main \
  --head feature/1234-csv-import-validation
```

Check PR:

```bash
gh pr status
gh pr checks
gh pr diff
```
