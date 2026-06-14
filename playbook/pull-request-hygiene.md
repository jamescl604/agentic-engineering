# Pull Request Hygiene

Every Claude-assisted PR should clearly explain what happened.

PR description:

```text
## Summary
What changed.

## Linked Issue
Closes #1234

## Scope
Files/modules intentionally changed.

## Out of Scope
What was intentionally not changed.

## Claude Code Usage
Planning, implementation, tests, debugging, review, docs, or agent team.

## Testing
Commands actually run and results.

## Risk / Rollout
Reviewer attention, operational risk, rollout notes, or "none".

## Risks / Follow-up
Known risks or follow-up work.
```

Use [Validation and evidence](validation-and-evidence.md) as the standard for what counts as validation.

Ask Claude to prepare the PR summary:

```text
Prepare a PR summary.

Include:
- summary
- linked issue
- scope
- out of scope
- testing
- files changed
- risk or rollout notes
- risks
- follow-up work

Use only facts from the current diff and commands actually run.
Do not claim tests passed unless they were run.
```

## CODEOWNERS

Use CODEOWNERS for high-risk areas.

Example:

```text
# Auth/security
/src/auth/ @security-team

# Database migrations
/db/migrations/ @backend-leads

# API contracts
/api/schema/ @api-owners

# CI/CD
/.github/workflows/ @infra-team

# Package/dependency changes
/package.json @tech-leads
/package-lock.json @tech-leads
```

## Reviewing Agent-Generated Code

Tests passing and CI green are necessary but not sufficient. Agent-generated code has distinct failure modes that human reviewers should watch for.

### Common Agent Code Problems

**Over-engineering.** Agents add helper functions, abstractions, and indirection for one-time operations. If a utility is used once, it should probably be inline code.

**Non-idiomatic patterns.** Code that compiles and passes tests but does not follow the repo's established conventions. Watch for unfamiliar patterns when the repo already has a clear style.

**Hallucinated APIs.** Agents sometimes use functions, methods, or imports that look plausible but do not exist in the project's dependencies or standard library version. Verify unfamiliar calls against actual API docs.

**Copy-paste instead of reuse.** Agents duplicate logic instead of using existing repo utilities. Check whether the new code reimplements something that already exists in the codebase.

**Circular test validation.** When the agent writes both the implementation and the tests, the tests may verify the agent's interpretation rather than the actual requirement. Read the test assertions against the acceptance criteria, not just the implementation.

**Missing edge cases.** Agent-written tests tend toward the happy path. Check whether error cases, boundary conditions, empty inputs, and concurrent access are covered when they matter.

**Silent behavior changes.** Agents sometimes rename, reorder, or restructure adjacent code while implementing a feature. Diff carefully for changes outside the stated scope.

**Performance blindness.** Agents rarely consider performance unless prompted. Watch for N+1 queries, unnecessary allocations, missing indexes, or O(n²) patterns in hot paths.

### Agent Code Review Checklist

When reviewing an agent-assisted PR, check:

- [ ] Does the diff match the stated scope? Are there unexpected file changes?
- [ ] Are new abstractions justified, or would simpler inline code work?
- [ ] Does the code follow existing repo patterns and conventions?
- [ ] Are imports and API calls real? Do they match the actual dependency versions?
- [ ] Does new code duplicate something that already exists in the codebase?
- [ ] Do tests verify the requirement, not just the implementation?
- [ ] Are error paths, edge cases, and boundaries tested when relevant?
- [ ] Are there performance concerns in the changed paths?
- [ ] Does the validation evidence match what the diff actually does?

## Branch Protection

Protect `main`:

- require PRs before merging
- require approvals
- require CODEOWNERS review
- require status checks
- require up-to-date branches if appropriate
- block force pushes
- block direct pushes

## CI as Quality Gate

Minimum CI:

- install
- lint
- type check if applicable
- unit tests
- build

Add these when relevant:

- integration tests
- e2e tests
- dependency review
- secret scanning
- code scanning
- migration validation
- API compatibility check
