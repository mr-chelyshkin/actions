# Reusable GitHub Actions

[![License: Apache-2.0](https://img.shields.io/github/license/mr-chelyshkin/actions?label=license)](LICENSE)

<p align="center">
  <img src=".github/assets/readme-header.png"
       alt="An engraved Jacquard machine executing punched-card instructions"
       width="800">
</p>

Reusable GitHub Actions and workflows for projects that use Taskfiles in local development and CI.

- Project-specific commands and configuration stay in consumer repositories.
- Shared workflow orchestration lives here.

## Actions

| Action                                             | Purpose                                           |
|----------------------------------------------------|---------------------------------------------------|
| [`invoke-taskfile`](invoke-taskfile/README.md)     | Run a Taskfile command with a cached Task binary. |
| [`check-job-results`](check-job-results/README.md) | Combine job results into one final check.         |

## Workflows

- [`static-site-ci`](.github/workflows/static-site-ci.yml)
- [`terraform-ci`](.github/workflows/terraform-ci.yml)
- [`require-changelog-label`](.github/workflows/require-changelog-label.yml)
- [`aws-s3-site-release`](.github/workflows/aws-s3-site-release.yml)

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
