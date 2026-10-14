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

MIT licensed.
