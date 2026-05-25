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
|---|---|---|
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

## Related repositories

- `.dev` — central development workspace for the organization
- `crypto`, `core`, `server-leaf`, `spec` — active product repositories
