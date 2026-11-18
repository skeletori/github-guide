# GitHub Guide for Junior Engineers

A practical, opinionated guide to using Git and GitHub well on a professional team. Every section has commands you can copy and paste, plus the reasoning behind them.

> **Golden rule:** when your team's conventions differ from this guide, follow your team. This guide gives you sensible defaults for when nobody has told you otherwise.

## Contents

| # | Topic | What you'll learn |
|---|-------|-------------------|
| 1 | [Git Fundamentals](docs/01-git-fundamentals.md) | Setup, the daily workflow, staging, `.gitignore`, merge vs. rebase |
| 2 | [Undoing Mistakes](docs/02-undoing-mistakes.md) | How to recover from almost anything, safely |
| 3 | [Commit & Branch Conventions](docs/03-commits-and-branches.md) | Writing good commit messages, naming branches, keeping history clean |
| 4 | [Pull Requests](docs/04-pull-requests.md) | Opening PRs that get reviewed quickly and merged smoothly |
| 5 | [Code Review](docs/05-code-review.md) | Reviewing others' code and responding to feedback |
| 6 | [Team Workflow & Safety](docs/06-team-workflow-and-safety.md) | Protected branches, CI, secrets, SSH keys, 2FA, issues |
| ★ | [Cheat Sheet](docs/cheat-sheet.md) | One-page command reference |

## The 10 habits that matter most

1. **Never commit directly to `main`.** Always work on a branch and open a pull request.
2. **Pull before you start, and often while you work.** Small conflicts are easy; big ones hurt.
3. **Make small, focused commits** that each do one thing.
4. **Write commit messages that explain *why*,** not just *what*.
5. **Run `git status` and `git diff --staged` before every commit.** Know exactly what you're committing.
6. **Keep pull requests small.** Aim for under ~400 changed lines.
7. **Never commit secrets** (API keys, passwords, `.env` files). Treat a leaked secret as compromised and rotate it.
8. **Never force-push to a shared branch.** On your own branch, use `--force-with-lease`.
9. **Review your own PR before asking anyone else to.**
10. **Ask for help early.** Being stuck for 30+ minutes is a signal, not a failure.

## How to use this guide

- Read sections 1–4 in your first week.
- Keep the [cheat sheet](docs/cheat-sheet.md) open while you work.
- Come back to [Undoing Mistakes](docs/02-undoing-mistakes.md) when something goes wrong. Don't panic: Git rarely loses committed work.

## Contributing

Spotted a mistake or have a tip? Open an issue or a pull request. This guide follows its own advice.

## License

[MIT](LICENSE)
