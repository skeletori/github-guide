# Cheat Sheet

[← Back to contents](../README.md)

## Daily

```bash
git switch main && git pull           # start fresh
git switch -c feat/thing              # new branch
git status                            # what's going on?
git diff                              # unstaged changes
git add -p                            # stage interactively
git diff --staged                     # review staged changes
git commit -m "feat: add thing"       # commit
git push -u origin feat/thing         # first push
git push                              # subsequent pushes
gh pr create --fill                   # open PR
```

## Staying current

```bash
git fetch origin
git rebase origin/main
git push --force-with-lease           # only on YOUR branch
```

## Inspecting

```bash
git log --oneline --graph --all
git show <sha>
git blame <file>
git diff main...HEAD
```

## Undoing

```bash
git restore <file>                    # discard unstaged changes
git restore --staged <file>           # unstage
git commit --amend                    # fix last commit (unpushed)
git reset --soft HEAD~1               # undo last commit, keep changes
git revert <sha>                      # undo a pushed commit safely
git stash / git stash pop             # shelve work
git reflog                            # find "lost" commits
git rebase --abort                    # bail out of a rebase
```

## Branches

```bash
git branch                            # list local
git branch -a                         # list all
git branch -d feat/thing              # delete merged branch
git push origin --delete feat/thing   # delete remote branch
```

## GitHub CLI

```bash
gh auth login
gh repo clone owner/repo
gh pr create --draft --fill
gh pr status
gh pr checks
gh pr checkout 123
gh pr merge --squash --delete-branch
gh issue list --assignee @me
```

## Commit message template

```
type(scope): short imperative summary

Why this change is needed and anything non-obvious.

Closes #123
```

Types: `feat` `fix` `docs` `refactor` `test` `perf` `chore` `ci`

## Never

- ❌ Commit to `main` directly
- ❌ `git push --force` on a shared branch
- ❌ Commit secrets or `.env` files
- ❌ Merge with failing CI
- ❌ `git add .` without checking `git status` first
