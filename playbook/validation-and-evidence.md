# Validation and Evidence

Agent-assisted work should include evidence, not just confidence.

Every PR should make clear what was checked, what was not checked, and what remains risky.

## Evidence Standard

Record:

- exact commands run
- pass/fail result for each command
- relevant output summary
- checks intentionally skipped and why
- manual validation performed
- environment assumptions
- known residual risk

If validation was not run, say so directly.

## Good Validation Note

```text
Validation:
- npm test -- import: passed
- npm run lint: passed
- npm run build: passed
- Manual: imported sample CSV with one invalid email and confirmed row-level error

Not run:
- e2e tests, not available for import flow

Residual risk:
- API behavior is covered by unit tests only; no staging validation yet
```

## Bad Validation Note

```text
Validation:
- Looks good
- Should pass tests
```

## Local vs. CI Validation

Local validation is useful for fast feedback. CI is the merge gate.

Use local validation to catch obvious failures before review:

- focused tests first
- lint or type checks for touched language areas
- build when shared code changes
- targeted manual checks for UI or workflow behavior

Use CI to enforce repository-wide checks:

- install
- lint
- type check
- unit tests
- integration tests
- build
- security or dependency scanning when configured

## Agent Rules for Reporting Validation

Agents should:

- distinguish commands actually run from recommended commands
- include failures instead of hiding them
- avoid saying "tests pass" unless tests ran and passed
- mention when a command could not run because tooling or dependencies were missing
- avoid inventing manual validation that did not happen

## Handling Failures

When validation fails:

1. Capture the failing command.
2. Summarize the relevant error.
3. Decide whether the failure is in scope.
4. Fix in-scope failures.
5. Record unrelated failures without broadening the task unnecessarily.
6. Re-run the narrowest useful check.
