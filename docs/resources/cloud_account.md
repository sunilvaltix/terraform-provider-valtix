# Resource: valtix_cloud_account

Cloud account resource defines the credentials of the cloud provider that can be accessed by the Valtix controller.

## Example Usage

## GCP 

Create a GCP service account for use by the Valtix controller and generate/download the key file before running this block.

```hcl
resource "valtix_cloud_account" gcp1 {
  name                 = "gcpaccount1"
  csp_type             = "GCP"
  gcp_credentials_file = file("valtix_controller_key.json")
}
```

## AWS

Create a cross account IAM role before running this block. Look at the [AWS Setup Guide](/AWS_setup_guide/sg_aws_onboarding) for more details.

```hcl
resource "valtix_cloud_account" aws1 {
  name                     = "awsaccount1"
  csp_type                 = "AWS"
  aws_credentials_type     = "AWS_IAM_ROLE"
  aws_iam_role             = "arn:aws:iam::123456789012:role/valtixcontrollerrole"
  aws_account_number       = "123456789012"
  aws_iam_role_external_id = "shared-external-id"
}
```
If using the [Valtix IAM Terraform Module](https://github.com/valtix-security/terraform-aws-valtix-iam), make sure you add a dependency on this module for the Valtix Cloud Account resource using the depends_on meta-argument.

## Azure

Create an application and secret before running this block. Look at the [Azure Setup Guide](/Azure_setup_guide/sg_azure_overview) for more details.

```hcl
resource "valtix_cloud_account" azure1 {
  name                  = "azure-terraform"
  csp_type              = "AZURE"
  azure_directory_id    = "11111111-2222-4bab-aaec-ae38a24dd990"
  azure_subscription_id = "11111111-2222-41a6-8dfc-05b1fc703aa7"
  azure_application_id  = "11111111-2222-499b-a03f-e47e012e63ae"
  azure_client_secret   = "client-secret-password"
}
```

## Account with Inventory Monitoring
```hcl
resource "valtix_cloud_account" aws1 {
  name                     = "awsaccount1"
  csp_type                 = "AWS"
  aws_credentials_type     = "AWS_IAM_ROLE"
  aws_iam_role             = "arn:aws:iam::123456789012:role/valtixcontrollerrole"
  aws_account_number       = "123456789012"
  aws_iam_role_external_id = "shared-external-id"
  inventory_monitoring {
    regions          = ["us-east-1", "us-east-2"]
    refresh_interval = 10
  }
  inventory_monitoring {
    regions          = ["us-west-1", "us-west-2"]
    refresh_interval = 30
  }
}
```

## Argument Reference

* `name` - (Required) Name of the Cloud Account on the Valtix console. Must contain only alphanumeric, hyphens or underscore characters and not exceed 100 characters
* `csp_type` - (Required)  Defines the Cloud Service Provider. Must be "GCP" or "AWS" or "AZURE"

### GCP Arguments

* `gcp_credentials_file` - (GCP - Required) Service account credentials key file created for the Valtix controller access.
* `inventory_monitoring` - Enable inventory monitoring (can be repeated multiple times), look at [Inventory Monitoring](#inventory-monitoring) for details

### AWS Arguments

* `aws_credentials_type` - (AWS - Required) must be "AWS_IAM_ROLE"
* `aws_iam_role` - (AWS - Required) Cross IAM role ARN that Valtix assumes to manage your cloud account
* `aws_account_number` - (AWS - Required) AWS account number
* `aws_iam_role_external_id` - (AWS - Required) External Id for trust relationship
* `inventory_monitoring` - Enable inventory monitoring (can be repeated multiple times), look at [Inventory Monitoring](#inventory-monitoring) for details

### Azure Arguments

* `azure_directory_id` - (Azure - Required) Azure Active Directory Id (Tenant Id)
* `azure_subscription_id` - (Azure - Required) Azure Subscription Id where the Valtix gateway instances are deployed
* `azure_application_id` - (Azure - Required) Azure Application Id that's used as credentials (along with the secret) to manage Azure account/subscription
* `azure_client_secret` - (Azure - Required) Azure client secret for the above application
* `inventory_monitoring` - Enable inventory monitoring (can be repeated multiple times), look at [Inventory Monitoring](#inventory-monitoring) for details

## Inventory Monitoring

* `regions` - List of regions to enable and monitor inventory
* `refresh_interval` - Interval in minutes where the inventory is refreshed