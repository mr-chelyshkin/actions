# Get tags

Select repository tags using Git version order and return their commit SHAs.

Check out the current repository with all tags. The runner needs Bash, Git, and `jq`; release filtering also needs `gh`.

```yaml
permissions:
  contents: read

steps:
  - uses: actions/checkout@v6
    with:
      fetch-depth: 0
  - uses: mr-chelyshkin/actions/get-tags@<ref>
    id: tags
    with:
      require-release: 'true'
      pattern: 'v0.1.*'
      sort: desc
      count: '3'
```

## Inputs

| Input             | Value                                                                                |
|-------------------|--------------------------------------------------------------------------------------|
| `require-release` | Require a published GitHub Release. Default: `'false'`.                              |
| `pattern`         | Nonempty tag-name glob, such as `v*` or `v0.1.*`. Default: `'*'`.                    |
| `sort`            | Git version order: `asc` or `desc`. Default: `desc` (higher versions first).         |
| `count`           | Maximum results after filtering. Positive integer, no leading zeros. Default: `'1'`. |

Release filtering excludes drafts and includes published prereleases. Tags are read from the existing checkout without fetching.

## Outputs

`tags` is a JSON array in the selected order:

```json
[
  {"tag": "v0.1.0+10", "commit_sha": "0123456789abcdef0123456789abcdef01234567"},
  {"tag": "v0.1.0+2", "commit_sha": "0123456789abcdef0123456789abcdef01234567"}
]
```

- Lightweight and annotated tags resolve to commit SHAs.
- Tags pointing to the same commit remain separate results.
- No matches return `[]`; fewer than `count` return all matches.
- Invalid inputs, Git/API errors, or tags that cannot resolve to commits fail the action.

Use `fromJSON(steps.tags.outputs.tags)` in workflow expressions to consume the array.

Implementation: [action.yml](action.yml).
