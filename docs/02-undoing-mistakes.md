# 2. Undoing Mistakes

[← Back to contents](../README.md)

Almost everything in Git is recoverable. Before trying anything drastic, **stop and run `git status`**. It usually tells you what to do next.

## Quick lookup

| I want to… | Command |
|---|---|
| Discard unstaged changes to a file | `git restore <file>` |
| Unstage a file (keep the changes) | `git restore --staged <file>` |
| Fix the last commit's message | `git commit --amend` |
| Add a forgotten file to the last commit | `git add <file> && git commit --amend --no-edit` |
| Undo the last commit, keep the changes | `git reset --soft HEAD~1` |
| Undo a commit that's already pushed/shared | `git revert <commit>` |
| Temporarily set work aside | `git stash` / `git stash pop` |
| Recover "lost" commits | `git reflog` |
| Abort a rebase or merge in progress | `git rebase --abort` / `git merge --abort` |

## Local vs. pushed: the key distinction

- **Not pushed yet?** You can freely rewrite history (`amend`, `reset`, interactive rebase).
- **Pushed to a shared branch (like `main`)?** Don't rewrite history. Use `git revert`, which creates a *new* commit that undoes the old one.
- **Pushed only to your own feature branch?** Rewriting is fine, then `git push --force-with-lease`.

## `reset` flavours

```bash
git reset --soft HEAD~1   # undo commit, changes stay staged
git reset --mixed HEAD~1  # undo commit, changes stay in working dir (default)
git reset --hard HEAD~1   # undo commit AND throw away the changes ⚠️
```

⚠️ `--hard` permanently discards uncommitted changes. Committed work can still be found with `git reflog`, but uncommitted edits can't.

## Committed to `main` by accident?

If you haven't pushed:

```bash
git switch -c my-new-branch   # new branch keeps your commits
git switch main
git reset --hard origin/main  # put local main back to match GitHub
git switch my-new-branch
```

## The safety net: `git reflog`

`reflog` records every place `HEAD` has been, including after resets and rebases.

```bash
git reflog
# a1b2c3d HEAD@{0}: reset: moving to HEAD~3
# e4f5g6h HEAD@{1}: commit: the work I thought I lost
git switch -c rescue e4f5g6h
```

## Committed a secret?

Removing it in a new commit is **not enough**. It's still in history, and bots scan GitHub for keys within seconds.

1. **Rotate/revoke the secret immediately.** Assume it's compromised.
2. Tell your lead or security team. This is routine; hiding it is what causes real damage.
3. Then clean up history if needed (with help), using a tool like [`git filter-repo`](https://github.com/newren/git-filter-repo).

See [Team Workflow & Safety](06-team-workflow-and-safety.md#secrets) for prevention.

## Rules of thumb

- Make a backup branch before anything scary: `git branch backup-before-rebase`.
- Prefer `--force-with-lease` over `--force`. It refuses to overwrite commits you haven't seen.
- If you're unsure, ask. A five-minute question beats an afternoon of recovery.
