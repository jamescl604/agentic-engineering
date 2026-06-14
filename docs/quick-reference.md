# Quick Reference

## Start New Work

```bash
cd ~/work/myrepo-main
git fetch origin
git checkout main
git pull --ff-only

git worktree add ../myrepo-feature-1234-short-name -b feature/1234-short-name origin/main
cd ../myrepo-feature-1234-short-name
claude
```

## Resume Work

```bash
git status
git fetch origin
git rebase origin/main
claude
```

Prompt:

```text
The branch has been rebased on latest main.
Run git status and inspect the current diff before continuing.
Do not edit until you confirm the current state and plan next steps.
```

## Before PR

```bash
git status
git diff --stat
git diff
npm test
npm run lint
npm run build
```

Claude prompt:

```text
Review the current diff. Do not edit.

Summarize:
1. changed files
2. why each changed
3. tests run
4. risks
5. collision risk
6. PR summary draft
```

## After Merge

```bash
cd ~/work/myrepo-main
git checkout main
git pull --ff-only
git worktree remove ../myrepo-feature-1234-short-name
git branch -d feature/1234-short-name
```

## Recovery

### Agent Went Off Scope

Prompt:

```text
Stop. Do not edit further.
Show me the current diff. Identify every file changed outside [SCOPE LIST].
Revert changes to those files.
```

Or manually revert specific files:

```bash
git diff --stat                        # see what changed
git checkout -- src/unrelated/file.ts  # revert one file
```

### Undo All Agent Changes

```bash
git stash                  # save changes in case you want them later
# or
git checkout -- .          # discard all uncommitted changes permanently
```

To retrieve stashed changes later:

```bash
git stash list
git stash pop              # restore most recent stash
```

### Resolve Merge Conflicts After Rebase

```bash
git fetch origin
git rebase origin/main
# if conflicts appear:
git status                 # see conflicted files
```

Prompt:

```text
We have merge conflicts after rebasing on main.

For each conflicted file:
1. Explain what both sides intended.
2. Propose a resolution that preserves both changes.
3. Wait for my approval before editing.

Do not auto-resolve. Do not discard either side without asking.
```

After resolving each file:

```bash
git add <resolved-file>
git rebase --continue
```

### Agent Broke Tests

```bash
git diff                   # see what the agent changed
npm test                   # confirm which tests fail
```

Prompt:

```text
These tests are failing: [paste test names or output].

Inspect the failures. For each:
1. Is the failure caused by our changes or a pre-existing issue?
2. If caused by our changes, propose a fix.
3. If pre-existing, note it and move on.

Do not fix unrelated test failures.
```

### Stale Worktree

If a worktree has fallen behind main:

```bash
cd ~/work/myrepo-feature-1234-short-name
git fetch origin
git log --oneline main..HEAD   # see your commits
git rebase origin/main         # bring up to date
```

If the worktree is abandoned and should be removed:

```bash
cd ~/work/myrepo-main
git worktree remove ../myrepo-feature-1234-short-name
git branch -D feature/1234-short-name
```

### Check Local Worktree State

```bash
git worktree list              # see all worktrees
git branch -vv                 # see all branches and tracking
```

Clean up any worktrees for merged or abandoned work before starting new work.
