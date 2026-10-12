# Invoke Taskfile

Install or restore a cached Task binary, then run a task from the consumer's
Taskfile.

## Inputs

| Input | Value |
| -- | -- |
| `task` | Required task to run, for example `ci/build`. |
| `args` | Arguments for Task, one per line: variables `KEY=value` and flags such as `--dry`. Optional. |
| `directory` | Working directory where Task runs. Default: `.`. |
| `version` | Task release version without the `v` prefix. Default: `3.53.1`. |

Environment variables for the task go in the step's `env:`.

The SHA-256 checksums of the default `version` are pinned in the action. Another
version is checked against `task_checksums.txt` of its release, which detects a
damaged download but not a replaced release.

## Example

The consumer defines `ci/build` in its root Taskfile. Replace `<ref>` with a
published commit or tag.

```yaml
jobs:
  build:
    runs-on: ubuntu-24.04
    permissions:
      contents: read
    steps:
      - uses: actions/checkout@v7
      - uses: mr-chelyshkin/actions/invoke-taskfile@<ref>
        env:
          NODE_ENV: production
        with:
          task: ci/build
          args: |
            TARGET_ARCH=amd64
          version: '3.53.1'
          directory: '.'
```

The action runs `task --yes ci/build TARGET_ARCH=amd64` in the selected
directory. Each line of `args` reaches Task as one argument without shell
evaluation: `${{ }}` values in `args` stay data. `--yes` accepts Task
confirmation prompts.

## Results

| Situation | Result |
| -- | -- |
| Cached binary found for the version and platform | Restore Task under `runner.temp` and add it to `PATH`. |
| No cached binary | Download the matching release, check its SHA-256 and add it to `PATH`. |
| Archive SHA-256 differs from the expected value | Step fails before running the task. |
| Task exits with code `0` | Step succeeds. |
| Task exits with a nonzero code | Step fails. |
| Download or installation fails | Step fails before running the task. |

Implementation: [action.yml](action.yml).
