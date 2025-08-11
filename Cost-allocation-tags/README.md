# AWS Cost Allocation Tags — POC

This README documents a proof-of-concept (POC) for implementing AWS Cost Allocation Tags across an account (or set of accounts) so cost can be attributed correctly to teams, projects and applications. It includes acceptance criteria, prerequisites, a step-by-step setup, a demo checklist for Pre & L0 reviewers, right-sizing guidance to run before tagging, spot instance exploration, troubleshooting guidance, conclusion and references.

---



## Author information

| Created    | Version | Author      | Modified   | Comment         | Reviewer                    |
| ---------- | ------- | ----------- | ---------- | --------------- | --------------------------- |
| 11-08-2025 |   V1    | Sneha Joshi | 11-08-2025 | Internal Review | Siddharth Pawar/Sahil Gupta |

---

## Table of Contents

* [Overview](#overview)
* [Prerequisites](#prerequisites)
* [Tagging strategy and conventions](#tagging-strategy-and-conventions)
* [Right-sizing (required before tagging)](#right-sizing-required-before-tagging)
* [Spot instances: exploration & recommendations](#spot-instances-exploration--recommendations)
* [Step-by-step setup guide](#step-by-step-setup-guide)

  * [1. Define tags and policy](#1-define-tags-and-policy)
  * [2. IAM and permissions](#2-iam-and-permissions)
  * [3. Bulk tagging (Tag Editor / CLI)](#3-bulk-tagging-tag-editor--cli)
  * [4. Activate tags for Cost Allocation](#4-activate-tags-for-cost-allocation)
  * [5. Validate with Cost Explorer and CUR](#5-validate-with-cost-explorer-and-cur)
  * [6. Operationalize (guardrails & automation)](#6-operationalize-guardrails--automation)
* [Demo checklist for Pre & L0 reviewers](#demo-checklist-for-pre--l0-reviewers)
* [Troubleshooting](#troubleshooting)
* [Conclusion](#conclusion)
* [References](#references)

---

## Overview

Cost allocation tags allow you to allocate AWS costs to owners, projects, departments and applications. The goal of this POC is to demonstrate a reproducible, auditable tagging process that: (a) ensures tags are applied consistently, (b) are activated for Cost Explorer / CUR, and (c) enable billing transparency for Pre & L0 reviewers.

This POC also requires a *right-sizing* exercise and a *spot instance exploration* to be completed **before** enabling tags broadly—so cost data reflects an optimized baseline and teams have guidance for using spot capacity safely.

---

## Prerequisites

* AWS account(s) with Billing access (root or user with `aws-portal` billing permissions) to activate cost allocation tags and enable Cost Explorer/CUR.
* An IAM administrator or tagging-admin role with permission to tag resources (see IAM example below).
* AWS CLI v2 configured with credentials for an account with tagging permissions.
* Cost Explorer enabled in the account (required to view reports filtered by tags).
* Optionally: AWS Organizations if you plan to roll out tagging policy across multiple accounts.
* Access to AWS Compute Optimizer and Cost Explorer rightsizing recommendations (for the pre-tagging right-sizing step).
* A CI/CD or automation mechanism (Lambda, SSM Run Command, Terraform, or CloudFormation) for enforcing tags and auto-tagging new resources.
* S3 bucket and role to host the Cost and Usage Report (CUR) if you will enable CUR for detailed reporting.

---

## Tagging strategy and conventions

A consistent tagging standard is critical. Below is a minimal recommended set (adjust to your org's needs):

* `Environment` (e.g., prod, staging, dev, test)
* `Project` or `Application` (project short name)
* `Owner` (team or individual)
* `CostCenter` (accounting code)
* `BillingCode` (optional finer-grained allocation)
* `Stack` (e.g., web, backend, data)
* `Expiry` or `CreatedOn` (optional lifecycle)

Naming conventions (examples):

* Keys are TitleCase (e.g., `CostCenter`).
* Values should be consistent: use agreed shortcodes for projects and cost centers.
* Limit tag values to a stable vocabulary (document this in a central place).
* Document allowed keys/values in a CSV or central tag policy repository.

Tagging limits and notes:

* AWS typically supports up to 50 tags per resource. Keep essential tags only.
* Not all AWS resource types automatically propagate tags—check service-specific docs.

---

## Right-sizing (required before tagging)

Rationale: if you enable cost allocation tags before optimizing instance sizes, your baseline costs will reflect over-provisioning. To create an accurate baseline, run a right-sizing exercise.

Steps (high level):

1. **Enable Compute Optimizer and Cost Explorer** (if not already enabled). Compute Optimizer requires a few days of metrics to produce recommendations.
2. **Collect historical utilization** (CPU, memory where available, network, I/O) for 14–30 days.
3. **Fetch recommendations**

   * Use **Compute Optimizer** for EC2, Auto Scaling groups, EBS and Lambda recommendations.
   * Use **Cost Explorer** rightsizing recommendations and analyze cost vs utilization.
4. **Classify instances** as: `Safe to downsize`, `Monitor`, `Do not change`.
5. **Plan maintenance windows** for resizing (take snapshots/backups where appropriate).
6. **Test** the downsized instance family in a staging environment.
7. **Rollout** changes in a controlled manner (canary or phased approach).
8. **Document changes** and update the tagging plan to include `RightSizeApprovedBy` or similar if desired.

Outcome: produce a short report (CSV) listing instances changed, before/after instance types, expected savings estimate and validation results. This report will be used during the demo.

---

## Spot instances: exploration & recommendations

Spot instances can deliver significant savings but come with interruption risk. Include spot exploration as part of the POC:

* **Use cases**: batch processing, stateless web nodes behind a load balancer, worker queues, CI runners, test jobs.
* **Not suitable** for stateful databases unless you have a robust replication/restore strategy.
* **Recommended patterns**:

  * Use **Auto Scaling Groups** with a `MixedInstancesPolicy` (On-Demand + Spot). Set a fallback to On‑Demand.
  * Use **Spot Fleet / EC2 Fleet** with allocation strategies (capacity-optimized or lowest-price depending on tolerance).
  * Design for interruption: use Spot interruption notices (2-minute warning) and implement graceful shutdown hooks.
* **Tagging**: treat Spot instances the same as On-Demand for tagging and billing attribution.
* **Measure**: collect savings estimates by running a small portion of workload on Spot and track interruption rates and restart overhead.

Include one small pilot in this POC: a stateless ASG with 50% spot capacity, 50% on‑demand fallback.

---

## Step-by-step setup guide

### 1. Define tags and policy

1. Create a canonical tag list (CSV) containing `Key,Description,AllowedValues,Owner` columns.
2. Agree naming conventions with cost owners and finance.
3. Store the list in a central repo (Git) and expose it to automation jobs.

*Example CSV row:*

```
Key,Description,AllowedValues,Owner
Environment,Deployment environment,dev|staging|prod,PlatformTeam
Project,Project short name,PROJECTX|PROJECTY,ProductTeam
Owner,Team or person,email alias,PlatformTeam
CostCenter,Accounting code,1001|1002,Finance
```

---

### 2. IAM and permissions

Create a tagging-admin role (or use an existing admin role) that can apply tags across services. Use least privilege in production but for the POC you can start with a broader set of actions and then refine.

*Sample IAM policy (example — review before use):*

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "resourcegroupstaggingapi:GetResources",
        "resourcegroupstaggingapi:TagResources",
        "resourcegroupstaggingapi:UntagResources",
        "ec2:CreateTags",
        "ec2:DeleteTags",
        "s3:PutBucketTagging",
        "s3:DeleteBucketTagging",
        "rds:AddTagsToResource",
        "rds:RemoveTagsFromResource",
        "lambda:TagResource",
        "elasticloadbalancing:AddTags",
        "autoscaling:CreateOrUpdateTags",
        "tag:GetResources",
        "tag:GetTagKeys",
        "tag:GetTagValues"
      ],
      "Resource": "*"
    }
  ]
}
```

Attach this policy to the role/account you will use for bulk tagging. After the POC, tighten to least privilege.

---

### 3. Bulk tagging (Tag Editor / CLI)

Options:

* **AWS Console**: Resource Groups > Tag Editor (recommended for initial exploratory tagging).
* **AWS CLI**: good for automation and scripting.
* **Terraform / CloudFormation**: add tags at provisioning time to prevent drift.

*Example commands:*

* EC2:

```bash
aws ec2 create-tags --resources i-0123456789abcdef0 --tags Key=Owner,Value=alice Key=Project,Value=MyProject
```

* S3:

```bash
aws s3api put-bucket-tagging --bucket my-bucket --tagging 'TagSet=[{Key=Owner,Value=alice},{Key=Project,Value=MyProject}]'
```

* RDS:

```bash
aws rds add-tags-to-resource --resource-name arn:aws:rds:us-east-1:123456789012:db:mydb --tags Key=Owner,Value=alice Key=Project,Value=MyProject
```

* Tag Editor (console):

  1. Open Resource Groups > Tag Editor.
  2. Select regions and resource types.
  3. Search and select resources.
  4. Add tags in bulk and save.

*Bulk tag via Resource Groups Tagging API (CLI):*

```bash
aws resourcegroupstaggingapi tag-resources --resource-arn-list arn:aws:ec2:us-east-1:123456789012:instance/i-0123456789abcdef0 --tags Owner=alice Project=MyProject
```

Notes:

* Test tagging on a small set first.
* Keep a CSV of changed resources and tags for audit.

---

### 4. Activate tags for Cost Allocation

1. Sign in to AWS Management Console with an account that has Billing permissions.
2. Open **Billing & Cost Management** -> **Cost allocation tags**.
3. Under *User-defined tags*, find the keys you want to include and click **Activate** for each key.
4. Wait for AWS to process the change (usually tags start showing up in Cost Explorer and CUR within 24 hours; allow up to 48 hours for full coverage).

**Important**: Activating a tag key does not retroactively attach tags to resources — it only tells Billing to include that tag as a dimension in reports. Ensure resources are tagged first or in parallel.

---

### 5. Validate with Cost Explorer and CUR

1. Enable **Cost Explorer** (Billing -> Cost Explorer) if it is not already enabled.
2. Use Cost Explorer to create a report grouped by `Tag` -> choose your tag key (for example `Project`).
3. Create a Cost and Usage Report (CUR) if you want full granularity:

   * Billing -> Cost & Usage Reports -> Create report
   * Include resource IDs and enable tags
   * Configure S3 destination and optional Athena / Glue tables for analysis
4. Validate results by comparing pre-tagging vs post-tagging exports and ensure tag values appear in the CUR and Cost Explorer.

---

### 6. Operationalize (guardrails & automation)

1. **Enforce tags at creation time**

   * Add required tags in Terraform/CloudFormation templates.
   * Use CloudFormation stack-level tags.
   * Add tagging step in CI pipelines that roll out resources.
2. **Detect drift**

   * Use AWS Config rules (e.g., `required-tags`) to detect resources missing mandatory tags.
   * Use Lambda/Step Functions to auto-tag newly created resources where appropriate.
3. **Reporting & Alerts**

   * Build dashboards in Cost Explorer or QuickSight grouped by tags.
   * Create CloudWatch events / automated reports listing resources without tags and notify owners via Slack/Email.
4. **Periodic audit**

   * Schedule weekly or monthly audits and generate CSVs for finance review.

---


## Troubleshooting

**Issue: Tags not appearing in Cost Explorer or CUR**

* Verify that the tag key is *activated* in Billing -> Cost allocation tags.
* Ensure resources have the tag applied.
* Wait 24–48 hours after activation for reporting systems to include tags.
* Confirm Cost Explorer is enabled.

**Issue: Permission denied when applying tags**

* Confirm IAM role has `resourcegroupstaggingapi` and service-specific tagging permissions (EC2, S3, RDS etc.).

**Issue: Some resources not supporting tags or tags not propagating**

* Check service-specific docs — some services require tags at creation time or use service-specific tagging APIs.
* Use Resource Groups Tag Editor for service coverage checks.

**Issue: CUR not showing tag columns**

* When creating CUR, ensure you include resource IDs and include tags. CUR configuration is needed before you can query tags at fine granularity.

**Issue: Too many tag keys or inconsistent values**

* Revisit tag list, deprecate redundant keys, and normalize values via a canonical list.

---

## Conclusion

This README provides a POC blueprint to implement AWS cost allocation tags in a controlled way: run right-sizing first, run a spot pilot, define a canonical set of tags, apply tags (console/CLI/automation), activate tags in Billing, and validate results in Cost Explorer and CUR. Operationalize using AWS Config, automation and periodic audits.

The POC should deliver:

* Demonstrable cost attribution for owners/projects.
* A repeatable tagging process for future rollouts.
* A pilot demonstrating safe spot instance usage and expected savings.

---


## Contact information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Sneha Joshi | [sneha.joshi.snaatak@mygurukulam.co](mailto:sneha.joshi.snaatak@mygurukulam.co) |


---

## References

| Topic                    | Link                                                                 |
|-------------------------|----------------------------------------------------------------------|
| Cost Allocation Tags    | [Link](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-115-anuj/Cost-Optimization/aws-cost-allocation-tags/README.md) |
| AWS SCP                 | [Link](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-116-aryan-mishra/Cost-Optimization/Service-Control-Policies/README.md) |
| AWS Cost Reports via Cost Explorer| [Link](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-118-Sneha-Joshi/Cost-Optimization/Cost-Tag-Reports/README.md)|

---
