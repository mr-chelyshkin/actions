# Check tag branch

Check that the commit targeted by a tag is contained in a remote branch.

The calling job must check out the repository with complete history before using this action:

```yaml
steps:
  - uses: actions/checkout@v6
    with:
      fetch-depth: 0
  - uses: mr-chelyshkin/actions/check-tag-branch@<ref>
    with:
      branch: main
```

## Inputs

| Input    | Value                             |
|----------|-----------------------------------|
| `branch` | Branch to check. Default: `main`. |

The action fails when the tag commit is not an ancestor of the selected remote branch.

Implementation: [action.yml](action.yml).
