# 6. Team Workflow & Safety

[← Back to contents](../README.md)

## Account security

### Enable two-factor authentication (2FA)
*GitHub → Settings → Password and authentication.* Prefer a passkey, security key or authenticator app over SMS. Save your recovery codes somewhere safe, like a password manager.

### Authenticate with SSH keys (or the `gh` CLI)

```bash
# Generate a key (use your GitHub email)
ssh-keygen -t ed25519 -C "you@example.com"

# Add it to the agent (macOS: store passphrase in Keychain)
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

# Upload the public key to GitHub
gh ssh-key add ~/.ssh/id_ed25519.pub --title "Work laptop"

# Test
ssh -T git@github.com
```

Alternatively, run `gh auth login` and let the CLI manage HTTPS credentials for you.

- Use a passphrase on your key.
- **Never share your private key** (the file without `.pub`).
- Consider [signing your commits](https://docs.github.com/en/authentication/managing-commit-signature-verification) so they show as "Verified".

## Secrets

**Never commit secrets:** API keys, tokens, passwords, private keys, connection strings, `.env` files.

Prevention:
- Add `.env` and similar files to `.gitignore` *before* creating them.
- Commit a `.env.example` with placeholder values so others know which variables are needed.
- Load secrets from environment variables or a secrets manager, never hard-code them.
- Enable GitHub **secret scanning** and **push protection** (*Settings → Code security*). Push protection blocks pushes that contain known secret formats.
- Consider a pre-commit hook such as [gitleaks](https://github.com/gitleaks/gitleaks).
- Use **GitHub Actions secrets** for CI, never plain text in workflow files.

If you leak one: see [Undoing Mistakes → Committed a secret?](02-undoing-mistakes.md#committed-a-secret). **Rotate first, clean up second, and tell someone.**

## Protected branches

Your `main` branch should be protected (*Settings → Branches / Rulesets*). Common rules:

- Require a pull request before merging
- Require at least 1 approving review
- Require status checks (CI) to pass
- Require branches to be up to date before merging
- Block force pushes and deletions
- Require conversation resolution

If you can push directly to `main` at work, **don't**, and gently mention it to your lead.

## Continuous Integration (CI)

CI runs tests and checks automatically on every push and PR, most commonly with **GitHub Actions**.

A minimal workflow (`.github/workflows/ci.yml`):

```yaml
name: CI
on:
  pull_request:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm test
```

Habits:
- **Never merge with red CI.** Fix it, or understand exactly why it's failing.
- Don't "retry until green" on flaky tests. Report them so they get fixed.
- Run the same checks locally before pushing (or use pre-commit hooks).

## CODEOWNERS

A `.github/CODEOWNERS` file automatically requests reviews from the right people:

```
# Default owners for everything
*                 @your-org/engineering

# Specific areas
/frontend/        @your-org/frontend-team
/infra/           @your-org/platform-team
*.sql             @your-org/data-team
```

## Issues and project tracking

- **Search before filing** to avoid duplicates.
- **Good bug reports include:** steps to reproduce, expected vs. actual behaviour, environment/version, logs or screenshots.
- **Link work to issues:** put `Closes #123` in your PR description.
- Use **labels** (`bug`, `enhancement`, `good first issue`) and **GitHub Projects** boards to track status.
- Issue templates live in `.github/ISSUE_TEMPLATE/`.

```bash
gh issue list --assignee @me
gh issue create
gh issue view 123 --web
```

## Dependency hygiene

- Enable **Dependabot** alerts and security updates (*Settings → Code security*).
- Commit lock files (`package-lock.json`, `poetry.lock`, etc.) so builds are reproducible.
- Review dependency-update PRs like any other code. Check the changelog for breaking changes.

## A healthy team workflow (GitHub Flow)

```
main  ──●──────────●───────────●──▶  (always deployable)
         \        /  \        /
feature   ●──●──●     ●──●──●
         branch  PR    branch  PR
                 +review       +review
                 +CI           +CI
```

1. `main` is always deployable.
2. Branch off `main` for every change.
3. Commit and push early and often.
4. Open a PR, get review, pass CI.
5. Merge, deploy, delete the branch.
