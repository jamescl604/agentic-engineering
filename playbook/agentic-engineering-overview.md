# Agentic Engineering Overview

Agentic engineering is the practice of directing AI agents through the full engineering loop:

1. understand requirements
2. inspect the codebase
3. propose a plan
4. implement changes
5. run validation
6. debug failures
7. update documentation
8. prepare a PR
9. support review

It is not just "AI writes code." It is a disciplined workflow where the human acts as product owner, architect, reviewer, and release owner while Claude Code performs much of the execution.

## Vibe Coding vs. Agentic Engineering

| Dimension | Vibe Coding | Agentic Engineering |
|---|---|---|
| Primary goal | Quickly build something that works | Reliably ship maintainable changes |
| Human role | Prompt writer | Architect, reviewer, task owner |
| AI role | Code generator | Engineering executor |
| Typical output | Prototype or demo | Tested PR-sized change |
| Validation | Manual inspection | Tests, lint, build, review, CI |
| Context | Often minimal | Repo-aware and test-aware |
| Best use | Exploration | Production-oriented engineering |

## What Makes It Engineering

Agentic work becomes engineering when it has:

- clear requirements
- explicit constraints
- architecture awareness
- version control discipline
- automated validation
- human review
- merge gates
- a repeatable workflow

Without those, it is still mostly vibe coding.

## When Not to Use Agents as the Primary Actor

Keep humans in the lead for work where the main risk is judgment, accountability, or organizational context rather than implementation speed.

Examples:

- ambiguous product or UX decisions
- security incidents
- production data repair
- legal, compliance, privacy, or policy-sensitive changes
- broad architecture changes
- risky migrations without a rollback plan
- release decisions
- changes where reviewers cannot realistically understand the diff

Agents can still help by summarizing context, drafting plans, exploring code, writing tests, or reviewing options. They should not become the accountable decision maker.

## Production Definition of Done

For production repositories, an agent-assisted change is not done until:

- scope matches the issue or explicit human instruction
- the diff is small enough for human review
- high-risk files were approved before editing
- tests, lint, build, or other expected checks were run or explicitly skipped
- validation evidence is recorded
- rollout and rollback risk were considered when relevant
- a human reviewed the final diff
