# Check changelog label

Verify that the current pull request has at least one label. 
Optionally restrict the check to an explicit list of allowed labels.

The action must run in a pull request context:

```yaml
jobs:
  label-check:
    runs-on: ubuntu-24.04
    permissions:
      pull-requests: read
    steps:
      - uses: mr-chelyshkin/actions/check-changelog-label@<ref>
```

## Input

| Input            | Value                                                                          |
|------------------|--------------------------------------------------------------------------------|
| `allowed-labels` | Optional newline-separated allowed labels. Empty means every label is allowed. |

To restrict the accepted labels:

```yaml
- uses: mr-chelyshkin/actions/check-changelog-label@<ref>
  with:
    allowed-labels: |
      feature
      bug
      docs
```

The action fails when the pull request has no labels, or when none of its labels match a nonempty `allowed-labels` list.

Implementation: [action.yml](action.yml).
