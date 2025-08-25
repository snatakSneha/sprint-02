# Terraform Modules Documentation
<img width="800" height="336" alt="image" src="https://github.com/user-attachments/assets/51884448-1f71-47af-baf1-6a3c163df897" />


---

## Author Metadata

| Created by   | Created on | Version | Last Updated On | Pre Reviewer    | L0 Reviewer | L1 Reviewer   | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | --------------- | ----------- | ------------- | ------------ |
| Sneha-Joshi  | 21-08-2025 | V 1     | 25-08-2025      | Sahil/Siddharth | Ram-Ratan   | Gaurav-Singla | Mahesh-Kumar |

---

## Table of Contents

- [Introduction](#introduction)
- [What are Terraform Modules?](#what-are-terraform-modules)  
- [Why Use Terraform Modules?](#why-use-terraform-modules)  
- [Key Features](#key-features)  
- [Module Structure](#module-structure)  
- [Input Variables](#input-variables)  
- [Output Values](#output-values)  
- [Usage Example](#usage-example)  
- [Best Practices](#best-practices)  
- [Use Cases](#use-cases)
- [Contact Information](#contact-information) 
- [References](#references)  
  
---

## Introduction

Terraform Modules are reusable, configurable building blocks for defining and provisioning infrastructure in a consistent and scalable way. By grouping related resources, modules enable teams to create a standardized approach for deploying infrastructure across environments and projects.  

This documentation provides a detailed explanation of what Terraform Modules are, why they are essential, their structure, usage, best practices, and common use cases.

---

## What are Terraform Modules?

A **Terraform Module** is a container for multiple resources that are used together.  
- A module consists of a collection of `.tf` configuration files in a directory.  
- The root module is the main directory where Terraform operations are executed, and it can call other **child modules**.  
- Modules help encapsulate resource definitions and make them reusable, version-controlled, and easier to maintain.

---

## Why Use Terraform Modules?

| Benefit                    | Description                                                                 |
| -------------------------- | --------------------------------------------------------------------------- |
| **Reusability**            | Modules allow teams to reuse configurations across environments and teams. |
| **Consistency**            | Enforces a standard approach to provisioning similar infrastructure.       |
| **Scalability**            | Easily replicate infrastructure at scale with minimal effort.              |
| **Maintainability**        | Centralized codebase makes it easier to apply updates and bug fixes.       |
| **Collaboration**          | Teams can share modules through private registries or version control.     |

---

## Key Features

| S.no  | Feature                                           | Description                                                                 |
|----|--------------------------------------------------|---------------------------------------------------------------------------|
| 1  | **Encapsulation of Infrastructure Components**   | Groups related resources together for modular and reusable design.        |
| 2  | **Version-Controlled Reusable Code**             | Modules can be versioned and reused across multiple projects/environments.|
| 3  | **Dynamic Variables**                            | Accepts input variables to customize infrastructure deployments.          |
| 4  | **Simplified Outputs**                           | Provides outputs for easy integration with other modules or root configs. |
| 5  | **Terraform Registry Compatibility**             | Easily shareable and consumable through Terraform Registry.               |

---

## Module Structure

A typical module directory structure:  

```bash

my-module/
├── main.tf 
├── variables.tf 
├── outputs.tf
├── versions.tf 
└── README.md 
```

**Module Structure Description**

| File Name        | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **main.tf**      | Defines the core infrastructure resources that this module manages.         |
| **variables.tf** | Declares all input variables, their types, descriptions, and default values.|
| **outputs.tf**   | Specifies the output values to share with other modules or the root module. |
| **versions.tf**  | Sets Terraform and provider version constraints for compatibility.          |
| **README.md**    | Provides documentation on the module's purpose, variables, and usage.      |

---

## Input Variables

Input variables are parameters passed to modules to customize their behavior.  
Example:

```hcl
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t2.micro"
}
```

---

## Output Values

Outputs allow modules to return values to the root module or other modules.
Example:

```hcl
output "instance_id" {
  description = "The ID of the created EC2 instance"
  value       = aws_instance.my_ec2.id
}
```

---

## Usage Example

Root module calling a reusable EC2 module:

```hcl
module "ec2_instance" {
  source        = "./modules/ec2"
  instance_type = "t3.medium"
  region        = "us-east-1"
}
```

---

## Best Practices

| S.no  | Practice                              | Description                                                            |
|----|--------------------------------------|------------------------------------------------------------------------|
| 1  | **Follow a Consistent Structure**    | Use `main.tf`, `variables.tf`, `outputs.tf`, and `versions.tf` for organization. |
| 2  | **Use Version Pinning**              | Lock module versions to avoid unexpected changes.                      |
| 3  | **Write Clear Documentation**        | Include usage instructions, variables, and outputs in `README.md`.     |
| 4  | **Keep Modules Small and Focused**   | Modules should solve a single infrastructure concern.                 |
| 5  | **Validate Modules**                 | Use `terraform validate` and automated CI/CD checks.                  |
| 6  | **Leverage Terraform Registry**      | Share and consume community-verified modules.                         |
| 7  | **Tag Releases**                     | Use Git tags for version control.                                     |

---

## Use Cases

| S.no  | Use Case                                     | Description                                                      |
|----|---------------------------------------------|------------------------------------------------------------------|
| 1  | **Provisioning VPCs**                       | Create VPCs with subnets, gateways, and route tables.           |
| 2  | **Managing Compute Resources**              | Deploy EC2 instances                    |
| 3  | **Creating IAM Roles and Policies**         | Define access control policies for cloud resources.             |
| 4  | **Deploying Databases**                     | Automate database setup (PostgreSQL).            |
| 5  | **Networking Configurations**               | Configure load balancers, security groups, and network rules.   |
| 6  | **Multi-Cloud Infrastructure**              | Build consistent infra templates for AWS, etc.      |


---

## Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Sneha Joshi | [sneha.joshi.snaatak@mygurukulam.co](mailto:sneha.joshi.snaatak@mygurukulam.co) |

---

## References

| S.no | Resource                          | Link                                                                                      | Description                                       |
|----|----------------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------|
| 1  | Terraform Official Documentation | [View Docs](https://developer.hashicorp.com/terraform/docs)                               | Official documentation for Terraform usage.       |
| 2  | Terraform Registry               | [Visit Registry](https://registry.terraform.io/)                                         | Library of publicly available Terraform modules.  |
| 3  | Terraform Best Practices         | [Read Guide](https://www.terraform.io/docs/language/modules/index.html)                  | Official guide on writing and structuring modules.|

