# AWS Cost Allocation Tags – POC
<img width="1200" height="428" alt="image" src="https://github.com/user-attachments/assets/b17cc866-c926-4b72-a042-72c99a00c084" />

---

## Author Metadata

| Created    | Version | Author      | Modified   | Comment         | Reviewer                    |
| ----------- | ------- | ----------- | ---------- | --------------- | --------------------------- |
|11-08-2025 |   V1    | Sneha Joshi | 31-07-2025 | Internal Review | Siddharth Pawar/Sahil Gupta |

---

## Objective

The purpose of this POC is to **configure and enable AWS Cost Allocation Tags** to track and allocate AWS resource costs.
These tags help in categorizing and identifying resource usage for cost management and reporting.
> For Detailed Documentation, Please vist [Link](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-115-anuj/Cost-Optimization/Aws-Cost-Allocation-Tags/README.md)


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
<details>
<summary>Click here to view Screenshot</summary>
<img width="1856" height="553" alt="image" src="https://github.com/user-attachments/assets/a04ccb27-3733-415d-94ef-116d24033d15" />
</details>

---

### 2. Create a Cost Allocation Tag

1. Go to **Resource Groups & Tag Editor** in AWS Console.
<details>
<summary>Click here to view Screenshot</summary>
 <img width="1059" height="184" alt="image" src="https://github.com/user-attachments/assets/7e02f809-d5be-428c-a2fd-29ba1328f1e2" />
</details>

2. Click **Manage Tags** → **Create Tag**.


3. Enter:

   * **Key**: `Project` (or as per requirement)
   * **Value**: e.g., `POC-AWS-Cost`
4. Save the tag.

---

### 3. Assign Tag to Resources

1. In **Tag Editor** select **Region**
2. Select **Resource types**
<details>
<summary>Click here to view Screenshot</summary>
   <img width="1844" height="897" alt="Screenshot from 2025-08-11 23-08-51" src="https://github.com/user-attachments/assets/d848037b-54c1-40f3-b802-bc2a86597e79" />
</details>

4. **Search resources**
<details>
<summary>Click here to view Screenshot</summary>
 <img width="1542" height="586" alt="Screenshot from 2025-08-11 23-02-22" src="https://github.com/user-attachments/assets/5b9e6258-3a63-408f-91f2-7293d00e489b" />
</details>

5. **Select resource**
<details>
<summary>Click here to view Screenshot</summary>
 <img width="1844" height="364" alt="Screenshot from 2025-08-11 23-07-50" src="https://github.com/user-attachments/assets/4d2732be-9012-4d33-878d-ed7fde261df0" />
</details>

6. **Manage tags of selected resources**
7. **Add Tag**
   * **Key** field : `Project`
   * **Value** field : `POC-AWS-Cost`
   * Save/Apply changes.
<details>
<summary>Click here to view Screenshot</summary>
  <img width="1846" height="902" alt="Screenshot from 2025-08-11 23-07-17" src="https://github.com/user-attachments/assets/fa8368ee-e508-4e4d-9ba2-eeb3177badbd" />
</details>

---

### 4. Activate Tag for Cost Allocation

1. Open **Billing Console** → **Cost Allocation Tags**.
<details>
<summary>Click here to view Screenshot</summary>
 <img width="1853" height="573" alt="image" src="https://github.com/user-attachments/assets/a54d0805-37df-4a06-a048-9d1707dd5b67" />
<details>
2. Locate your tag key (e.g., `Project`).
3. Check the box next to it.
4. Click **Activate**.
<details>
<summary>Click here to view Screenshot</summary>
<img width="1853" height="573" alt="Screenshot from 2025-08-12 10-58-29" src="https://github.com/user-attachments/assets/7c3aa625-4279-43e9-bce6-ff5bb4bbcfc2" />
<details>
5. Wait up to **24 hours** for AWS to start applying it to billing data.
<details>
<summary>Click here to view Screenshot</summary>
 <img width="1853" height="573" alt="image" src="https://github.com/user-attachments/assets/cb97e21d-c2b0-45c5-bea8-98f31d8fe92b" />
<details>
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

## References

| Title | Link |
|-------|------|
| AWS Documentation – Cost Allocation Tags | [View](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) |
| AWS Resource Tagging Best Practices | [View](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) |
| Last Sprint Documentation | [View](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-115-anuj/Cost-Optimization/Aws-Cost-Allocation-Tags/README.md) |

