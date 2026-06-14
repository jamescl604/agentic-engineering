# Agentic Engineering Playbook

Practical best practices for using AI coding agents (Claude Code and similar) on production repositories with multiple human developers.

The goal is not to run the most agents. The goal is to merge more small, safe, reviewed, validated changes.

## Where to Start

| You are… | Start here |
|---|---|
| New to professional software engineering | [Engineering practices](playbook/engineering-practices.md) — a standalone field guide to the habits, practices, and mindset that separate professional engineering from code that happens to work |
| An experienced engineer who wants to start immediately | [One-hour quickstart](training/quickstart.md) |
| New to agentic engineering and want structured training | [Training curriculum](training/README.md) |
| Looking for the full playbook and guide map | [Agentic Engineering Playbook](playbook/) |
| Looking for a specific topic | [Guide map](#guide-map) below |

## Core Principles

1. **Humans own decisions. Agents execute work.**
2. **One backlog item, one accountable assignee.**
3. **Small PRs beat giant agentic PRs.**
4. **Use git isolation aggressively** — one branch, one worktree, one session.
5. **Plan before edit** — inspect the repo and propose a plan first.
6. **CI is the merge gate.**
7. **Review is the bottleneck**, not code generation.
8. **Evidence beats confidence** — record what was run, what passed, what failed.
9. **Production risk needs explicit human ownership.**

## Guide Map

**Practices and workflow**
- [Engineering practices](playbook/engineering-practices.md) — start here if you are new to professional software engineering
- [Agentic engineering overview](playbook/agentic-engineering-overview.md)
- [Claude Code operating model](playbook/claude-code-operating-model.md)
- [Worktrees and parallel work](playbook/worktrees-and-parallel-work.md)

**Collaboration and coordination**
- [Multi-human collaboration](playbook/multi-human-collaboration.md)
- [GitHub coordination](playbook/github-coordination.md)
- [Pull request hygiene](playbook/pull-request-hygiene.md)

**Safety and quality**
- [Safety and permissions](playbook/safety-and-permissions.md)
- [Validation and evidence](playbook/validation-and-evidence.md)
- [Rollout and production risk](playbook/rollout-and-production-risk.md)

**Reference**
- [Anti-patterns](playbook/anti-patterns.md)
- [Throughput and minimal setup](playbook/throughput-and-minimal-setup.md)
- [Quick reference](playbook/quick-reference.md)

**Training**
- [Training overview](training/README.md)
- [Quickstart](training/quickstart.md)
- [Curriculum](training/curriculum.md)
- [Exercises](training/exercises.md)
