# Reusable workflows

Shared GitHub Actions workflows, called from the other brig·id repositories.

A repository calls them at `@main`:

```yaml
uses: brig-id/.github/.github/workflows/<workflow-file>@main
```

`main` of this repository is protected like every other `main` of the
organisation, and a commit pin would cost a pull request in every repository
each time a workflow changes. Everything from outside the organisation is
different: inside these workflows and in the callers, every third-party action
is pinned to a commit SHA, with its version in a trailing comment so that the
update pull requests keep it current (see "Updates" below).

No workflow here takes a registry token: every repository builds from the
public registries alone.

## How the repositories hold together

brig·id is several repositories and no monorepo. Three things keep a change
small and safe:

- **One pull request per repository for a change**, and nothing else: no
  release pull request, no pull request to move a pin.
- **A server follows the others at a commit, not at a release.** In a server's
  `Cargo.toml`, crypto and the core are `branch = "main"`, and `Cargo.lock`
  holds the commit; the interface is `APP_REF: <commit>` in the server's
  `ci.yml`. The pull request that needs a newer commit moves it (`cargo update
  -p <crate>`, or the new `APP_REF`). crypto, the core and the interface have
  no release of their own.
- **A change is tested with the other repositories before it is merged**
  (`integration.yml`, below).

## Integration

`integration.yml` builds a server from the caller's change and the other
repositories, and runs the suites that need a whole server: the Rust tests of
what depends on the caller, the interface's Playwright suite against seedling,
and the demonstration site's (a site signs people in through seedling).

```yaml
jobs:
  integration:
    uses: brig-id/.github/.github/workflows/integration.yml@main
    with:
      heads: true # crypto, core, app: the others at the head of their main
      suites: tests interface demo
```

| Caller | `heads` | `suites` |
| --- | --- | --- |
| crypto, core | `true` | `tests interface demo` |
| app | `true` | `interface demo` |
| server-seedling | `false` | `tests interface demo` |
| server-grove | `false` | `tests` |
| demo | `false` | `demo` |

With `heads: false` the others are taken at the commits the server follows:
the run says what the server ships. A server's own Rust tests run in its own
CI; here they run again only in a stack (below).

**A change that spans repositories is a stack**: one pull request per
repository, **on branches of the same name**. Each run takes the other
repositories at that branch when it has an open pull request there, so the
whole change is tested together before any of it is merged. Open the pull
requests from crypto down to the servers, and merge them in the same order;
once the core's is merged, the server's pull request moves its `Cargo.lock`
to the commit it got on `main` (a server's own CI is red until then, which is
what keeps the order). The first job of a run lists the commit it took of each
repository, and why.

A server's run also fails when its `APP_REF` is not a commit of the
interface's `main`: a commit of a pull request's branch disappears when that
pull request is merged.

`integration-main.yml` runs the same suites on every `main`, once a week.

Its check to require on `main` is `integration / Integration`.

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
  ⬆️ update …`. Such an update ships with the next release of a server and is
  no reason for one;
- a major version, one pull request each, only once its box is ticked on the
  repository's Dependency Dashboard (an issue Renovate keeps);
- a lock file refresh a month, which also brings a server to the head of
  crypto and the core;
- a security fix at once, on its own, as `fix(deps): 🔒️ update …`;
- **the project's own repositories are left alone**: Renovate proposes nothing
  for crypto, the core, the interface or the servers, and does not pin the
  `@main` of these workflows to a commit;
- **Renovate never merges.**

## Releases

Only the servers are released, and only when someone asks: nothing releases
on a merge. The version of a server is its tag; no file of the repository
holds it (`Cargo.toml` says `0.0.0`, and the release build writes the version
in before compiling), and there is no `CHANGELOG.md`: the notes of each
release are on its GitHub Release.

A server has a `Release` workflow, started by hand (`Actions → Release → Run
workflow`, or `brigid release cut seedling` from the workspace):

1. `release-plan.yml` (this repository) refuses a commit of `main` whose runs
   are not all green, then reads what was merged since the last release in
   the server **and in crypto, the core and the interface** between the
   commits the two releases follow. It gives the version and the notes.
   Before 1.0 a release is a patch, and a `!` or `BREAKING CHANGE:` a minor;
   from 1.0 on, a feature is a minor and a break a major. `bump` overrides it.
   **When nothing changed for the people who run the server** (only `docs`,
   `test`, `ci`, `chore`, `build`, `refactor`), there is no release, unless
   `force` is ticked.
2. The server's CI runs on that commit, as a release build: tests, static
   binaries, image.
3. The image is pushed, then the GitHub Release is created with the binaries,
   and **the tag with it, last**: a release that fails on the way leaves no
   tag and uses no number. Run it again.

`dry-run` stops before step 3.

```yaml
jobs:
  plan:
    uses: brig-id/.github/.github/workflows/release-plan.yml@main
    permissions:
      contents: read
      actions: read
    with:
      bump: ${{ inputs.bump }}
      force: ${{ inputs.force }}
```

From the workspace, `brigid release` says for each server what a release would
hold and whether one is due, and what is merged in crypto, the core or the
interface that the server does not follow yet. `brigid release notes
<server>` prints the notes.

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
    uses: brig-id/.github/.github/workflows/dco.yml@main
    permissions:
      contents: read
      pull-requests: read
```

then, in the repository's branch protection or ruleset for `main`, adds the
check `dco / Every commit is signed off by its author` to the required ones.
