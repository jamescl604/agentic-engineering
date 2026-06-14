# Practice Exercises

These exercises use the [Acme Orders practice repo](acme-orders-practice-repo.md). Each exercise produces a reviewable artifact.

## Exercise 0: Setup and Workflow Access

Objective: prove the learner can participate in the workflow before implementation starts.

Task:

1. Complete the setup checklist in [Setup and prerequisites](setup-and-prerequisites.md).
2. Authenticate GitHub CLI.
3. View the practice repo, an issue, a Project item, and a PR check.
4. Create a local branch and worktree.
5. Document any missing access or tool substitutes.

Artifact:

- completed setup checklist

Pass criteria:

- learner can access the practice repo and Project
- learner can explain the Project status, labels, milestone, linked PRs, and reviewers used in training
- learner can create a branch and worktree without touching production
- missing access is documented rather than worked around silently

## Exercise 1: Explain the Operating Model

Objective: prove the learner understands the human and agent roles.

Task:

1. Read the playbook overview.
2. Write a short explanation of agentic engineering in your own words.
3. List three decisions the human owns.
4. List three tasks an agent can execute well.

Artifact:

- one-page onboarding note

Pass criteria:

- separates human judgment from agent execution
- mentions review, validation, and scope control
- does not frame agents as replacements for engineering ownership

## Exercise 2: Turn a Vague Request into an Agent-Ready Issue

Objective: learn to scope work before touching code.

Vague request:

```text
Improve discount handling.
```

Task:

1. Convert the request into one or more agent-ready issues.
2. Define goal, scope, out of scope, acceptance criteria, likely files, files to avoid, coordination notes, and suggested checks.
3. Set the expected GitHub state: Project status, assignees, area labels, priority label, readiness label, and milestone.
4. Identify what questions still need a human answer.

Artifact:

- issue draft
- GitHub state update draft

Pass criteria:

- issue is narrow enough for one PR
- validation commands are specific
- risky files are called out
- GitHub state matches the workflow state
- unresolved product decisions are not hidden

## Exercise 3: Plan-First Prompting

Objective: practice directing Claude Code without letting it edit too early.

Task:

1. Pick Issue A from the sample backlog.
2. Write a prompt that asks Claude Code to inspect and plan before editing.
3. Include scope, files to avoid, testing expectations, and approval gate.
4. Review the returned plan and decide whether to approve, narrow, or reject it.

Artifact:

- final prompt
- reviewed plan notes

Pass criteria:

- prompt contains clear scope and avoid lists
- prompt asks for coordination risks when relevant
- learner can explain why the plan is safe or unsafe

## Exercise 4: Small Implementation with Validation Evidence

Objective: complete one low-risk change with honest validation.

Task:

1. Implement Issue A in a dedicated branch or worktree.
2. Keep the diff limited to expected files.
3. Run focused validation.
4. Write a validation note using the evidence standard.

Artifact:

- small diff
- validation note

Pass criteria:

- change stays in scope
- tests are added or updated when appropriate
- validation note lists actual commands and results
- skipped checks are disclosed

## Exercise 5: PR Hygiene Drill

Objective: make the change easy to review.

Task:

1. Draft a PR description for Exercise 4.
2. Include summary, linked issue, scope, out of scope, Claude Code usage, testing, risks, and follow-up.
3. Update the Project item to In Review and confirm labels, milestone, and linked PR are current.
4. Ask Claude to review the current diff without editing.
5. Compare Claude's PR summary draft against the actual diff.

Artifact:

- PR description
- Project update note
- reviewer-mode notes

Pass criteria:

- PR description uses facts, not assumptions
- validation claims match commands run
- Project state matches the actual workflow state
- risks and follow-up are explicit

## Exercise 6: Coordination Triage

Objective: learn to choose safe parallel work.

Scenario:

Three learners want to work on Issues B, C, and E at the same time.

Task:

1. Identify likely changed files.
2. Identify coordination risk.
3. Decide what can run in parallel.
4. Decide what should be serialized or require approval.
5. Write coordination notes for the team.

Artifact:

- coordination review

Pass criteria:

- dependency and lockfile work is flagged
- API-shape risk is flagged
- parallel work recommendation is practical

## Exercise 7: Safety Gate Drill

Objective: practice stopping before high-risk actions.

Task:

1. Review Issue E.
2. Identify why the work is high risk.
3. Write the approval request a learner should send before implementation.
4. Propose a safer investigation-only first step.

Artifact:

- approval request
- investigation plan

Pass criteria:

- learner does not start dependency edits
- approval request includes reason, impact, alternatives, validation, and reviewer needs
- investigation plan is read-only or low-risk

## Exercise 8: Rollout and Rollback Planning

Objective: treat operationally risky changes differently from ordinary code changes.

Task:

1. Pick Issue D.
2. Classify production risk.
3. Draft a rollout checklist.
4. Draft a rollback note.
5. Identify what observability should or should not change.

Artifact:

- rollout checklist
- rollback note

Pass criteria:

- identifies affected systems and owner
- includes monitoring signals
- explains rollback path or says why rollback is hard
- avoids adding noisy observability without purpose

## Exercise 9: Peer Review Simulation

Objective: practice human review of agent-assisted work.

Task:

1. Review another learner's Exercise 4 or 5 artifact.
2. Look for scope creep, missing tests, validation overclaims, risky files, and unclear PR notes.
3. Leave review comments ordered by severity.

Artifact:

- review comments

Pass criteria:

- findings are specific and actionable
- reviewer prioritizes correctness and risk over style preference
- reviewer identifies missing evidence when present

## Exercise 10: High-Throughput Simulation

Objective: practice multiple safe workstreams without outrunning review.

Scenario:

The team has Issues A, B, C, and D ready. Two learners are available. One reviewer is available.

Task:

1. Choose what to start first.
2. Set WIP limits.
3. Create branch/worktree assignments.
4. Run one implementation task.
5. Review one peer artifact before starting another implementation.
6. Decide whether to start a second workstream or wait.

Artifact:

- team execution plan
- one PR-ready artifact
- one peer review
- retrospective note

Pass criteria:

- learner limits WIP based on review capacity
- parallel work avoids shared files when possible
- learner can explain what was intentionally not started

## Exercise 11: Backlog-to-Release Simulation

Objective: work a backlog item end to end beyond PR approval.

Scenario:

Issue C has been refined and implemented. The PR is approved and CI is green. The change affects an API export field and needs release tracking.

Task:

1. Start with the issue in Ready for Agent and move it through each Project status.
2. Record assignees, area labels, priority label, milestone, linked PR, and Project status.
3. Record branch, worktree, agent usage, testing evidence, and release notes in the PR description or delivery log where they naturally belong.
4. Draft merge readiness notes.
5. Draft release readiness notes.
6. Draft release evidence after simulated deployment.
7. Close or update the issue.
8. Create any follow-up issue needed.
9. Clean up the branch and worktree.

Artifact:

- end-to-end delivery log
- Project state transition notes
- release evidence note
- cleanup checklist

Pass criteria:

- learner separates Ready to Merge from Ready for Release
- release evidence is factual
- issue and Project state stay aligned
- follow-up work is explicit
- branch and worktree cleanup is included
