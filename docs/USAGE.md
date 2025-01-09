# Usage

## Distribute a Helm Image

This GitHub Action copies a Helm image to multiple registries.

## Inputs

| Name                                | Description                                                                 | Required | Default |
| ----------------------------------- | --------------------------------------------------------------------------- | -------- | ------- |
| `source_registry`                   | Source registry                                                             | true     |         |
| `source_workload_identity_provider` | Workload Identity Provider                                                  | false    |         |
| `source_service_account`            | Service Account                                                             | false    |         |
| `source_region`                     | Region to pull the Helm chart from. Valid values: Google Cloud              | false    |         |
| `source_gcp_project_id`             | Google Cloud Project ID                                                     | false    | ""      |
| `target_registry`                   | Target registry                                                             | true     |         |
| `target_workload_identity_provider` | Workload Identity Provider                                                  | false    |         |
| `target_service_account`            | Service Account                                                             | false    |         |
| `target_region`                     | Region to push the Helm chart to. Valid values: Google Cloud or AWS regions | false    |         |
| `target_gcp_project_id`             | Google Cloud Project ID                                                     | false    |         |
| `helm_chart`                        | Helm chart in the format `namespace/chart`                                  | true     |         |
| `helm_chart_version`                | Helm chart version                                                          | true     |         |

## Usage

```yaml
name: Distribute Helm Image

on: [push]

jobs:
    distribute:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout code
              uses: actions/checkout@v2

            - name: Distribute helm image
              uses: martoc/action-helm-distribute@v1
              with:
                  source_registry: "gcp"
                  source_workload_identity_provider: "projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID"
                  source_service_account: "source-service-account@project-id.iam.gserviceaccount.com"
                  source_region: "us-central1"
                  source_gcp_project_id: "source-project-id"
                  target_registry: "gcp"
                  target_workload_identity_provider: "projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID"
                  target_service_account: "target-service-account@project-id.iam.gserviceaccount.com"
                  target_region: "europe-west1"
                  target_gcp_project_id: "target-project-id"
                  helm_chart: "namespace/chart"
                  helm_chart_version: "1.0.0"
```
