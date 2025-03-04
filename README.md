# Terraform Google Cloud Functions (Gen 2) module

The Terraform module handles the deployment of Cloud Functions (Gen 2) on GCP.

The resources/services/activations/deletions that this module will create/trigger are:

- Deploy Cloud Functions (2nd Gen) with provided source code and trigger
- Provide Cloud Functions Invoker or Developer roles to the users and service accounts

## Assumptions and Prerequisites

This module assumes that below mentioned prerequisites are in place before consuming the module.

* APIs are enabled
* Permissions are available

## Usage

Basic usage of this module is as follows:

```hcl
module "cloud_functions2" {
  source  = "terraform-google-modules/cloud-functions/google"
  version = "~> 0.1.0"

  # Required variables
  function_name  = "<FUNCTION_NAME>"
  project_id     = "<PROJECT_ID>"
  location       = "<LOCATION>"
  runtime        = "<RUNTIME>"
  entrypoint     = "<ENTRYPOINT>"
  storage_source = {
    bucket      = "<BUCKET_NAME>"
    object      = "<ARCHIVE_PATH>"
    generation  = "<GCS_GENERATION>"
  }
}
```

Functional examples are included in the
[examples](./examples/) directory.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| build\_env\_variables | User-provided build-time environment variables | `map(string)` | `null` | no |
| description | Short description of the function | `string` | `null` | no |
| docker\_repository | User managed repository created in Artifact Registry optionally with a customer managed encryption key. | `string` | `null` | no |
| entrypoint | The name of the function (as defined in source code) that will be executed. Defaults to the resource name suffix, if not specified | `string` | n/a | yes |
| event\_trigger | Event triggers for the function | <pre>object({<br>    trigger_region        = string<br>    event_type            = string<br>    service_account_email = string<br>    pubsub_topic          = string<br>    retry_policy          = string<br>    event_filters = set(object({<br>      attribute       = string<br>      attribute_value = string<br>      operator        = string<br>    }))<br>  })</pre> | `null` | no |
| function\_location | The location of this cloud function | `string` | n/a | yes |
| function\_name | A user-defined name of the function | `string` | n/a | yes |
| labels | A set of key/value label pairs associated with this Cloud Function | `map(string)` | `null` | no |
| members | Cloud Function Invoker and Developer roles for Users/SAs. Key names must be developers and/or invokers | `map(list(string))` | `{}` | no |
| project\_id | Project ID to create Cloud Function | `string` | n/a | yes |
| repo\_source | Get the source from this location in a Cloud Source Repository | <pre>object({<br>    project_id   = string<br>    repo_name    = string<br>    branch_name  = string<br>    dir          = string<br>    tag_name     = string<br>    commit_sha   = string<br>    invert_regex = bool<br>  })</pre> | `null` | no |
| runtime | The runtime in which to run the function. | `string` | n/a | yes |
| service\_config | Details of the service | <pre>object({<br>    max_instance_count    = string<br>    min_instance_count    = string<br>    available_memory      = string<br>    timeout_seconds       = string<br>    runtime_env_variables = map(string)<br>    runtime_secret_env_variables = set(object({<br>      key_name   = string<br>      project_id = string<br>      secret     = string<br>      version    = string<br>    }))<br>    secret_volumes = set(object({<br>      mount_path = string<br>      project_id = string<br>      secret     = string<br>      versions = set(object({<br>        version = string<br>        path    = string<br>      }))<br>    }))<br>    vpc_connector                  = string<br>    vpc_connector_egress_settings  = string<br>    ingress_settings               = string<br>    service_account_email          = string<br>    all_traffic_on_latest_revision = bool<br>  })</pre> | <pre>{<br>  "all_traffic_on_latest_revision": true,<br>  "available_memory": "256M",<br>  "ingress_settings": null,<br>  "max_instance_count": "100",<br>  "min_instance_count": null,<br>  "runtime_env_variables": null,<br>  "runtime_secret_env_variables": null,<br>  "secret_volumes": null,<br>  "service_account_email": null,<br>  "timeout_seconds": "60",<br>  "vpc_connector": null,<br>  "vpc_connector_egress_settings": null<br>}</pre> | no |
| storage\_source | Get the source from this location in Google Cloud Storage | <pre>object({<br>    bucket     = string<br>    object     = string<br>    generation = string<br>  })</pre> | `null` | no |
| worker\_pool | Name of the Cloud Build Custom Worker Pool that should be used to build the function. | `string` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| function\_name | Name of the Cloud Function (Gen 2) |
| function\_uri | URI of the Cloud Function (Gen 2) |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Requirements

These sections describe requirements for using this module.

### Software

The following dependencies must be available:

- [Terraform][terraform] v0.13
- [Terraform Provider for GCP][terraform-provider-gcp] plugin v3.0

### Service Account

A service account with the following roles must be used to provision
the resources of this module:

- Storage Admin: `roles/storage.admin`
- Cloud Functions Admin: `roles/cloudfunctions.admin`
- Cloud Run Admin: `roles/run.admin`
- Pub/Sub Admin: `roles/pubsub.admin`
- Artifact Registry Admin: `roles/artifactregistry.admin`
- Cloud Build Editor: `roles/cloudbuild.builds.editor`
- Secret Manager Admin: `roles/secretmanager.admin`

The [Project Factory module][project-factory-module] and the
[IAM module][iam-module] may be used in combination to provision a
service account with the necessary roles applied.

### APIs

A project with the following APIs enabled must be used to host the
resources of this module:

- Google Cloud Storage JSON API: `storage-api.googleapis.com`
- Cloud Functions API: `cloudfunctions.googleapis.com`
- Cloud Run Admin API: `run.googleapis.com`
- Cloud Build API: `cloudbuild.googleapis.com`
- Artifact Registry API: `artifactregistry.googleapis.com`
- Pub/Sub API: `pubsub.googleapis.com`
- Secret Manager API: `secretmanager.googleapis.com`

The [Project Factory module][project-factory-module] can be used to
provision a project with the necessary APIs enabled.

## Contributing

Refer to the [contribution guidelines](./CONTRIBUTING.md) for
information on contributing to this module.

[iam-module]: https://registry.terraform.io/modules/terraform-google-modules/iam/google
[project-factory-module]: https://registry.terraform.io/modules/terraform-google-modules/project-factory/google
[terraform-provider-gcp]: https://www.terraform.io/docs/providers/google/index.html
[terraform]: https://www.terraform.io/downloads.html

## Security Disclosures

Please see our [security disclosure process](./SECURITY.md).
