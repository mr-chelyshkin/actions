# Terraform apply

Initialize an S3 backend and apply a Terraform configuration.

The calling job must check out the repository and configure AWS credentials before using this action.

```yaml
steps:
  - uses: actions/checkout@v6
  - uses: aws-actions/configure-aws-credentials@v6
    with:
      aws-region: eu-central-1
      role-to-assume: <role-arn>
  - uses: mr-chelyshkin/actions/terraform-apply@<ref>
    with:
      version: 1.15.9
      workdir: tf
      backend-bucket: <state-bucket>
      backend-key: <state-key>
      backend-region: eu-central-1
```

## Inputs

| Input            | Value                                                                       |
|------------------|-----------------------------------------------------------------------------|
| `version`        | Terraform CLI version. Default: `1.15.9`.                                   |
| `workdir`        | Terraform directory relative to the workspace. Default: `.`.                |
| `backend-bucket` | Required S3 state bucket.                                                   |
| `backend-key`    | Required state object key.                                                  |
| `backend-region` | Required AWS region containing the state bucket.                            |
| `write-summary`  | Write `init` and `apply` output to `$GITHUB_STEP_SUMMARY`. Default: `true`. |

## Outputs

| Output    | Value                                                    |
|-----------|----------------------------------------------------------|
| `outputs` | Terraform outputs as JSON from `terraform output -json`. |

AWS credentials, region settings and project-specific `TF_VAR_*` values are inherited from the calling job's environment.

The action runs `terraform init`, `terraform apply` and `terraform output -json`.
The resulting JSON is returned in the `outputs` action output. 
When`write-summary` is `true`, `init` and `apply` output is written to the job log and to a fenced `Terraform apply` section in the GitHub step summary. 
Terraform failures remain action failures. 
Set `write-summary: 'false'` to keep command output only in the job log.

Implementation: [action.yml](action.yml).
