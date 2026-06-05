# AGENTS.md — brig·id `.github`

This repository contains **organization-level GitHub configuration** only.

## Language

**All content must be in English** — templates, documentation, workflow names,
comments, issue titles. No exceptions.

## Scope

- community health files (`CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`)
- issue and pull request templates (`ISSUE_TEMPLATE/`, `PULL_REQUEST_TEMPLATE.md`)
- reusable workflows (`.github/workflows/`) — called by product repos
- workflow templates (`workflow-templates/`)
- GitHub organization profile (`profile/README.md`)

## Reusable workflows

| Workflow | Triggered by | Purpose |
| --- | --- | --- |
| `ci-rust.yml` | `workflow_call` | fmt check, clippy, cargo nextest |
| `security-audit.yml` | `workflow_call` | cargo audit, cargo deny check |
| `coverage.yml` | `workflow_call` | cargo llvm-cov + upload Codecov |

Product repos call these with a thin caller workflow in their own `.github/workflows/`.

## Rules

- Prefer generic, reusable GitHub conventions over product-specific content.
- Do not implement brig·id application features in this repository.
- Put org profile content in `profile/README.md`.
- Put reusable workflows in `.github/workflows/`.
- Keep issue templates actionable and short.

## Commit conventions

All repositories in this organization follow **Conventional Commits** with **gitmoji**.

### Format

```text
type(scope): <emoji> short description

[optional body]

[optional footer: Co-Authored-By, Fixes #...]
```

**Example:**

```text
feat(api): ✨ add delete passkey endpoint

Implements DELETE /auth/passkeys/{id} authenticated by Bearer token.
Cross-user deletion is blocked via VSID verification.

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

### Types and their emoji

| Type | Emoji | When to use |
| --- | --- | --- |
| `feat` | ✨ | New feature or capability |
| `fix` | 🐛 | Bug fix |
| `docs` | 📝 | Documentation only |
| `chore` | 🔧 | Maintenance, config, tooling |
| `test` | ✅ | Adding or fixing tests |
| `refactor` | ♻️ | Code restructuring, no behaviour change |
| `perf` | ⚡️ | Performance improvement |
| `style` | 🎨 | Formatting, whitespace (no logic change) |
| `ci` | 👷 | CI/CD workflows |
| `security` | 🔒 | Security fix or hardening |
| `build` | 📦 | Build system, dependencies |
| `revert` | ⏪ | Reverts a previous commit |
| `wip` | 🚧 | Work in progress (never merge to main) |

### Scopes

Scopes are **per-repository** and defined in `.vscode/settings.json` and the caller
workflow in `.github/workflows/conventional-commits.yml`.

**Agents must use only the scopes listed in the calling repo's workflow.**
Do not invent a new scope unless a new module or top-level concern is introduced —
in that case, update the workflow file and `.vscode/settings.json` first.

| Repository | Allowed scopes |
| --- | --- |
| `.github` | `governance` `community` `templates` `workflows` `conventional-commits` `security` `support` |
| `core` | `store` `did` `identity` `webauthn` `oidc` `api` `ui` `workspace` `ci` `deps` |
| `server-leaf` | `leaf` `config` `docker` `ci` `deps` |
| `web` | `login` `register` `passkeys` `components` `webauthn` `ci` `deps` |
| `crypto` | `aes` `hkdf` `kem` `dsa` `hybrid` `ci` `deps` |
| `spec` | `protocol` `operations` `audit` `ci` |
| `.dev` | `phases` `memory` `workspace` `devcontainer` `ai` `ci` |

### PR check

Every PR title is automatically validated by the reusable workflow
`.github/workflows/conventional-commits.yml` (called from each product repo).
PRs with a non-compliant title are blocked from merging.

### VSCode extension

Install `vivaxy.vscode-conventional-commits`. Each repo ships a `.vscode/settings.json`
with `"conventionalCommits.gitmoji": true` and the repo's scope list — the extension
will prompt for type, scope, and emoji automatically.

## Related repositories

- `.dev` — central development workspace for the organization
- `crypto`, `core`, `server-leaf`, `spec` — active product repositories
