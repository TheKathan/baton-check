<p align="center"><a href="https://github.com/TheKathan/baton"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/TheKathan/baton/v1/assets/banner-dark.svg"><img alt="baton: hand-offs for AI agents that build software together" src="https://raw.githubusercontent.com/TheKathan/baton/v1/assets/banner-light.svg" width="720"></picture></a></p>

<p align="center">
  <img alt="latest release" src="https://img.shields.io/github/v/release/TheKathan/baton-check">
  <img alt="license" src="https://img.shields.io/github/license/TheKathan/baton-check">
  <a href="https://github.com/TheKathan/baton"><img alt="baton 1.4+" src="https://img.shields.io/badge/baton-%E2%89%A5%201.4-F76707"></a>
  <img alt="runners: Linux, macOS" src="https://img.shields.io/badge/runners-Linux%20%7C%20macOS-555">
</p>

# baton PR check

Reports on the [baton](https://github.com/TheKathan/baton) task boards (`.baton/tasks/*.jsonl`) a pull request adds or changes, and fails while any has open questions or blockers. Make it a required check, and a PR from an agent can't merge until its agents' questions are settled.

```yaml
on: pull_request
jobs:
  baton:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: { fetch-depth: 0 }
      - uses: TheKathan/baton-check@v1
```

The report (one summary per task: open questions, contracts, decisions and hand-offs) appears in the run's job summary. A PR without task boards passes.

| Input | Default | Meaning |
|---|---|---|
| `base` | the PR's base branch | The git ref to compare with. |
| `strict` | `true` | Fail while a task board has open questions or blockers. |
| `install` | `true` | Install baton with `TheKathan/setup-baton`. Set to `false` if an earlier step did. |
| `version` | `v1` | baton version to install. |

| Output | Meaning |
|---|---|
| `report-file` | Path of the Markdown report, empty when no task board changed. |
| `open` | Number of open questions and blockers across the changed boards. |

**If an automated job opens the PR,** it must use a GitHub App token or a personal access token. PRs created with the default `GITHUB_TOKEN` don't trigger other workflows, so this check would never run.

Needs baton 1.4 or newer, on Linux or macOS runners. It reads the PR's files only and needs no write permissions.

## Versions

Releases follow the same trunk-based semver as [baton](https://github.com/TheKathan/baton): every merged PR is released, a `feature/*` branch bumps the minor version and any other branch the patch version. Use `@v1` for the latest 1.x, or pin an exact tag such as `@v1.0.1`.

MIT licensed.
