# Reusable workflows

Shared GitHub Actions workflows, called from the other brig·id repositories.

Reference them by full commit SHA, never by branch: a caller pinned to `@main`
would run whatever lands on this repository next, with the caller's secrets.

```yaml
uses: brig-id/.github/.github/workflows/<workflow-file>@<40-character commit SHA>
```

The same rule applies inside these workflows: every third-party action is
pinned to a commit SHA, with its version in a trailing comment so that the
update pull requests keep it current (see "Updates" below).

No workflow here takes a registry token: every repository builds from the
public registries alone.

## Updates

`default.json`, at the root of this repository, is the organisation's Renovate
preset. A repository uses it with one file, `renovate.json`:

```json
{ "extends": ["github>brig-id/.github"] }
```

and no longer needs a `dependabot.yml`. What the preset does:

- cargo, npm, GitHub Actions (pinned by digest) and Dockerfile images (pinned
  by digest); one grouped pull request a week for Actions; a lock file refresh
  a month;
- a public release is proposed once it is a week old; a security fix at once;
- **the project's own repositories** (`crypto`, `core`, `app`) are proposed as
  soon as they release, together. A cargo git dependency pinned by
  `tag` is followed as it is. A commit that must be written next to its tag
  (the interface a server builds from source) is annotated:

  ```yaml
  # renovate: datasource=github-tags depName=brig-id/app
  ref: <40-character commit SHA> # v1.2.3
  ```

  and both are updated together;
- commits are `chore(deps): ⬆️ update …`, which passes the commit check, except
  for the project's own releases, all in one pull request `feat(deps): ✨ bring
  in …` (a new minor) or `fix(deps): 🐛 bring in …` (anything else), so that
  release-please sees them. A server (seedling, grove), which the interface and
  the demo only test against, comes in as `ci(deps): 👷 test against …`: it
  releases nothing, or each release would start the other one again;
- **Renovate never merges.**

## Releases

`release.yml` is how a repository releases. A repository's own `release.yml`
calls it on every push to `main`:

```yaml
on:
  push:
    branches: [main]
concurrency:
  group: release
  cancel-in-progress: false
jobs:
  release:
    uses: brig-id/.github/.github/workflows/release.yml@<sha>
    permissions:
      contents: write
      pull-requests: write
      actions: write
    with:
      workflows: ci.yml
```

- release-please keeps **one release pull request** open, with the next
  version (`feat` → minor before 1.0, `fix` → patch) and the changelog entry,
  written from the commit subjects. Merging it is the release: the tag
  `vX.Y.Z` and the GitHub Release follow on their own.
- The workflows named in `workflows` are started on that pull request, so it
  is tested like any other, and again on the tag, where a server builds and
  publishes its binaries and image. They need `workflow_dispatch:` among their
  triggers.
- A server's update to a new release of crypto, core or the interface comes
  from Renovate as `feat(deps)` or `fix(deps)` (see "Updates"), so the
  server's own release pull request follows.

From the workspace, `brigid release` lists the pull requests to merge, in
order, with their checks.
