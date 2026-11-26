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
  by digest), proposed once a release is a week old, on Monday morning:
  **one pull request a week per repository** for all of them, `build(deps):
  ⬆️ update …`. release-please leaves `build` out: such an update ships with
  the next release and makes none;
- a major version, one pull request each, only once its box is ticked on the
  repository's Dependency Dashboard (an issue Renovate keeps);
- a lock file refresh a month;
- a security fix at once, on its own, as `fix(deps): 🔒️ update …`, so that it
  is released;
- **the project's own repositories** (`crypto`, `core`, `app`): one pull
  request per repository, on the same Monday, `fix(deps): 🐛 bring in …`
  whatever the update, so the release it leads to is a patch. Its box on the
  Dependency Dashboard ("Awaiting Schedule") opens it at once. A cargo git
  dependency pinned by `tag` is followed as it is. A commit that must be
  written next to its tag (the interface a server builds from source) is
  annotated:

  ```yaml
  # renovate: datasource=github-tags depName=brig-id/app
  ref: <40-character commit SHA> # v1.2.3
  ```

  and both are updated together. A server (seedling, grove), which the
  interface and the demo only test against, comes in as `ci(deps): 👷 test
  against …`: it releases nothing, or each release would start the other one
  again;
- the workflows of this repository are pinned by a commit without a tag, which
  Renovate does not follow: a caller moves its pins by hand;
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
  version and the changelog entry, written from the commit subjects. Before
  1.0, `feat` and `fix` both make a patch, and a `!` or `BREAKING CHANGE:` a
  minor (`bump-patch-for-minor-pre-major`); `build`, `ci`, `chore`, `docs`,
  `test` and `refactor` alone open no release pull request. Merging it is the
  release, and nothing else is: the tag `vX.Y.Z` and the GitHub Release follow
  on their own.
- The workflows named in `workflows` are started on that pull request, so it
  is tested like any other, and again on the tag, where a server builds and
  publishes its binaries and image. They need `workflow_dispatch:` among their
  triggers. The `pull_request` runs GitHub also creates for that pull request
  wait for an approval, run nothing and fail when it expires: they can be
  ignored.
- A server's update to a new release of crypto, core or the interface comes
  from Renovate as `fix(deps)` (see "Updates"), so the server's own release
  pull request follows.

From the workspace, `brigid release` lists the pull requests to merge, in
order, with their checks.

## Sign-off

`dco.yml` checks that every commit of a pull request carries a
`Signed-off-by:` line with its author's name and address (the Developer
Certificate of Origin, explained in `CONTRIBUTING.md`).

- It is **skipped** when the pull request comes from a member of the
  organisation (its `author_association` is `OWNER`, `MEMBER` or
  `COLLABORATOR`, or its author can push to the repository) or from a bot (a
  login ending in `[bot]`, `dependabot`, `renovate`). A skipped job counts as
  passed when the check is required.
- It checks out nothing and runs nothing of the pull request: it reads the
  commits from the API. A pull request of more than 250 commits fails (the API
  lists no more).
- For a member whose membership of the organisation is private, the
  association is `CONTRIBUTOR`, in the event and in what the API tells the
  workflow's token: what lets such a member through is the right to push to
  the repository. A private member with read access only is asked to sign off.

This repository runs it on its own pull requests. **A repository opts in** with
one file, `.github/workflows/dco.yml`, also offered as a workflow template
(`workflow-templates/dco.yml`):

```yaml
name: DCO
on:
  pull_request:
    branches: [main, "dev/*"]
permissions:
  contents: read
jobs:
  dco:
    uses: brig-id/.github/.github/workflows/dco.yml@<40-character commit SHA>
    permissions:
      contents: read
      pull-requests: read
```

then, in the repository's branch protection or ruleset for `main`, adds the
check `dco / Every commit is signed off by its author` to the required ones.
