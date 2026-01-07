[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-helm-distribute

A GitHub Action that copies Helm charts between container registries. It simplifies the distribution of Helm charts across different cloud providers, regions, or projects.

## Features

- Copy Helm charts between GCP Artifact Registry and AWS ECR
- Support for GCP Workload Identity Federation and AWS OIDC authentication
- Cross-cloud, cross-region, and cross-project chart distribution
- Supports GCP → GCP, GCP → AWS, AWS → GCP, and AWS → AWS transfers

## Quick Start

### GCP to GCP

```yaml
- name: Distribute Helm chart
  uses: martoc/action-helm-distribute@v1
  with:
    source_registry: gcp
    source_workload_identity_provider: ${{ secrets.SOURCE_WIF_PROVIDER }}
    source_service_account: ${{ secrets.SOURCE_SERVICE_ACCOUNT }}
    source_region: us-central1
    source_gcp_project_id: source-project
    target_registry: gcp
    target_workload_identity_provider: ${{ secrets.TARGET_WIF_PROVIDER }}
    target_service_account: ${{ secrets.TARGET_SERVICE_ACCOUNT }}
    target_region: europe-west1
    target_gcp_project_id: target-project
    chart_name: namespace/chart
    chart_version: 1.0.0
```

### AWS to AWS

```yaml
- name: Distribute Helm chart
  uses: martoc/action-helm-distribute@v1
  with:
    source_registry: aws
    source_aws_role_arn: ${{ secrets.SOURCE_AWS_ROLE_ARN }}
    source_region: us-east-1
    target_registry: aws
    target_aws_role_arn: ${{ secrets.TARGET_AWS_ROLE_ARN }}
    target_region: eu-west-1
    chart_name: namespace/chart
    chart_version: 1.0.0
```

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `source_registry` | Source registry (`gcp` or `aws`) | Yes |
| `source_workload_identity_provider` | Source GCP Workload Identity Provider | No |
| `source_service_account` | Source GCP Service Account | No |
| `source_region` | Source region (GCP or AWS) | No |
| `source_gcp_project_id` | Source GCP Project ID | No |
| `source_aws_role_arn` | Source AWS IAM Role ARN for OIDC authentication | No |
| `target_registry` | Target registry (`gcp` or `aws`) | Yes |
| `target_workload_identity_provider` | Target GCP Workload Identity Provider | No |
| `target_service_account` | Target GCP Service Account | No |
| `target_region` | Target region (GCP or AWS) | No |
| `target_gcp_project_id` | Target GCP Project ID | No |
| `target_aws_role_arn` | Target AWS IAM Role ARN for OIDC authentication | No |
| `chart_name` | Helm chart in format `namespace/chart` | Yes |
| `chart_version` | Helm chart version | Yes |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
