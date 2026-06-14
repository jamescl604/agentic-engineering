# Anti-Patterns

These are real failure modes that teams encounter when using AI agents on production repositories. Each anti-pattern includes what happens, why it matters, and how to prevent it.

## The Scope Creep Refactor

**What happens:** You ask the agent to fix a validation bug. The agent fixes the bug, then "improves" the surrounding code by extracting helpers, renaming variables, restructuring imports, and reformatting adjacent functions. The PR goes from 8 changed lines to 180.

**Why it matters:** Reviewers cannot distinguish the bug fix from the refactor. The refactor may conflict with other in-flight work. If the PR introduces a regression, it is hard to tell whether the bug fix or the cleanup caused it.

**Prevention:**
- Set explicit scope and avoid lists in the prompt.
- Include "do not refactor unrelated code" as a standing rule.
- Review the diff before opening the PR. Revert out-of-scope changes.

## The Invented Abstraction

**What happens:** You ask the agent to add a discount expiration check. The agent creates a `DiscountPolicyEngine`, a `PolicyEvaluationContext`, a `PolicyResult` type, and a `PolicyRegistry` — for a single `if (discount.expiresAt < now)` check.

**Why it matters:** Over-engineering slows future development. The next person reading the code has to understand four new abstractions to follow a one-line business rule. Agents tend to build "frameworks" because their training data is full of them.

**Prevention:**
- Prompt with "keep the implementation simple" or "prefer inline logic over new abstractions."
- During review, ask: could this be done with less indirection?
- If a new helper is used once, it should probably be inline code.

## The Hallucinated API

**What happens:** The agent imports a function that looks plausible — `import { validateEmail } from '@company/shared-utils'` — but the function does not exist. The code compiles because the project's type checking is loose, or because the agent also created a type stub. Tests pass because the agent mocked it.

**Why it matters:** The code will fail at runtime in production. This is especially dangerous when the agent writes both the code and the tests, because the tests validate the agent's assumptions rather than actual behavior.

**Prevention:**
- Verify unfamiliar imports against actual installed packages and their exports.
- Be suspicious of agent-created type stubs or mocks that paper over missing dependencies.
- Run integration tests, not just unit tests, for code that crosses module boundaries.

## The Circular Test

**What happens:** You ask the agent to implement a feature and write tests. The agent writes the implementation, then writes tests that verify exactly what the implementation does rather than what the requirement says. If the implementation has a subtle bug, the tests encode that bug as expected behavior.

**Why it matters:** The tests give false confidence. "All tests pass" means the code does what the agent thinks it should do, not what the product actually requires.

**Prevention:**
- Review test assertions against the acceptance criteria, not the implementation.
- Write acceptance criteria before the agent starts. Check whether the tests actually cover them.
- For critical logic, write the test expectations yourself and let the agent implement the assertions.

## The Confidence Overclaim

**What happens:** The PR description says "all tests pass, lint clean, build succeeds." But the agent only ran `npm test -- discount`, not the full suite. Or worse, the agent recommended the commands but never actually ran them.

**Why it matters:** Reviewers trust the validation note. If it's inaccurate, the review is based on false premises. This erodes team trust in agent-assisted work.

**Prevention:**
- Require exact command output, not summaries.
- Distinguish "commands run" from "commands recommended."
- If you didn't run it, say "Not run" explicitly.

## The Merge Collision

**What happens:** Two developers use agents on related issues at the same time. Both agents touch `src/api/orders.ts` to add different fields to the API response. Both PRs pass CI independently. When the second one merges, it silently drops the first one's changes because the merge was clean at the text level but semantically conflicting.

**Why it matters:** Production behavior is silently wrong. Both PRs passed review and CI. Nobody notices until a customer reports a missing field.

**Prevention:**
- Before starting, check who else is working in the same area.
- Record likely files in the issue so other developers can spot overlap.
- Serialize work on shared contracts, APIs, and schemas.
- After merge, re-run CI and spot-check the final state of shared files.

## The Dependency Drive-By

**What happens:** You ask the agent to add email validation. The agent installs `email-validator`, `validator.js`, and `@types/validator` — adding three new dependencies for a check that could be a 10-line regex or a call to an existing utility already in the repo.

**Why it matters:** Each dependency is an ongoing maintenance cost: security updates, license compliance, bundle size, version conflicts, and supply chain risk. Agents default to adding packages because their training data is full of `npm install` solutions.

**Prevention:**
- Require approval before any dependency change.
- Ask: does the repo already have a way to do this?
- Ask: is a dependency justified, or would a small inline solution work?

## The Stale Worktree Pile-Up

**What happens:** A developer creates worktrees for five issues across two days. Three PRs merge. The developer forgets to clean up the merged worktrees and delete the branches. A week later, they have eight worktrees, half of them stale, and accidentally start work in an old one.

**Why it matters:** Stale worktrees cause confusion, wasted work, and merge conflicts against an outdated base. They also make `git worktree list` noisy and error-prone.

**Prevention:**
- Clean up worktrees and branches immediately after merge or abandonment.
- Run `git worktree list` before starting new work to verify your local state.
- Include cleanup in the definition of done for every issue.

## The Runaway Agent Team

**What happens:** A developer launches an agent team with four parallel sessions to "speed up" a feature. The sessions produce 800 lines of changes across 12 files. Some of the sessions make conflicting assumptions. The developer spends more time reconciling the output than they would have spent doing the work with a single agent.

**Why it matters:** Agent teams amplify generation speed, but review and reconciliation scale with the number of parallel changes. For most work, one focused session produces cleaner output faster than four competing sessions.

**Prevention:**
- Default to one session. Use parallel sessions only for genuinely independent investigation or review.
- If using agent teams, give each session a clearly non-overlapping scope.
- Never parallelize work that touches the same files.
