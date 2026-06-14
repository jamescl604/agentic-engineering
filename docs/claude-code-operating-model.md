# Claude Code Operating Model

Claude Code is more than a chatbot in your terminal. It has several useful control surfaces:

- `CLAUDE.md` for project memory and repo rules
- skills and slash commands for reusable procedures
- subagents for focused helper tasks
- agent teams for coordinated multi-session work
- hooks for deterministic guardrails
- permissions for tool access control
- MCP for external tools and context
- Git for isolation and review
- tests/CI for validation

Use the features in this order:

1. `CLAUDE.md`
2. plan-first prompts
3. git branches/worktrees
4. local validation
5. reusable skills
6. read-only reviewer subagents
7. hooks for a few high-risk guardrails
8. agent teams only when parallelism is genuinely useful
9. SDK/automation later

## Recommended Repo Files

```text
AGENTS.md
CLAUDE.md
README.md
docs/
  architecture.md
  development.md
  testing.md
  decisions/
.claude/
  skills/
  agents/
.github/
  workflows/
  ISSUE_TEMPLATE/
  pull_request_template.md
tests/
scripts/
.env.example
```

## AGENTS.md and CLAUDE.md Roles

Use `AGENTS.md` for tool-neutral repository context:

- repo goal
- durable engineering principles
- cross-agent working style
- human ownership expectations
- constraints that should apply to any coding agent

Use `CLAUDE.md` for Claude Code-specific operational guidance:

- setup commands
- test/lint/build commands
- Claude-specific workflows
- local tool usage
- repo-specific constraints Claude should always follow

Keep the two files consistent, but avoid duplicating long procedures. Put repeated procedures into skills, templates, or focused docs.

## Practical CLAUDE.md Guidance

Keep `CLAUDE.md` short and durable. It should describe things that are almost always true.

Good content:

- what the repo does
- architecture overview
- setup commands
- test/lint/build commands
- coding standards
- dependency rules
- protected files
- definition of done

Avoid stuffing every possible workflow into `CLAUDE.md`. Put repeated procedures into skills or templates.

## Starter Claude Code Prompt

Use this for a non-trivial task:

```text
Read CLAUDE.md first.

This worktree is for GitHub issue #[ISSUE_NUMBER]:
[PASTE ISSUE BODY OR SUMMARY]

Branch:
[BRANCH NAME]

Scope:
[FILES/MODULES IN SCOPE]

Avoid:
[FILES/MODULES TO AVOID]

Rules:
- Do not edit files outside scope without asking.
- Do not modify shared or high-risk files without asking.
- Do not add dependencies without asking.
- Do not perform broad refactors.
- Do not commit.
- Keep the diff focused.

First:
1. Inspect relevant files.
2. Map acceptance criteria to implementation steps.
3. Identify collision risk.
4. Propose a plan.
5. Wait for approval before editing.
```

## Daily Claude Code Loop

1. Start on a clean branch/worktree.
2. Ask Claude to inspect and plan.
3. Approve or modify the plan.
4. Let Claude implement.
5. Ask Claude to run focused tests first.
6. Review the diff yourself.
7. Ask Claude for a reviewer-mode pass.
8. Run broader validation.
9. Open a PR with a clear summary.
10. Let CI and human review decide merge readiness.

## Claude Code Features: Use Them Pragmatically

### One Session

Use one Claude Code session for most work:

- small features
- bug fixes
- tests
- docs
- local refactors
- repo exploration

Default to one session until you feel the bottleneck is parallel investigation, not coding.

### Subagents

Use subagents for focused helper work. Good first subagents:

- code reviewer
- test reviewer
- security reviewer
- docs writer
- repo explorer

Prefer read-only reviewer subagents. They should inspect and report, not edit.

Use subagents when the helper does not need to coordinate directly with other helpers.

### Agent Teams

Agent teams coordinate multiple Claude Code sessions. They are useful when independent teammates need to communicate, divide work, and synthesize results.

Good uses:

- multi-perspective PR review
- debugging competing hypotheses
- parallel repo investigation
- frontend/backend/test planning for a larger feature

Avoid agent teams for:

- small bug fixes
- sequential work
- same-file edits
- tightly coupled refactors
- work that humans cannot review quickly

Practical rule:

> Use subagents for focused delegation. Use agent teams for coordinated parallel thinking.

For Windows/WSL, prefer in-process mode first when supported by the installed Claude Code version:

```bash
claude --teammate-mode in-process
```

Verify version-sensitive Claude Code commands against current docs before making them team policy.

### Hooks

Use hooks sparingly for the top risks:

- prevent editing `.env` or secrets
- warn on dependency changes
- warn or block protected files
- remind about tests before task completion
- flag auth, migration, CI/CD, or API contract changes

Do not overbuild hook policy early. Add hooks when you see repeated mistakes.

### MCP

Use MCP when it gives Claude useful context or tools:

- docs
- issue tracker
- schema inspection
- observability
- design systems
- local tools

Start read-only. Do not expose production secrets casually. Treat MCP tools with write access as production-risk integrations unless proven otherwise.

### Skills and Slash Commands

Create skills for workflows you repeat:

- review current diff
- write tests
- debug failing test
- prepare PR summary
- validate local build
- update docs

A useful skill is a reusable procedure, not a giant policy document.
