
# AWS Cost Allocation Tags – POC

---

## Author Metadata

| Created    | Version | Author      | Modified   | Comment         | Reviewer                    |
| ----------- | ------- | ----------- | ---------- | --------------- | --------------------------- |
|11-08-2025 |   V1    | Sneha Joshi | 31-07-2025 | Internal Review | Siddharth Pawar/Sahil Gupta |

---

## Objective

The purpose of this POC is to **configure and enable AWS Cost Allocation Tags** to track and allocate AWS resource costs.
These tags help in categorizing and identifying resource usage for cost management and reporting.

---

## Prerequisites

* **AWS Account** with permissions to:

  * Manage tags
  * Access AWS Billing Console
  * View Cost Explorer
* Basic understanding of AWS tagging concepts
* IAM user/role with:

  * `billing:EnableCostAllocationTags`
  * `tag:GetResources`
  * `tag:TagResources`

---

## Step-by-Step Setup

### 1. Access the Billing Console

1. Sign in to the AWS Management Console as **Root User** or **IAM user with billing access**.
2. Navigate to **Billing and Cost Management** service.

---

### 2. Create a Cost Allocation Tag

1. Go to **Resource Groups & Tag Editor** in AWS Console.
2. Click **Manage Tags** → **Create Tag**.
3. Enter:

   * **Key**: `Project` (or as per requirement)
   * **Value**: e.g., `POC-AWS-Cost`
4. Save the tag.

---

### 3. Assign Tag to Resources

1. In **Tag Editor**, select the desired **Region** and **Resource types**.
2. Select the resources you want to tag.
3. Click **Manage Tags of Selected Resources** → **Add Tag**.
4. Add the same key-value pair created above.
5. Save the changes.

---

### 4. Activate Tag for Cost Allocation

1. Open **Billing Console** → **Cost Allocation Tags**.
2. Locate your tag key (e.g., `Project`).
3. Check the box next to it.
4. Click **Activate**.
5. Wait up to **24 hours** for AWS to start applying it to billing data.

---

### 5. Verify in Cost Explorer

1. After at least 24 hours, open **Cost Explorer**.
2. Select **Group By** → **Tag**.
3. Choose your activated tag key (`Project`).
4. Review the cost breakdown by tag value.

---

## Expected Outcome

* The tag is visible in **Cost Allocation Tags** as **Active**.
* The Cost Explorer report can be grouped and filtered by the created tag.
* AWS CUR (Cost and Usage Report) contains the tag column for cost tracking.

---
## Troubleshooting

| Issue                                          | Possible Cause                                  | Resolution/Steps                                                      |
|-----------------------------------------------|------------------------------------------------|----------------------------------------------------------------------|
| Cost tags not appearing in Cost Explorer      | Tags not activated as cost allocation tags     | Go to AWS Billing → Cost Allocation Tags → Activate the required tags. |
| Cost reports show empty or incomplete data    | Wrong date range or filters in report settings | Adjust date range and filters in Cost Explorer or reports configuration. |
| Missing expected tag keys in reports          | Resources launched without proper tagging      | Use AWS Config or Resource Groups Tag Editor to find untagged resources and apply tags. |
| Tags not updated in cost reports after change | Delay in AWS cost data refresh                 | Wait 24-48 hours for AWS to reflect updated tags in billing reports.  |
| Cannot download cost report                   | IAM permissions missing for billing            | Ensure your IAM user/role has `billing:ViewBilling` and `billing:DownloadBillingReport` permissions. |
| Report export fails with error                | Browser/network issues                         | Try again with a stable connection or use a different browser/device. |

---
## Conclusion

Tagging AWS resources and using Cost Explorer for cost analysis is a powerful combination for controlling and optimizing cloud costs. With a disciplined tagging strategy, automated governance, and integration with AWS tools, organizations can maximize cloud ROI and accountability.

---

## Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Sneha Joshi | [sneha.joshi.snaatak@mygurukulam.co](mailto:sneha.joshi.snaatak@mygurukulam.co) |

---

## References

* [AWS Documentation – Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)
* [AWS Resource Tagging Best Practices](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)
* [Last Sprint Documentation](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-115-anuj/Cost-Optimization/Aws-Cost-Allocation-Tags/README.md)

