# AWS CloudFront invalidate

Invalidate cached paths in a CloudFront distribution and optionally wait for the invalidation to complete.

The calling job must configure AWS credentials and make the AWS CLI available before using this action.

```yaml
steps:
  - uses: aws-actions/configure-aws-credentials@v6
    with:
      aws-region: eu-central-1
      role-to-assume: <role-arn>
  - uses: mr-chelyshkin/actions/aws-cloudfront-invalidate@<ref>
    with:
      distribution-id: <distribution-id>
      paths: "/*"
      wait: "true"
```

## Inputs

| Input             | Value                                               |
|-------------------|-----------------------------------------------------|
| `distribution-id` | Required CloudFront distribution ID.                |
| `paths`           | Space-separated paths to invalidate. Default: `/*`. |
| `wait`            | Wait for completion. Default: `true`.               |

The action creates an invalidation for the selected paths. When `wait` is `true`, it waits until the invalidation is complete.

Implementation: [action.yml](action.yml).
