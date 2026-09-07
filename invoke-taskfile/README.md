# Invoke Taskfile

Install or restore a cached Task binary, then run a command from the consumer's Taskfile.

## Inputs

| Input         | Value                                                                 |
|---------------|-----------------------------------------------------------------------|
| `command`     | Required Task command and arguments, for example `ci/build`.          |
| `environment` | Environment variables as comma-separated `KEY=value` pairs. Optional. |
| `directory`   | Working directory where Task runs. Default: `.`.                      |
| `version`     | Task release version without the `v` prefix. Default: `3.53.1`.       |

Use nonempty values without commas in `environment`. Variables are written to `GITHUB_ENV` and remain available to subsequent steps in the same job.

## Example

The consumer defines `ci/build` in its root Taskfile. Replace `<ref>` with a published commit or tag.

```yaml
jobs:
  build:
    runs-on: ubuntu-24.04
    permissions:
      contents: read
    steps:
      - uses: actions/checkout@v6
      - uses: mr-chelyshkin/actions/invoke-taskfile@<ref>
        with:
          environment: NODE_ENV=production,CI=true
          command: ci/build
          version: '3.53.1'
          directory: '.'
```

The action runs `task --yes ci/build` in the selected directory. 
`command` is inserted into the Bash script directly; supply trusted workflow configuration. 
`--yes` accepts Task confirmation prompts.

## Results

| Situation                                     | Result                                                       |
|-----------------------------------------------|--------------------------------------------------------------|
| Cached binary found for the requested version | Restore `/usr/local/bin/task`; skip installation.            |
| No cached binary                              | Download the Linux amd64 release and install it with `sudo`. |
| Task command exits with code `0`              | Step succeeds.                                               |
| Task command exits with a nonzero code        | Step fails.                                                  |
| Download or installation fails                | Step fails before running the Task command.                  |

Implementation: [action.yml](action.yml).
