# gcp-labels

## Usage 
Example usage of the gcp-labels module below.

Note: Lifecycle block to prevent resources being recreated when the timestamp() function executes on a later date.
```
module "labels" {
  source = "./modules/gcp-labels"

  project_id             = var.project_id
  environment            = var.environment
  creator                = var.creator
  creation_date          = replace(substr(timestamp(), 0, 10), "-", "")
  project_name           = var.project_name
  business_name          = var.business_name
  cost_code              = var.cost_code
  project_sponsor        = var.project_sponsor
  project_technical_lead = var.project_technical_lead
  review_date            = replace(substr(timeadd(timestamp(), "168h"), 0, 10), "-", "")
}

resource "google_storage_bucket" "terraform" {
  name               = local.bucket_name
  bucket_policy_only = true
  force_destroy      = true
  labels             = module.labels.rendered
  location           = var.region

  lifecycle {
    ignore_changes = [labels]
  }
}

```



## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| business\_name | Business name | string | n/a | yes |
| cost\_code | Code Code | string | n/a | yes |
| creation\_date | Creation date | string | `""` | no |
| creator | Creator name | string | n/a | yes |
| environment | Environment name | string | n/a | yes |
| project\_id | Project ID to create resources | string | n/a | yes |
| project\_name | Project name | string | n/a | yes |
| project\_sponsor | Project sponsor name | string | n/a | yes |
| project\_technical\_lead | Project technical lead | string | n/a | yes |
| review\_date | Review date | string | `""` | no |
| state | Resource state | string | `"active"` | no |

## Outputs

| Name | Description |
|------|-------------|
| rendered | Key / value map |

