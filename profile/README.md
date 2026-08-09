# brig·id

**brig·id** is a self-hosted identity provider built around passkeys (WebAuthn), OIDC, and
hybrid post-quantum cryptography.

| Repository | Role |
| --- | --- |
| [`.github`](https://github.com/brig-id/.github) | Shared GitHub defaults, community files, and org profile |
| [`roots`](https://github.com/brig-id/roots) | Orchestration — canonical `AGENTS.md`, devcontainer, VS Code workspace |
| [`cli`](https://github.com/brig-id/cli) | `brigid` dev orchestrator CLI |
| [`crypto`](https://github.com/brig-id/crypto) | Cryptographic primitives |
| [`core`](https://github.com/brig-id/core) | Business logic crates |
| [`server-leaf`](https://github.com/brig-id/server-leaf) | Single-server deployment binary |
| [`server-grove`](https://github.com/brig-id/server-grove) | Multi-server orchestration (future) |
| [`server-forest`](https://github.com/brig-id/server-forest) | Global federation layer (future) |
| [`spec`](https://github.com/brig-id/spec) | Technical specs for audit |
| [`app`](https://github.com/brig-id/app) | Qwik UI |
| [`site`](https://github.com/brig-id/site) | Public marketing/landing site |

## Direction

- passwordless authentication with WebAuthn / passkeys
- OIDC issuance with pairwise, non-correlable subject identifiers
- hybrid post-quantum cryptography alongside classical primitives
- privacy-respecting, decentralized-compatible identity (DID:web)
- self-hostable: single-server today (`server-leaf`), multi-server and federated
  deployments planned (`server-grove`, `server-forest`)

Roadmap and in-progress work are tracked on
[GitHub Project 1](https://github.com/orgs/brig-id/projects/1), not in this file.
