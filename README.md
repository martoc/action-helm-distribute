[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-helm-distribute

A GitHub Action that copies Helm charts between GCP Artifact Registry instances. It simplifies the distribution of Helm charts across different regions or projects.

## Features

- Copy Helm charts between GCP Artifact Registry instances
- Workload Identity Federation support for secure authentication
- Cross-region and cross-project chart distribution

## Quick Start

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

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `source_registry` | Source registry (e.g., `gcp`) | Yes |
| `source_workload_identity_provider` | Source Workload Identity Provider | No |
| `source_service_account` | Source Service Account | No |
| `source_region` | Source region | No |
| `source_gcp_project_id` | Source GCP Project ID | No |
| `target_registry` | Target registry (e.g., `gcp`) | Yes |
| `target_workload_identity_provider` | Target Workload Identity Provider | No |
| `target_service_account` | Target Service Account | No |
| `target_region` | Target region | No |
| `target_gcp_project_id` | Target GCP Project ID | No |
| `chart_name` | Helm chart in format `namespace/chart` | Yes |
| `chart_version` | Helm chart version | Yes |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
