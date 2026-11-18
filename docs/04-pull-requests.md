# 4. Pull Requests

[← Back to contents](../README.md)

A pull request (PR) asks your team to review and merge your changes. A good PR is easy to review, so it gets reviewed faster.

## Before you open it

- [ ] Branch is up to date with `main` (`git fetch && git rebase origin/main`)
- [ ] Tests pass locally, and you've added tests for new behaviour
- [ ] Linter/formatter has been run
- [ ] You've **read your own diff** on GitHub (you'll catch half the issues yourself)
- [ ] No debug code, commented-out blocks, or stray files
- [ ] No secrets or credentials

## Keep it small

- Aim for **under ~400 changed lines**. Reviews get dramatically worse beyond that.
- One purpose per PR. Don't mix a feature with an unrelated refactor.
- For big features, split into a sequence of PRs (e.g. data model → API → UI), or use feature flags.

## Writing the description

Title: a clear summary, ideally in Conventional Commit form (e.g. `feat(search): add fuzzy matching`).

Body template:

```markdown
## What
Short summary of the change.

## Why
The problem this solves / link to issue or ticket. Closes #123

## How
Key implementation decisions and anything non-obvious.

## Testing
How you verified it: tests added, manual steps, screenshots for UI.

## Notes for reviewers
Where to start, areas you're unsure about, follow-ups planned.
```

> Add this as `.github/pull_request_template.md` in a repo and GitHub will pre-fill it for every PR.

## Draft PRs

Open a **draft PR** early to:
- get early feedback on direction before you've built everything
- let CI run
- show progress

```bash
gh pr create --draft --fill
gh pr ready          # when it's ready for review
```

## Working with the GitHub CLI

```bash
gh pr create --fill                 # create from your commits
gh pr view --web                    # open it in the browser
gh pr checks                        # CI status
gh pr status                        # your PRs and review requests
gh pr checkout 123                  # check out someone else's PR locally
gh pr merge --squash --delete-branch
```

## After you open it

- Assign reviewers (or let CODEOWNERS do it) and link the issue.
- Make sure CI is green. Reviewers often skip red PRs.
- Respond to every comment, even if just "Done" or a 👍.
- Push follow-up commits rather than force-pushing during review, so reviewers can see what changed. Squash at merge time.
- If a PR sits for more than a day, a polite nudge is fine.

## Merge strategies

| Strategy | Result | Good when |
|---|---|---|
| **Squash and merge** | All PR commits → one commit on `main` | Most feature work; keeps `main` clean |
| **Rebase and merge** | Each commit replayed onto `main` | Your commits are already clean and meaningful |
| **Merge commit** | Merge commit preserving branch history | Long-lived branches, release merges |

Use whatever your repo is configured for. Many teams allow only one.
