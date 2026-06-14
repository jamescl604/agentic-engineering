# Agentic Engineering Playbook

This repository captures practical best practices for using Claude Code and AI agents on production repositories with multiple human developers.

The goal is not to run the most agents. The goal is to merge more small, safe, reviewed, validated changes.

Use this playbook as a lightweight operating model for:

- solo Claude Code work
- multiple Claude Code workstreams using git worktrees
- multiple human developers using Claude Code in the same repo
- GitHub Issues, Projects, and PRs as the coordination layer
- production-grade guardrails for safety, validation, rollout, and review

## How to Use This Playbook

Start with the overview, then jump to the workflow you need. The topic files are intentionally focused so humans and agents can find the right guidance quickly.

New to professional software engineering? Start with [Engineering practices](engineering-practices.md) — a standalone field guide covering code design, testing, debugging, security, observability, and the working habits that make an engineer effective. Everything else in this playbook builds on those fundamentals.

New employees should start with [Agentic engineering training](../training/README.md), which teaches the playbook through hands-on practice in a simple fictitious repo.

## Core Operating Principles

1. **Humans own decisions. Agents execute work.**
   Claude Code can plan, implement, debug, test, and review, but a human owns scope, architecture, risk, review, and merge decisions.

2. **One backlog item has one accountable assignee.**
   Even if agents do most of the implementation, one assigned human is accountable for the branch, PR, review response, and final quality.

3. **Small PRs beat giant agentic PRs.**
   Agent-generated diffs can get big quickly. Keep changes narrow enough that a human can review confidently.

4. **Use git isolation aggressively.**
   One backlog item should usually mean one branch, one worktree, and one Claude Code session.

5. **Plan before edit.**
   For non-trivial work, Claude Code should inspect the repo and propose a plan before making changes.

6. **CI is the merge gate.**
   Local validation is useful, but GitHub Actions or equivalent CI should be the final safety net.

7. **Review is the bottleneck, not code generation.**
   If humans cannot keep up with reviewing and validating changes, reduce WIP.

8. **Evidence beats confidence.**
   Agents should record what was actually run, what passed, what failed, and what was not checked.

9. **Production risk needs explicit ownership.**
   Rollout, rollback, observability, migrations, and incident response stay human-owned.

## Guide Map

- [Engineering practices](engineering-practices.md)
- [Agentic engineering overview](agentic-engineering-overview.md)
- [Claude Code operating model](claude-code-operating-model.md)
- [Worktrees and parallel work](worktrees-and-parallel-work.md)
- [Multi-human collaboration](multi-human-collaboration.md)
- [GitHub coordination](github-coordination.md)
- [Pull request hygiene](pull-request-hygiene.md)
- [Safety and permissions](safety-and-permissions.md)
- [Validation and evidence](validation-and-evidence.md)
- [Rollout and production risk](rollout-and-production-risk.md)
- [Anti-patterns](anti-patterns.md)
- [Throughput and minimal setup](throughput-and-minimal-setup.md)
- [Quick reference](quick-reference.md)
- [Agentic engineering training](../training/README.md)

## Recommended First Pass Setup

If you want the simplest version that still manages the top risks, do this:

1. Add a durable `AGENTS.md` and, when Claude Code is the primary tool, a focused `CLAUDE.md`.
2. Create an agent-ready issue template.
3. Create a Claude-assisted PR template.
4. Use one worktree per issue.
5. Use one Claude session per worktree.
6. Require plan-first for non-trivial changes.
7. Treat shared files and production-risk files as high risk.
8. Require validation evidence in every agent-assisted PR.
9. Protect `main` with review and CI.
10. Keep PRs small enough that humans can review them well.

## Source Notes

This playbook is informed by Claude Code and GitHub capabilities, especially:

- Claude Code `CLAUDE.md`, skills, hooks, subagents, agent teams, MCP, and extension features.
- GitHub Issues, issue types, Projects, custom fields, sub-issues, PRs, Actions, CODEOWNERS, and branch protection.

Tool-specific commands can change. Verify version-sensitive Claude Code, GitHub CLI, and MCP behavior against current documentation before turning examples into team policy.

The process is intentionally lighter than a formal enterprise SDLC. Add more structure only when repeated failures show that the team needs it.
