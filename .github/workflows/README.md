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
  soon as they release, each on its own. A cargo git dependency pinned by
  `tag` is followed as it is. A commit that must be written next to its tag
  (the interface a server builds from source) is annotated:

  ```yaml
  # renovate: datasource=github-tags depName=brig-id/app
  ref: <40-character commit SHA> # v1.2.3
  ```

  and both are updated together;
- commits are `chore(deps): ⬆️ update …`, which passes the commit check;
- **Renovate never merges.**
