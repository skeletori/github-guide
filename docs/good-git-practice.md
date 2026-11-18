# Good Git Practice Checklist

[← Back to contents](../README.md)

Everything in one place. Each item links to the section that explains it in more detail.

## Before you start work

- [ ] Switch to `main` and pull the latest changes: `git switch main && git pull` ([details](01-git-fundamentals.md#the-daily-workflow))
- [ ] Create a new branch for the task, with a clear name like `feat/short-description` ([details](03-commits-and-branches.md#branch-naming))
- [ ] Check that `.gitignore` covers dependencies, build output and `.env` files ([details](01-git-fundamentals.md#gitignore))

## While you work

- [ ] Commit small, focused changes: one logical change per commit ([details](03-commits-and-branches.md#what-makes-a-good-commit))
- [ ] Keep every commit in a working state, so it builds and tests pass
- [ ] Bring in changes from `main` regularly: `git fetch && git rebase origin/main` ([details](01-git-fundamentals.md#keeping-your-branch-up-to-date))
- [ ] Push your branch often, so your work is backed up on GitHub

## Before every commit

- [ ] Run `git status`, so you know which files have changed
- [ ] Stage on purpose with `git add -p` or by naming files, not a blind `git add .` ([details](01-git-fundamentals.md#why-git-add--p))
- [ ] Review exactly what you're committing: `git diff --staged`
- [ ] Remove debug prints, commented-out code and stray files
- [ ] Check there are no secrets, tokens or `.env` files ([details](06-team-workflow-and-safety.md#secrets))
- [ ] Write a message that explains *why*: `type(scope): imperative summary` ([details](03-commits-and-branches.md#commit-message-format))

## Before you open a pull request

- [ ] Rebase onto the latest `main` and resolve any conflicts ([details](01-git-fundamentals.md#resolving-merge-conflicts))
- [ ] Tidy messy "wip" and "fix typo" commits with `git rebase -i origin/main` ([details](03-commits-and-branches.md#cleaning-up-before-you-open-a-pr))
- [ ] Run tests and linters locally
- [ ] Read through your own diff: `git diff main...HEAD`
- [ ] Follow the full PR checklist ([details](04-pull-requests.md#before-you-open-it))

## History and safety rules

- [ ] Never commit directly to `main`
- [ ] Only rewrite history (`amend`, `reset`, `rebase`) on commits that haven't been pushed or that are on your own branch ([details](02-undoing-mistakes.md#local-vs-pushed-the-key-distinction))
- [ ] To undo a commit on a shared branch, use `git revert`, not `reset`
- [ ] Use `git push --force-with-lease`, never `--force`, and only on your own branch
- [ ] Before anything risky, make a backup branch: `git branch backup-before-rebase`
- [ ] If you think work is lost, check `git reflog` before panicking ([details](02-undoing-mistakes.md#the-safety-net-git-reflog))
- [ ] If you commit a secret, rotate it first, then tell someone, then clean up the history ([details](02-undoing-mistakes.md#committed-a-secret))

## After merging

- [ ] Delete the branch, locally (`git branch -d <branch>`) and on GitHub
- [ ] Switch back to `main` and pull before starting the next task
- [ ] Clean up stale branches now and then: `git fetch --prune` and `git branch --merged`
