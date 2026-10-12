# Terraform apply

Initialize an S3 backend and apply a Terraform configuration.

The calling job must check out the repository and configure AWS credentials
before using this action.

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: aws-actions/configure-aws-credentials@v6
    with:
      aws-region: eu-central-1
      role-to-assume: <role-arn>
  - uses: mr-chelyshkin/actions/terraform-apply@<ref>
    with:
      working-directory: tf
      terraform-state-bucket: <state-bucket>
      terraform-state-key: <state-key>
      terraform-state-region: eu-central-1
```

## Inputs

| Input | Value |
| -- | -- |
| `terraform-version` | Terraform CLI version. Default: `.terraform-version` in `working-directory`. |
| `working-directory` | Terraform directory relative to the workspace. Default: `.`. |
| `terraform-state-bucket` | Required S3 state bucket. |
| `terraform-state-key` | Required state object key. |
| `terraform-state-region` | Required AWS region containing the state bucket. |
| `write-summary` | Write `init` and `apply` output to `$GITHUB_STEP_SUMMARY`. Default: `true`. |

## Outputs

| Output | Value |
| -- | -- |
| `terraform-outputs` | Terraform outputs as JSON from `terraform output -json`, without sensitive outputs. |

AWS credentials, region settings and project-specific `TF_VAR_*` values are
inherited from the calling job's environment.

The action runs `terraform init`, `terraform apply` and
`terraform output -json`. The resulting JSON is returned in the
`terraform-outputs` action output. When `write-summary` is `true`, `init` and
`apply` output is also written to the step summary.

Implementation: [action.yml](action.yml).
