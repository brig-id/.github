# Reusable workflows

Shared GitHub Actions workflows, called from the other brig·id repositories.

Reference them by full commit SHA, never by branch: a caller pinned to `@main`
would run whatever lands on this repository next, with the caller's secrets.

```yaml
uses: brig-id/.github/.github/workflows/<workflow-file>@<40-character commit SHA>
```

The same rule applies inside these workflows: every third-party action is
pinned to a commit SHA, with its version in a trailing comment so Dependabot
can keep it current.
