# 3. Commit & Branch Conventions

[← Back to contents](../README.md)

## What makes a good commit

- **Atomic:** one logical change per commit. A refactor and a bug fix are two commits.
- **Builds and passes tests:** every commit should leave the code working. This makes `git bisect` and reverts painless.
- **Explains why:** the diff shows *what* changed; the message should say *why*.

## Commit message format

A widely used standard is [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<optional scope>): <short summary>

<optional body: what and why, wrapped at ~72 chars>

<optional footer: issue refs, breaking changes>
```

### Common types

| Type | Use for |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding or fixing tests |
| `perf` | Performance improvement |
| `chore` | Tooling, dependencies, housekeeping |
| `ci` | CI/CD configuration |

### Rules for the summary line

- Imperative mood: "add", not "added" or "adds". It should complete the sentence *"If applied, this commit will…"*
- ≤ 50 characters where possible (hard limit ~72)
- No trailing full stop
- Lowercase after the type (unless your team does otherwise)

### Examples

✅ Good:

```
fix(cart): prevent negative quantities on update

Users could submit -1 via the API and get a credit applied.
Validate quantity >= 1 server-side and return 422 otherwise.

Fixes #482
```

```
refactor(api): extract pagination into shared helper
```

❌ Avoid:

```
fixed stuff
WIP
changes
asdfgh
Updated file.js
```

### Linking issues

Words like `Fixes #123`, `Closes #123` or `Resolves #123` in a commit or PR description automatically close the issue when the change merges into the default branch.

## Branch naming

Use a short, lowercase, hyphenated name with a type prefix:

```
<type>/<short-description>
<type>/<ticket-id>-<short-description>
```

Examples:

```
feat/user-profile-page
fix/PROJ-142-login-redirect-loop
docs/update-setup-instructions
chore/bump-eslint-v9
```

- Keep branches short-lived: hours to a few days, not weeks.
- One branch = one task = one PR.
- Delete branches after merging. GitHub can do this automatically (*Settings → General → Automatically delete head branches*).

## Cleaning up before you open a PR

Your local history can be messy while you work ("wip", "fix typo"). Tidy it before asking for review using interactive rebase:

```bash
git rebase -i origin/main
```

In the editor:
- `pick` keeps a commit
- `reword` changes its message
- `squash` / `fixup` folds it into the previous commit
- `drop` removes it

A handy shortcut for small fixes to an earlier commit:

```bash
git commit --fixup <commit-sha>
git rebase -i --autosquash origin/main
```

> If your team uses **Squash and merge**, the PR title becomes the final commit message, so make the PR title a good Conventional Commit.
