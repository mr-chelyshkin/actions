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

| Action                                                             | Purpose                                           |
|--------------------------------------------------------------------|---------------------------------------------------|
| [`invoke-taskfile`](invoke-taskfile/README.md)                     | Run a Taskfile command with a cached Task binary. |
| [`check-changelog-label`](check-changelog-label/README.md)         | Check that a PR has at least one allowed label.   |
| [`check-job-results`](check-job-results/README.md)                 | Combine job results into one final check.         |
| [`check-tag-branch`](check-tag-branch/README.md)                   | Check a tag commit is contained in a branch.      |
| [`check-tag-format`](check-tag-format/README.md)                   | Check a tag name against a configurable pattern.  |
| [`terraform-apply`](terraform-apply/README.md)                     | Initialize and apply Terraform.                   |
| [`aws-cloudfront-invalidate`](aws-cloudfront-invalidate/README.md) | Invalidate CloudFront and wait for completion.    |

## Workflows

- [`pr-golang`](.github/workflows/pr-golang.yml)
- [`pr-nix`](.github/workflows/pr-nix.yml)
- [`pr-python`](.github/workflows/pr-python.yml)
- [`pr-static-site`](.github/workflows/pr-static-site.yml)
- [`pr-terraform`](.github/workflows/pr-terraform.yml)
- [`tag-aws-s3-static-release`](.github/workflows/tag-aws-s3-static-release.yml)
- [`tag-golang-binary`](.github/workflows/tag-golang-binary.yml)
- [`tag-python-package`](.github/workflows/tag-python-package.yml)

## Usage

Replace `<ref>` with a published tag or commit.

```yaml
- uses: mr-chelyshkin/actions/invoke-taskfile@<ref>
  with:
    command: <task-command>
```

Follow the component links for inputs, examples, and behavior.

## Related repositories

- [Containerized Taskfiles](https://github.com/mr-chelyshkin/tasks)
- [Container images](https://github.com/mr-chelyshkin/images)
