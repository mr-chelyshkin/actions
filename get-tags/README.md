# Get tags

Filter repository tags with a regular expression, select them using Git version order, and return their commit SHAs.

Check out the selected repository with all tags. The runner needs Bash, Git, and `jq`; release filtering also needs `gh`.

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
      pattern: '^v0\.1\.(0|[1-9][0-9]*)(\+[1-9][0-9]*)?$'
      sort: desc
      count: '3'
      ignore-build-metadata: 'true'
```

## Inputs

| Input                   | Value                                                                                                |
|-------------------------|------------------------------------------------------------------------------------------------------|
| `repository`            | Repository matching the checkout. Default: `github.repository`.                                      |
| `require-release`       | Require a published GitHub Release. Default: `'false'`.                                              |
| `pattern`               | Nonempty Bash extended regular expression. Default: `'.*'` (all tags).                               |
| `sort`                  | Git version order: `asc` or `desc`. Default: `desc` (higher versions first).                         |
| `ignore-build-metadata` | Ignore `+...` when grouping versions; keep the first matching tag in sort order. Default: `'false'`. |
| `count`                 | Maximum results after filtering and grouping. Positive integer, no leading zeros. Default: `'1'`.    |

Regex filtering runs before release filtering, version grouping, and the result limit.
Use `^` and `$` to match the entire tag name; for example, `^v[1-9][0-9]*$` selects tags such as `v1` and `v12`.
Invalid regular expressions fail even when the checkout has no tags.

Release filtering excludes drafts and includes published prereleases.
Tags are read from the existing checkout without fetching.

The `pattern` input now accepts a regular expression instead of a Git glob.
When upgrading from the glob-based implementation, replace `*` with `.*`, `v*` with `^v.*$`, and `v0.1.*` with `^v0\.1\..*$` to preserve those glob matches.
Use a stricter expression when only a specific version format should qualify.

With `ignore-build-metadata: 'true'` and `sort: desc`, `v0.1.1`, `v0.1.0+2`, `v0.1.0+1`, and `v0.1.0` become `v0.1.1` and `v0.1.0+2`.

## Outputs

`tags` is a JSON array in the selected order:

```json
[
  {"tag": "v0.1.1", "commit_sha": "0123456789abcdef0123456789abcdef01234567"},
  {"tag": "v0.1.0+2", "commit_sha": "0123456789abcdef0123456789abcdef01234567"}
]
```

- Lightweight and annotated tags resolve to commit SHAs.
- Tags pointing to the same commit remain separate unless grouped by `ignore-build-metadata`.
- No matches return `[]`; fewer than `count` return all matches.
- Invalid inputs, Git/API errors, or tags that cannot resolve to commits fail the action.

Use `fromJSON(steps.tags.outputs.tags)` in workflow expressions to consume the array.

Implementation: [action.yml](action.yml).
