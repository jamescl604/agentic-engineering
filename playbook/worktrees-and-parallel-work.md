# Worktrees and Parallel Work

Git worktrees let you check out multiple branches from one repo into separate directories.

This is the cleanest way to run multiple Claude Code workstreams at the same time.

## Recommended Layout

```text
~/work/
  myrepo-main/
  myrepo-feature-1234-import-validation/
  myrepo-feature-1250-settings-page/
  myrepo-bugfix-1277-auth-timeout/
```

Keep `myrepo-main` as the coordination copy. Do real work in separate worktrees.

## Create Worktrees

```bash
cd ~/work/myrepo-main

git fetch origin
git checkout main
git pull --ff-only

git worktree add ../myrepo-feature-1234-import-validation -b feature/1234-import-validation origin/main
git worktree add ../myrepo-feature-1250-settings-page -b feature/1250-settings-page origin/main
git worktree add ../myrepo-bugfix-1277-auth-timeout -b bugfix/1277-auth-timeout origin/main
```

Start one Claude Code session per worktree:

```bash
cd ~/work/myrepo-feature-1234-import-validation
claude
```

On Windows PowerShell, use the same git commands with Windows paths or WSL paths consistently. Avoid mixing native Windows paths and WSL paths inside the same worktree workflow.

## Worktree Rules

1. **One backlog item per worktree.**
2. **One Claude Code session per worktree.**
3. **Do not run multiple Claude sessions against the same directory.**
4. **Keep worktrees short-lived.**
5. **Rebase or merge main regularly.**
6. **Remove worktrees after merge or abandonment.**

Remove a completed worktree:

```bash
cd ~/work/myrepo-main
git worktree remove ../myrepo-feature-1234-import-validation
git branch -d feature/1234-import-validation
```

Force removal only when intentionally discarding work:

```bash
git worktree remove --force ../myrepo-feature-1234-import-validation
git branch -D feature/1234-import-validation
```

## Personal WIP Limits

For one human:

| Work Type | Practical Limit |
|---|---:|
| Implementation worktrees | 2-3 |
| Investigation-only worktrees | 3-5 |
| Broad refactors | 1 |
| PRs awaiting your review | 2-4 |

Do not maximize session count. Maximize reviewed, validated, merged changes.
