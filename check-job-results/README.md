# Check job results

Combine dependency job results into one final check. Every job must succeed unless explicitly ignored or allowed to be skipped.

## Inputs

| Input     | Value                                                                                                                 |
|-----------|-----------------------------------------------------------------------------------------------------------------------|
| `results` | Required JSON object from `${{ toJSON(needs) }}`.                                                                     |
| `ignored` | Space-separated job IDs whose results are ignored, including failures and cancellations. Default: empty.              |
| `skipped` | Space-separated job IDs allowed to return `skipped`. Failures and cancellations still fail the check. Default: empty. |

Use job IDs, not display names. `ignored` takes precedence over `skipped`.

## Example

Add this job to a workflow that defines `build`, `audit` and `docs`. Replace `<ref>` with a published commit or tag.

```yaml
jobs:
  # build, audit and docs are defined elsewhere in this workflow.
  check-job-results:
    if: always()
    needs: [build, audit, docs]
    runs-on: ubuntu-24.04
    steps:
      - uses: mr-chelyshkin/actions/check-job-results@<ref>
        with:
          results: ${{ toJSON(needs) }}
          ignored: audit
          skipped: docs
```

`needs` selects the jobs to check. `if: always()` runs the check even when a dependency fails or is skipped. 

## Results

For the example above:

| `build`   | `audit`     | `docs`      | Check                   |
|-----------|-------------|-------------|-------------------------|
| `success` | `success`   | `success`   | Pass                    |
| `success` | `failure`   | `skipped`   | Pass                    |
| `success` | `cancelled` | `success`   | Pass                    |
| `failure` | `success`   | `skipped`   | Fail: `build: failure`  |
| `success` | `success`   | `cancelled` | Fail: `docs: cancelled` |

Success exits with code `0`:

```text
Job results check passed.
```

Rejected job results exit with code `1` and are listed in one error message:

```text
::error::Jobs not successful: build: failure, docs: cancelled
```

Implementation: [action.yml](action.yml).
