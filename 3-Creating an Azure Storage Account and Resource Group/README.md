# Day 3 – Creating an Azure Storage Account and Resource Group with Terraform

In this lesson, you'll learn how to create an Azure Resource Group and Storage Account using Terraform. We'll cover authentication, provider configuration, implicit dependencies, and essential Terraform workflow commands.

---

![Azure Storage Account](https://docs.microsoft.com/en-us/azure/storage/common/media/storage-account-overview/Image.png)

## Objectives

By the end of this tutorial, you will be able to:

- Configure Terraform to use the AzureRM provider
- Authenticate using Azure CLI and a Service Principal
- Create and manage a Resource Group and Storage Account
- Understand implicit dependencies between resources
- Use Terraform commands to update, validate, and destroy infrastructure

---

## 1. Setup and Authentication

### Step 1: Login to Azure

```sh
az login
```
This opens a browser window for authentication.

### Step 2: Create a Service Principal

Use a Service Principal for automation:

```sh
az ad sp create-for-rbac --name "az-demo" --role contributor --scopes /subscriptions/<your-subscription-id>
```

Copy and store the output values: `appId`, `displayName`, `password`, `tenant`.

### Step 3: Export Environment Variables

Set these environment variables for Terraform authentication:

```sh
export ARM_CLIENT_ID="<appId>"
export ARM_CLIENT_SECRET="<password>"
export ARM_TENANT_ID="<tenant>"
export ARM_SUBSCRIPTION_ID="<subscriptionId>"
```

---

## 2. Create Terraform Configuration

Create a file named `main.tf`:

```hcl
terraform {
    required_version = ">= 1.9.0"
    required_providers {
        azurerm = {
            source  = "hashicorp/azurerm"
            version = "~> 4.8.0"
        }
    }
}

provider "azurerm" {
    features {}
}
```

---

## 3. Define Azure Resources

**Resource Group:**

```hcl
resource "azurerm_resource_group" "example" {
    name     = "example-resources"
    location = "West Europe"
}
```

**Storage Account:**

```hcl
resource "azurerm_storage_account" "example" {
    name                     = "techtutorials101"
    resource_group_name      = azurerm_resource_group.example.name
    location                 = azurerm_resource_group.example.location
    account_tier             = "Standard"
    account_replication_type = "GRS"
    tags = {
        environment = "staging"
    }
}
```

*Note: The storage account depends on the resource group via references.*

---

## 4. Initialize Terraform

Initialize your working directory:

```sh
terraform init
```

This creates the `.terraform/` directory and `.terraform.lock.hcl` file.

---

## 5. Validate and Plan

Validate your configuration:

```sh
terraform validate
```

Preview changes:

```sh
terraform plan
```

Expected output:  
`Plan: 2 to add, 0 to change, 0 to destroy`

---

## 6. Apply the Configuration

Apply the configuration:

```sh
terraform apply
```

Confirm with `yes` when prompted, or use `-auto-approve` to skip confirmation.

---

## 7. Update Resources

To update (e.g., change replication type):

```hcl
account_replication_type = "LRS"
```

Then run:

```sh
terraform plan
terraform apply -auto-approve
```

---

## 8. Destroy Resources

Clean up resources:

```sh
terraform destroy -auto-approve
```

---

## 9. Understanding Desired vs Actual State

- The `.tf` files define your **desired state**.
- The Terraform state file tracks the **actual state** in Azure.
- Terraform compares both to determine what to create, update, or destroy.

---

## Summary

You have learned:

- How to set up the Azure provider in Terraform
- Authentication using a Service Principal
- Creating a Resource Group and Storage Account
- Managing dependencies between resources
- Using `plan`, `apply`, `validate`, and `destroy` commands
- Safely updating and cleaning up resources