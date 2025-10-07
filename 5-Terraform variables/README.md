# Terraform Variables (Input, Output, and Locals)

## Overview

In this session, we explore Terraform variables—how to define, use, and manage them effectively. Variables make Terraform configurations reusable, dynamic, and easier to maintain.

Terraform provides three main types of variables:

- **Input Variables** – Values provided to Terraform configuration.
- **Output Variables** – Values returned from Terraform after execution.
- **Local Variables (Locals)** – Internally used computed values within Terraform configuration.

---

## 1. Understanding Variable Types

### Input Variables

- Used to pass values into Terraform configurations.
- Help avoid hardcoding and allow parameterization.

### Output Variables

- Used to display or export values after applying Terraform.
- Example: printing resource IDs, names, or connection strings.

### Local Variables

- Used for internal calculations or repeated expressions.
- Improve code readability and reduce repetition.

---

## 2. Variable Types and Constraints

Terraform supports both primitive and complex data types:

- **Primitive Types:** `string`, `number`, `bool`
- **Complex Types:** `list`, `set`, `map`, `object`, `tuple`

If you don’t specify a type, the default is `any`, meaning it can store any value.

---

## 3. Example Project Setup

We’ll reuse the previous project (Day 4) that created a resource group and storage account.

**Steps:**

1. Copy the existing `main.tf` from Day 4.
2. Paste it into a new folder `day5/`.
3. Update the configuration to include input, output, and local variables.

---

## 4. Input Variables

### Why Use Input Variables

- Avoid hardcoding values like locations, tags, or environment names.
- Enable flexibility and reusability across multiple configurations.

**Example:** Instead of defining `environment = staging` in every resource, define it once as a variable.

### Defining a Variable

In `main.tf` (or preferably a new `variables.tf`):

```hcl
variable "environment" {
    type        = string
    description = "Environment type"
    default     = "staging"
}
```

### Using a Variable

Reference variables using the syntax: `var.<variable_name>`

```hcl
tags = {
    environment = var.environment
}
```

---

## 5. Initializing and Running

Run the following commands:

```sh
terraform init
terraform plan
```

**Expected output:**

```
Plan: 2 to add, 0 to change, 0 to destroy
+ environment = "staging"
```

---

## 6. Variable Value Precedence

Terraform provides multiple ways to assign values to variables, with different precedence levels (later sources override earlier ones):

**Order of Precedence (Lowest → Highest):**

1. Environment variables
2. Terraform `.tfvars` or `.tfvars.json` files
3. Auto-loaded variable files (`*.auto.tfvars`)
4. Explicit command-line variable files (`-var-file`)
5. Command-line `-var` flag

### Methods to Pass Values

1. **Default Value** (inside the variable block):
        ```hcl
        default = "staging"
        ```
2. **Command Line (`-var`):**
        ```sh
        terraform plan -var="environment=dev"
        ```
3. **`.tfvars` File:**
        - Create a file `terraform.tfvars`:
            ```
            environment = "demo"
            ```
        - Run normally:
            ```sh
            terraform plan
            ```
4. **Environment Variable:**
        ```sh
        export TF_VAR_environment="commandline"
        terraform plan
        ```

---

## 7. Output Variables

### Purpose

To display or export values after Terraform execution.

**Example:**

```hcl
output "storage_account_name" {
    value = azurerm_storage_account.example.name
}
```

### Usage

After applying Terraform:

```sh
terraform apply
terraform output
```

**Output:**

```
storage_account_name = techtutorials101
```

You can also reference this output in:

- Other Terraform configurations (using remote state)
- Scripts
- CI/CD pipelines

---

## 8. Local Variables

### Purpose

- Used for intermediate calculations or repeated values.
- Ideal when the value doesn’t change often.

### Defining a Local Block

```hcl
locals {
    common_tags = {
        environment = "dev"
        lob         = "banking"
        stage       = "alpha"
    }
}
```

### Using a Local Variable

```hcl
tags = local.common_tags
```

To reference a specific value:

```hcl
tags = {
    environment = local.common_tags.environment
}
```

**Example Output:**

When running:

```sh
terraform plan
```

**Output:**

```
environment = alpha
```

If you omit the key (like `.environment`), Terraform will throw a type mismatch error since it expects a string, not a map.

---

## 9. Summary

| Type           | Use Case                | Syntax         | Example              |
|----------------|------------------------|---------------|----------------------|
| Input Variable | Accept external values  | `var.name`    | `var.environment`    |
| Output Variable| Display results         | `output`      | `terraform output`   |
| Local Variable | Internal reusable logic | `local.name`  | `local.common_tags`  |

---

## Conclusion

You’ve now learned:

- How to define and use input, output, and local variables
- How Terraform determines variable precedence
- Practical methods to manage values cleanly across configurations

This is one of the most important fundamentals to make your Terraform projects modular and reusable.
