# Agentic Engineering Playbook

Practical best practices for using AI coding agents (Claude Code and similar) on production repositories with multiple human developers.

The goal is not to run the most agents. The goal is to merge more small, safe, reviewed, validated changes.

## Where to Start

| You are… | Start here |
|---|---|
| An experienced engineer who wants to start immediately | [One-hour quickstart](training/quickstart.md) |
| New to agentic engineering and want structured training | [Training curriculum](training/README.md) |
| Looking for the full playbook and guide map | [Agentic Engineering Playbook](Agentic_Engineering_Playbook.md) |
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
- [Engineering practices](docs/engineering-practices.md)
- [Agentic engineering overview](docs/agentic-engineering-overview.md)
- [Claude Code operating model](docs/claude-code-operating-model.md)
- [Worktrees and parallel work](docs/worktrees-and-parallel-work.md)

**Collaboration and coordination**
- [Multi-human collaboration](docs/multi-human-collaboration.md)
- [GitHub coordination](docs/github-coordination.md)
- [Pull request hygiene](docs/pull-request-hygiene.md)

**Safety and quality**
- [Safety and permissions](docs/safety-and-permissions.md)
- [Validation and evidence](docs/validation-and-evidence.md)
- [Rollout and production risk](docs/rollout-and-production-risk.md)

**Reference**
- [Anti-patterns](docs/anti-patterns.md)
- [Throughput and minimal setup](docs/throughput-and-minimal-setup.md)
- [Quick reference](docs/quick-reference.md)

**Training**
- [Training overview](training/README.md)
- [Quickstart](training/quickstart.md)
- [Curriculum](training/curriculum.md)
- [Exercises](training/exercises.md)
