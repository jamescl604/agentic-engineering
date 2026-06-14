# Safety and Permissions

Production repositories need a clear permission model for agent work. Default to least privilege and require explicit human approval for higher-risk actions.

## Default Agent Access

Agents may usually:

- read repo files
- inspect git status and diffs
- edit files inside the approved scope
- run local tests, lint, type checks, and builds
- create draft documentation or PR summaries

Agents should ask before:

- editing files outside the approved scope
- adding, removing, or upgrading dependencies
- modifying lock files
- editing generated files
- editing auth, security, CI/CD, deployment, database migration, or API contract files
- changing global configuration
- running destructive commands
- using MCP tools with write access
- accessing staging, production, observability, or issue-tracker write tools

Agents must not:

- print secrets into chat, logs, commits, or PR descriptions
- commit credentials, tokens, private keys, or customer data
- bypass tests or CI
- force-push, rewrite shared history, or merge without explicit human instruction
- claim validation succeeded unless the command actually ran
- approve their own production-risk decisions

## Secrets and Sensitive Data

Rules for secrets:

1. Do not paste secrets into agent chat.
2. Do not ask an agent to handle secrets directly.
3. Do not let agents print environment variables wholesale.
4. Use `.env.example` for documentation, not real values.
5. Rotate credentials immediately if they are exposed in chat, logs, commits, or PRs.

Rules for sensitive data:

- Prefer synthetic data for tests and examples.
- Redact customer data before giving it to an agent.
- Avoid copying production logs into prompts unless reviewed and sanitized.
- Treat privacy-sensitive debugging as human-led work.

## Destructive Commands

Agents should ask before commands that delete, overwrite, migrate, publish, deploy, or rewrite history.

Examples:

```text
rm -rf
git reset --hard
git clean -fd
git push --force
database reset/migrate commands
deployment commands
package publish commands
bulk delete scripts
```

For any destructive operation, require:

- stated intent
- target environment
- expected impact
- rollback path
- human approval

## MCP and External Tools

Start MCP integrations as read-only.

Grant write access only when:

- the use case is clear
- tool permissions are scoped
- audit logs exist where possible
- secrets are not exposed casually
- humans approve production-impacting actions

Treat issue tracker writes, deployment tools, database consoles, observability changes, and cloud APIs as production-risk surfaces.

## Dependency Changes

Dependency changes are high-risk because they can affect security, build reproducibility, bundle size, runtime behavior, and merge conflicts.

Before changing dependencies, require:

- reason for the dependency
- alternatives considered
- license/security check when appropriate
- lockfile review
- CI validation
- human approval
