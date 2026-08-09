# brig·id — `.github`

Shared GitHub defaults for the **brig·id** organization.

This special repository centralizes the GitHub-facing bootstrap for the organization:

- the public organization profile in [`profile/README.md`](./profile/README.md)
- community health files inherited by future repositories
- issue and pull request templates
- reusable workflows and workflow templates as the organization grows

## What this repository is for

Use this repository for anything that should be shared across the organization on GitHub:

- contribution and support guidance
- security reporting policy
- issue / PR defaults
- reusable Actions workflows
- workflow templates surfaced in the GitHub UI

Do **not** use it for product implementation, runtime code, or repository-specific application logic.

## Layout

```text
.github/
├── .github/workflows/      # reusable workflows for future repos
├── ISSUE_TEMPLATE/         # org-wide issue templates
├── workflow-templates/     # starter workflow files shown by GitHub
├── profile/README.md       # GitHub organization profile
├── AGENTS.md               # repo-specific AI guidance
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── PULL_REQUEST_TEMPLATE.md
├── SECURITY.md
└── SUPPORT.md
```

## Repositories

| Repository | Purpose |
| --- | --- |
| [`.github`](https://github.com/brig-id/.github) | Shared GitHub defaults and organization profile |
| [`roots`](https://github.com/brig-id/roots) | Orchestration — canonical `AGENTS.md`, devcontainer, VS Code workspace |
| [`crypto`](https://github.com/brig-id/crypto) | Cryptographic primitives |
| [`core`](https://github.com/brig-id/core) | Business logic crates |
| [`server-leaf`](https://github.com/brig-id/server-leaf) | Single-server deployment binary |
| [`server-grove`](https://github.com/brig-id/server-grove) | Multi-server orchestration (future) |
| [`server-forest`](https://github.com/brig-id/server-forest) | Global federation layer (future) |
| [`spec`](https://github.com/brig-id/spec) | Technical specs for audit |
| [`app`](https://github.com/brig-id/app) | Qwik UI |

Product repositories opt into the shared defaults from here (reusable workflows,
community health files, issue/PR templates).
