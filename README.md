# Reusable GitHub Actions

[![License: Apache-2.0](https://img.shields.io/github/license/mr-chelyshkin/actions?label=license)](LICENSE)

<p align="center">
  <img src=".github/assets/readme-header.png"
       alt="github.com/mr-chelyshkin/actions"
       width="800">
</p>

### Reusable GitHub Actions and workflows for projects that use Taskfiles in local development and CI.

- Project-specific commands and configuration stay in consumer repositories.
- Shared workflow orchestration lives here.

## Actions

| Action | Purpose |
| -- | -- |
| [`task-run`](task-run/README.md) | Run a Taskfile task with a cached Task binary. |
| [`changelog-label-check`](changelog-label-check/README.md) | Require an allowed label on the pull request. |
| [`job-results-check`](job-results-check/README.md) | Combine the results of needed jobs into one check. |
| [`tag-branch-check`](tag-branch-check/README.md) | Check that a tag commit is in a remote branch. |
| [`tag-format-check`](tag-format-check/README.md) | Check a tag name against a regular expression. |
| [`tag-list`](tag-list/README.md) | Select version-sorted tags and their commit SHAs. |
| [`terraform-apply`](terraform-apply/README.md) | Apply Terraform with an S3 backend and return its outputs. |
| [`aws-cloudfront-invalidate`](aws-cloudfront-invalidate/README.md) | Invalidate CloudFront paths and wait for completion. |

## Workflows

- [`ci-golang`](.github/workflows/ci-golang.yml)
- [`ci-nix`](.github/workflows/ci-nix.yml)
- [`ci-node`](.github/workflows/ci-node.yml)
- [`ci-python`](.github/workflows/ci-python.yml)
- [`ci-rust`](.github/workflows/ci-rust.yml)
- [`ci-terraform`](.github/workflows/ci-terraform.yml)
- [`cd-aws-s3-static`](.github/workflows/cd-aws-s3-static.yml)

Every `ci-*` workflow accepts an optional `env-vars` input: a JSON object whose
entries become environment variables of every job.

```yaml
with:
  env-vars: |
    {"TF_STATE_KEY": ${{ toJSON(vars.TF_STATE_KEY) }}}
```

## Usage

Replace `<ref>` with a published tag or commit.

```yaml
- uses: mr-chelyshkin/actions/task-run@<ref>
  with:
    task: <task>
```

Follow the component links for inputs, examples, and behavior.

## Related repositories

- [Containerized Taskfiles](https://github.com/mr-chelyshkin/tasks)
- [Container images](https://github.com/mr-chelyshkin/images)
