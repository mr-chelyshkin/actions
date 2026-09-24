# Check tag format

Check `github.ref_name` against a regular expression. Use this action in a tag-triggered workflow; checkout is not required.

## Default: SemVer with a `v` prefix

```yaml
steps:
  - uses: mr-chelyshkin/actions/check-tag-format@<ref>
```

The default follows [SemVer 2.0.0](https://semver.org/) with a required lowercase `v` prefix:

| Accepted                            | Rejected               |
|-------------------------------------|------------------------|
| `v0.1.0`, `v1.2.3`                  | `1.2.3`, `v1`, `v1.2`  |
| `v1.2.3-rc.1`                       | `v01.2.3`, `v1.2.3-01` |
| `v1.2.3+001`, `v1.2.3-rc.1+build.7` | `v1.2.3-`, `v1.2.3+`   |

Prerelease and build metadata suffixes are optional. Numeric version components and numeric prerelease identifiers cannot have leading zeros; build metadata can.

## Custom format

For module catalog tags `v1`, `v2`, and higher, without leading zeros:

```yaml
steps:
  - uses: mr-chelyshkin/actions/check-tag-format@<ref>
    with:
      pattern: '^v[1-9][0-9]*$'
```

Replace `<ref>` with a published tag or commit that contains this action.

## Inputs

| Input     | Value                                                                         |
|-----------|-------------------------------------------------------------------------------|
| `pattern` | Bash extended regular expression. Defaults to SemVer 2.0.0 with a `v` prefix. |

Use `^` and `$` to match the complete tag name. Patterns use Bash's `=~` operator and POSIX extended syntax, not JavaScript or PCRE syntax. Matching is case-sensitive and uses the C locale.

An empty pattern, an invalid expression, or a tag that does not match fails the step. Branch membership is checked separately by [check-tag-branch](../check-tag-branch/README.md).

Implementation: [action.yml](action.yml).
