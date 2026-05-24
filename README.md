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

## Bootstrap status

The organization is intentionally starting with only two foundational repositories:

| Repository | Purpose |
| --- | --- |
| [`.github`](https://github.com/brig-id/.github) | Shared GitHub defaults and organization profile |
| [`.dev`](https://github.com/brig-id/.dev) | Shared workspace, devcontainer, and AI guidance |

Future repositories will opt into the shared defaults from here as they are created.
