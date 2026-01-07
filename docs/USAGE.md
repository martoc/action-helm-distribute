# Usage

## Distribute a Helm Chart

This GitHub Action copies a Helm chart between container registries. It supports GCP Artifact Registry and AWS ECR.

## Supported Registry Combinations

- GCP → GCP
- GCP → AWS
- AWS → GCP
- AWS → AWS

## Inputs

| Name                                | Description                                                                      | Required | Default |
| ----------------------------------- | -------------------------------------------------------------------------------- | -------- | ------- |
| `source_registry`                   | Source registry (`gcp` or `aws`)                                                 | true     |         |
| `source_workload_identity_provider` | GCP Workload Identity Provider for source                                        | false    |         |
| `source_service_account`            | GCP Service Account for source                                                   | false    |         |
| `source_region`                     | Region to pull the Helm chart from. Valid values: Google Cloud or AWS regions    | false    | ""      |
| `source_gcp_project_id`             | Google Cloud Project ID for source                                               | false    | ""      |
| `source_aws_role_arn`               | AWS IAM Role ARN for OIDC authentication (source)                                | false    | ""      |
| `target_registry`                   | Target registry (`gcp` or `aws`)                                                 | true     |         |
| `target_workload_identity_provider` | GCP Workload Identity Provider for target                                        | false    |         |
| `target_service_account`            | GCP Service Account for target                                                   | false    |         |
| `target_region`                     | Region to push the Helm chart to. Valid values: Google Cloud or AWS regions      | false    | ""      |
| `target_gcp_project_id`             | Google Cloud Project ID for target                                               | false    | ""      |
| `target_aws_role_arn`               | AWS IAM Role ARN for OIDC authentication (target)                                | false    | ""      |
| `chart_name`                        | Helm chart in the format `namespace/chart`                                       | true     |         |
| `chart_version`                     | Helm chart version                                                               | true     |         |

## Examples

### GCP to GCP

```yaml
name: Distribute Helm Chart (GCP to GCP)

on: [push]

jobs:
  distribute:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Distribute Helm chart
        uses: martoc/action-helm-distribute@v1
        with:
          source_registry: gcp
          source_workload_identity_provider: projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID
          source_service_account: source-service-account@project-id.iam.gserviceaccount.com
          source_region: us-central1
          source_gcp_project_id: source-project-id
          target_registry: gcp
          target_workload_identity_provider: projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID
          target_service_account: target-service-account@project-id.iam.gserviceaccount.com
          target_region: europe-west1
          target_gcp_project_id: target-project-id
          chart_name: namespace/chart
          chart_version: 1.0.0
```

### AWS to AWS

```yaml
name: Distribute Helm Chart (AWS to AWS)

on: [push]

jobs:
  distribute:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Distribute Helm chart
        uses: martoc/action-helm-distribute@v1
        with:
          source_registry: aws
          source_aws_role_arn: arn:aws:iam::123456789012:role/source-role
          source_region: us-east-1
          target_registry: aws
          target_aws_role_arn: arn:aws:iam::987654321098:role/target-role
          target_region: eu-west-1
          chart_name: namespace/chart
          chart_version: 1.0.0
```

### GCP to AWS

```yaml
name: Distribute Helm Chart (GCP to AWS)

on: [push]

jobs:
  distribute:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Distribute Helm chart
        uses: martoc/action-helm-distribute@v1
        with:
          source_registry: gcp
          source_workload_identity_provider: projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID
          source_service_account: source-service-account@project-id.iam.gserviceaccount.com
          source_region: us-central1
          source_gcp_project_id: source-project-id
          target_registry: aws
          target_aws_role_arn: arn:aws:iam::123456789012:role/target-role
          target_region: eu-west-1
          chart_name: namespace/chart
          chart_version: 1.0.0
```

### AWS to GCP

```yaml
name: Distribute Helm Chart (AWS to GCP)

on: [push]

jobs:
  distribute:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Distribute Helm chart
        uses: martoc/action-helm-distribute@v1
        with:
          source_registry: aws
          source_aws_role_arn: arn:aws:iam::123456789012:role/source-role
          source_region: us-east-1
          target_registry: gcp
          target_workload_identity_provider: projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID
          target_service_account: target-service-account@project-id.iam.gserviceaccount.com
          target_region: europe-west1
          target_gcp_project_id: target-project-id
          chart_name: namespace/chart
          chart_version: 1.0.0
```

## Authentication

### GCP Authentication

For GCP, the action uses Workload Identity Federation. You need to:

1. Set up a Workload Identity Pool and Provider in your GCP project
2. Grant the GitHub Actions service account access to Artifact Registry
3. Provide the `workload_identity_provider` and `service_account` inputs

### AWS Authentication

For AWS, the action uses OIDC authentication. You need to:

1. Set up an OIDC identity provider in AWS IAM for GitHub Actions
2. Create an IAM role with ECR permissions and trust policy for GitHub Actions
3. Provide the `aws_role_arn` input with the role ARN
