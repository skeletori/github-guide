# 1. Git Fundamentals

[← Back to contents](../README.md)

## One-time setup

```bash
# Who you are (shows up on every commit)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Name new repos' default branch "main"
git config --global init.defaultBranch main

# Make `git pull` rebase your local commits instead of creating merge commits
git config --global pull.rebase true

# Automatically clean up references to deleted remote branches
git config --global fetch.prune true

# Use your preferred editor for commit messages (example: VS Code)
git config --global core.editor "code --wait"
```

> Tip: if your work and personal email differ, use the email your employer's GitHub account is tied to. You can set it per repo with `git config user.email` (no `--global`).

## The mental model

Git has three places your changes live:

```
Working directory  --git add-->  Staging area  --git commit-->  Local repo  --git push-->  Remote (GitHub)
```

- **Working directory**: the files you're editing.
- **Staging area (index)**: what will go into your next commit.
- **Local repository**: your commit history on your machine.
- **Remote**: the shared copy on GitHub, usually called `origin`.

## The daily workflow

```bash
# 1. Start from an up-to-date main
git switch main
git pull

# 2. Create a branch for your task
git switch -c feat/add-login-rate-limit

# 3. Work, then check what changed
git status
git diff

# 4. Stage deliberately
git add path/to/file.py        # specific files
git add -p                     # review and stage hunk by hunk (recommended)

# 5. Double-check what you're about to commit
git diff --staged

# 6. Commit
git commit -m "feat(auth): rate-limit login attempts per IP"

# 7. Push your branch (first time sets the upstream)
git push -u origin feat/add-login-rate-limit

# 8. Open a pull request (or use the GitHub web UI)
gh pr create --fill
```

### Why `git add -p`?
`git add .` stages everything, including debug prints, stray files and secrets. `git add -p` shows each change and asks if you want it. It's the single best habit for clean commits.

## Keeping your branch up to date

While you work, `main` moves forward. Bring those changes in regularly:

```bash
git fetch origin
git rebase origin/main
# fix any conflicts, then:
git add <resolved-files>
git rebase --continue
# you've rewritten your branch's history, so:
git push --force-with-lease
```

## Merge vs. rebase

| | `git merge` | `git rebase` |
|---|---|---|
| What it does | Creates a merge commit joining two histories | Replays your commits on top of another branch |
| History | Preserves exactly what happened, but noisier | Linear and clean |
| Safe on shared branches? | Yes | **No.** Only rebase branches nobody else is using |

**Sensible default:** rebase your own feature branch onto `main` to stay current. Let GitHub do the final merge (often "Squash and merge"). Follow your team's convention if it differs.

## Resolving merge conflicts

When Git can't combine changes automatically, it marks the file:

```
<<<<<<< HEAD
the version on the branch you're rebasing onto / merging into
=======
your version
>>>>>>> your-commit
```

1. Open each conflicted file (`git status` lists them).
2. Decide what the correct final code is. Often it's a combination of both sides.
3. Delete the conflict markers.
4. Run the tests.
5. `git add <file>` then `git rebase --continue` (or `git commit` if merging).

Not sure? `git rebase --abort` or `git merge --abort` puts everything back as it was.

## `.gitignore`

A `.gitignore` file tells Git which files never to track. Add one at the start of every project.

```gitignore
# Dependencies
node_modules/
.venv/

# Build output
dist/
build/

# Environment & secrets
.env
.env.*
!.env.example

# OS / editor files
.DS_Store
.idea/
.vscode/*
!.vscode/extensions.json

# Logs
*.log
```

- Start from a template: [github/gitignore](https://github.com/github/gitignore).
- For OS/editor junk that's specific to *you*, use a global ignore file instead: `git config --global core.excludesFile ~/.gitignore_global`.
- `.gitignore` doesn't affect files that are already tracked. To stop tracking one: `git rm --cached <file>`.

## Useful commands for understanding history

```bash
git log --oneline --graph --decorate --all   # visual history
git log -p path/to/file                      # every change to a file
git blame path/to/file                       # who last changed each line (and in which commit)
git show <commit>                            # what a commit changed
git diff main...HEAD                         # what your branch changes compared to main
```

> Use `git blame` to find *context* (the commit and its PR), not to find someone to blame.
