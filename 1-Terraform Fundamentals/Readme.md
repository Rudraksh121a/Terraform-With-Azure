# Terraform Fundamentals

## Objective

Learn the fundamentals of Terraform and Infrastructure as Code (IaC):

- What is Infrastructure as Code
- Why we need it
- Benefits
- Terraform workflow
- Installation guide

---

## What is Infrastructure as Code (IaC)

**Infrastructure as Code (IaC)** means defining and managing infrastructure—servers, networks, databases, and more—using code instead of manual setup through a cloud console.

**Example:**  
A DevOps engineer writes Terraform code that provisions cloud servers automatically.  
Instead of manually creating resources via AWS, Azure, or GCP consoles, you use configuration files that:

- Create infrastructure (VMs, networks, databases)
- Update or destroy them programmatically

---

## Popular IaC Tools

| Tool                  | Description                        |
|-----------------------|------------------------------------|
| **Terraform**         | Cloud-agnostic IaC tool by HashiCorp |
| **Pulumi**            | IaC using familiar programming languages |
| **Azure ARM / Bicep** | Native IaC for Azure               |
| **AWS CloudFormation**| Native IaC for AWS                 |
| **GCP Deployment Manager** | Native IaC for Google Cloud   |

---

## Why Do We Need IaC

Consider a simple 3-tier architecture:

- **Web Tier:** 2 servers behind an external load balancer
- **App Tier:** Multiple servers behind an internal load balancer
- **Database Tier:** Master and replica databases

If you create this manually through the Azure portal or CLI:

- Takes about 2 hours per environment
- For six environments (Dev, UAT, SIT, Pre-Prod, DR, Prod) → around 12 hours total

### Problems with Manual Provisioning

- **Time-consuming** — slow to create and update environments
- **Human error** — risk of configuration mistakes
- **Inconsistency** — environments differ, causing “works on my machine” issues
- **High cost** — resources left running unnecessarily
- **Security risks** — limited control and auditing
- **Repetitive work** — same process repeated daily

---

## How Terraform Solves These Problems

- **Automation:**  
    Terraform automatically provisions and destroys infrastructure as needed. It can also schedule cleanups to reduce cost.

- **Consistency:**  
    The same Terraform code deploys identical environments such as Dev, QA, and Prod. This eliminates configuration drift and “works on my machine” problems.

- **Time and Cost Efficiency:**  
    Templates can be reused. Infrastructure can be created or destroyed with a single command.

- **Version Control:**  
    Terraform code is stored in Git. All changes can be tracked, reviewed, and rolled back easily.

- **Security and Governance:**  
    Supports role-based access controls and enables auditing through Git and CI/CD tools.

---

## Terraform Workflow

### Key Files

- `.tf` → Terraform configuration files
- `.tf.json` → JSON-based configuration format (optional)

### Basic Commands

| Command            | Description                                 |
|--------------------|---------------------------------------------|
| `terraform init`   | Initializes the working directory and downloads provider plugins |
| `terraform validate` | Validates the syntax of configuration files |
| `terraform plan`   | Shows the execution plan (dry run)          |
| `terraform apply`  | Provisions or updates infrastructure        |
| `terraform destroy`| Removes infrastructure defined in code      |

These commands can be executed manually or automated using CI/CD pipelines such as Azure DevOps, GitHub Actions, or Jenkins.

---

## Terraform Architecture

- **Terraform Configuration Files (.tf):** Define the desired infrastructure
- **Terraform CLI or CI/CD Tool:** Executes commands
- **Provider Plugin:** Connects Terraform to a cloud provider (AWS, Azure, GCP, etc.)
- **Terraform State File (.tfstate):** Tracks the current infrastructure state
- **Backend (e.g., remote storage):** Stores state securely for collaboration

---

## Installing Terraform

### Installation Options

- **Official download link:**  
    [https://developer.hashicorp.com/terraform/downloads](https://developer.hashicorp.com/terraform/downloads)

#### For macOS (using Homebrew)
```sh
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

#### For Windows (using Chocolatey)
```sh
choco install terraform
```
Or download the binary from the Terraform website.

#### For Linux
```sh
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform
```

### Verify Installation

```sh
terraform -version
```

**Example Output:**
```
Terraform v1.9.8
on darwin_arm64
```


## Summary

| Concept      | Key Idea                                 |
|--------------|------------------------------------------|
| IaC (Infrastructure as Code) | Managing infrastructure using code |
| Terraform    | Cloud-agnostic IaC tool by HashiCorp     |
| Benefits     | Automation, consistency, speed, and cost reduction |
| Key Commands | init, plan, apply, destroy               |
| Installation | Simple process via package manager or binary |
| Use Case     | Reusable templates for consistent deployments |

---

## End of Day 1

You should now understand:

- What Terraform and IaC are
- Why Terraform is useful in DevOps

