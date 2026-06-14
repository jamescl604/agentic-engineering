# Quickstart: Agentic Engineering in One Hour

This is the condensed path for experienced engineers who need to start using agents in a production repo today. It covers the essential habits without the full training curriculum.

For the complete training, see [Curriculum](curriculum.md).

## The Five Rules (5 minutes)

1. **Plan before edit.** Ask the agent to inspect and propose a plan. Approve it before implementation begins.
2. **One issue, one branch, one PR.** Keep changes isolated and small enough for human review.
3. **Evidence, not confidence.** Record what commands actually ran and what passed. Never claim "tests pass" unless they did.
4. **Ask before touching shared files.** Dependency manifests, lock files, migrations, auth, CI, API contracts, and global config are high risk.
5. **Review is the bottleneck.** If you're generating PRs faster than humans can review them, slow down.

## Minimum Viable Prompt (5 minutes)

Use this template for any non-trivial change:

```text
Read AGENTS.md / CLAUDE.md first.

Issue: [ISSUE NUMBER OR SUMMARY]
Branch: [BRANCH NAME]

Scope:
- [FILES/MODULES IN SCOPE]

Avoid:
- [FILES/MODULES TO AVOID]

Rules:
- Do not edit files outside scope without asking.
- Do not add dependencies without asking.
- Do not commit.

First:
1. Inspect relevant files.
2. Propose a plan.
3. Wait for approval before editing.
```

## Minimum Viable PR (5 minutes)

Every agent-assisted PR needs at minimum:

```text
## Summary
What changed and why.

## Linked Issue
Closes #1234

## Testing
- [exact command]: [pass/fail]
- Not run: [what and why]

## Risk
[Anything a reviewer should pay extra attention to, or "None"]
```

## Minimum Viable Validation (5 minutes)

After the agent implements a change:

1. Run focused tests for the changed area.
2. Run lint or type checks if the repo has them.
3. Review the diff yourself — agent diffs can include unexpected changes.
4. Check that new code uses existing repo patterns, not invented abstractions.
5. Record what you ran and what passed.

If you skipped something, say so. "Not run: e2e tests, environment not available" is honest. "Looks good" is not validation.

## Agent Code Review: What to Watch For (10 minutes)

Agent-generated code has specific failure modes. When reviewing (your own or someone else's):

- **Scope creep**: Did the agent change files outside the stated scope?
- **Over-engineering**: Did the agent create unnecessary abstractions or helper functions?
- **Hallucinated APIs**: Are all imports and API calls real? Verify unfamiliar ones.
- **Duplicated logic**: Does new code reimplement something the repo already has?
- **Circular tests**: If the agent wrote both code and tests, do the tests verify the requirement or just the implementation?
- **Silent changes**: Did the agent rename, reorder, or restructure unrelated code?

## Common Mistakes (10 minutes)

| Mistake | Fix |
|---|---|
| Letting the agent edit before you review the plan | Use plan-first prompts with an approval gate |
| Agent changes a shared file without warning | List files to avoid in the prompt |
| Claiming "tests pass" without running them | Record exact commands and output |
| PR too large to review | Split the issue before implementation |
| Starting a new workstream before reviewing the last one | Keep a personal WIP limit of 2-3 active branches |
| Agent adds a dependency you didn't ask for | Require explicit approval for dependency changes |
| Trusting the agent's PR summary without checking the diff | Always read the actual diff yourself |

## Recovery Playbook (10 minutes)

**Agent went off scope:**
```text
Stop. Do not edit further.
Show me the current diff. Identify every file changed outside [SCOPE LIST].
Revert changes to those files.
```

**Agent broke something:**
```bash
git diff                    # see what changed
git checkout -- <file>      # revert specific file
git stash                   # stash all changes to start fresh
```

**Merge conflict after rebase:**
```text
Show me the conflicts. For each conflict:
1. Explain what both sides intended.
2. Propose a resolution.
3. Wait for my approval before editing.
```

**Agent claims tests pass but you're not sure:**
Run the tests yourself. Check the actual output. Verify the test assertions match the acceptance criteria, not just the implementation.

## When to Use the Full Process

Use the full playbook workflow (issue template, plan-first, worktree, full validation evidence, rollout notes) when:

- The change touches multiple files or modules
- The change affects shared contracts, APIs, or database schemas
- The change has production or rollout risk
- Multiple people are working in the same area
- You would want a rollback plan if it went wrong

For everything else, use the five rules above and keep moving.

## Next Steps

When you're ready to go deeper:

- [Agentic engineering overview](../docs/agentic-engineering-overview.md) — understand the operating model
- [Pull request hygiene](../docs/pull-request-hygiene.md) — PR templates and agent code review checklist
- [Validation and evidence](../docs/validation-and-evidence.md) — evidence standards
- [Safety and permissions](../docs/safety-and-permissions.md) — what agents should never do
- [Full training curriculum](curriculum.md) — the complete onboarding path
