# Acme Orders Practice Repo

Acme Orders is a fictitious internal service for managing customer orders. It is intentionally small, but it has enough moving parts to practice production-style agentic engineering.

## Product Summary

Acme Orders lets support staff:

- create customer orders
- validate order line items
- apply discount codes
- export order summaries
- view fulfillment status

The repo is deliberately boring. The point is not to learn a framework. The point is to practice scope control, validation, collaboration, and safety.

## Suggested Repo Shape

The companion practice repo lives at `https://github.com/jamescl604/acme-orders-agentic-training`. It is an actual working TypeScript project with runnable tests, not a written simulation.

Exercises that reference commands like `npm test -- order-validation` expect a real repo where those commands work. If your team cannot use the companion repo, build a local skeleton that supports at least `npm test`, `npm run lint`, and `npm run build` before starting the exercises.

```text
acme-orders/
  AGENTS.md
  CLAUDE.md
  README.md
  package.json
  package-lock.json
  src/
    api/
      orders.ts
      discounts.ts
      fulfillment.ts
    domain/
      order.ts
      order-validation.ts
      discount-policy.ts
    data/
      order-repository.ts
      migrations/
    jobs/
      fulfillment-sync.ts
    ui/
      order-list.tsx
      order-detail.tsx
      discount-form.tsx
    config/
      routes.ts
  tests/
    domain/
      order-validation.test.ts
      discount-policy.test.ts
    api/
      orders.test.ts
    ui/
      discount-form.test.tsx
  docs/
    architecture.md
    testing.md
  .github/
    workflows/
      ci.yml
    pull_request_template.md
```

Reset and setup automation for future cohorts is maintained in the companion repo's `scripts/setup-github.ps1`.

## Repo Rules for Training

Use these rules in the practice repo:

- one issue per branch
- one branch per worktree
- no dependency changes without approval
- no lockfile edits without approval
- no migration edits without approval
- no auth, routing, CI, or global config edits without approval
- every PR needs testing evidence
- PRs include risk, rollout, or coordination notes when relevant
- learners do not merge their own PRs
- every backlog item must move through the training Project lifecycle
- release evidence is required before marking work Done

## Training GitHub State

Use GitHub-native state in the Acme Orders training repo:

| GitHub State | Example |
|---|---|
| Status | Ready for Agent |
| Assignees | accountable learner and collaborators |
| Labels | `area:domain`, `priority:p2`, `ready-for-agent` |
| Milestone | `2026 Q3 Release` |
| Sprint | `Sprint 2026-06-01` |
| Linked pull requests | implementation PR |
| Reviewers | required reviewer or CODEOWNER |

The learner should keep issue labels, milestone, linked PRs, and Project status aligned as the item moves from backlog to release and cleanup. Use GitHub's native issue and PR assignees as the ownership source instead of creating a separate owner field.

During cohort triage, use the Project's `Ready` view to find scoped work without an owner. When a learner volunteers, assign them directly in the `Assignees` column. Leave the item in `Todo` until work starts, then move it to `In Progress`.

## High-Risk Areas

Treat these as high risk:

- `package.json`
- `package-lock.json`
- `src/data/migrations/*`
- `src/config/routes.ts`
- `.github/workflows/*`
- `src/api/orders.ts` when changing request or response shape
- `src/jobs/fulfillment-sync.ts` when changing retry or idempotency behavior

## Sample Backlog

### Issue A: Add Quantity Validation

Goal: reject order lines with quantity less than 1.

Likely files:

- `src/domain/order-validation.ts`
- `tests/domain/order-validation.test.ts`

Collision risk: low.

Validation:

- `npm test -- order-validation`
- `npm run lint`

Release notes:

- low-risk domain validation change
- release through normal training deploy
- verify validation behavior after release if a runnable app exists

### Issue B: Add Discount Code Expiration

Goal: expired discount codes should not apply to new orders.

Likely files:

- `src/domain/discount-policy.ts`
- `tests/domain/discount-policy.test.ts`

Files to avoid:

- `src/ui/discount-form.tsx`
- `src/api/discounts.ts`

Collision risk: medium if another learner is changing discount UI.

Validation:

- `npm test -- discount-policy`
- `npm run lint`

### Issue C: Add Order Summary Export Field

Goal: include fulfillment status in CSV exports.

Likely files:

- `src/api/orders.ts`
- `tests/api/orders.test.ts`

Collision risk: medium because API shape changes can affect clients.

Validation:

- `npm test -- orders`
- `npm run build`

Release notes:

- API export shape changes require release tracking
- verify downstream consumers or simulated export checks

### Issue D: Add Fulfillment Retry Logging

Goal: log failed fulfillment sync retries with order ID and retry count.

Likely files:

- `src/jobs/fulfillment-sync.ts`
- related tests if present

Collision risk: high because job retry behavior affects operations.

Validation:

- focused job tests
- lint
- rollout note explaining observability impact

Release notes:

- production-risk style change
- requires monitoring signal and rollback note

### Issue E: Replace Discount Library

Goal: evaluate replacing a dependency used by discount calculation.

Likely files:

- `package.json`
- `package-lock.json`
- `src/domain/discount-policy.ts`

Collision risk: high.

Training purpose: learner should identify that this needs human approval before implementation.
