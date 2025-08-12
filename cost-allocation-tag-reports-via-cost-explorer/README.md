# Cost Tag Reports via Cost Explorer
<img width="1200" height="430" alt="image" src="https://github.com/user-attachments/assets/bd4101f9-625a-44d4-b039-a199125aa9c1" />

---

## Author information

| Created    | Version | Author      | Modified   | Comment         | Reviewer                    |
| ---------- | ------- | ----------- | ---------- | --------------- | --------------------------- |
| 12-08-2025 |   V1    | Sneha Joshi | 12-08-2025 | Internal Review | Siddharth Pawar/Sahil Gupta |

---

## Table of Contents
-  [Introduction](#introduction)
-  [Pre-requisites](#pre-requisites)
-  [Step-by-Step Setup Guide](#step-by-step-setup-guide)
-  [Example Use Case](#example-use-case)
-  [Best Practices](#best-practices)
-  [Contact Information](#contact-information)
-  [References](#references)

---

##  Introduction

AWS **Cost Tag Reports** help analyze and track costs by associating AWS resource usage with **tags** (e.g., `Project`, `Environment`, `Owner`). Using **AWS Cost Explorer**, you can filter and group costs based on these tags to generate meaningful financial and operational insights.  

 **Related Documentation:**  
 [Cost Tag Reports via Cost Explorer Documentation (Last Sprint)](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-118-Sneha-Joshi/Cost-Optimization/Cost-Tag-Reports/README.md)

---

##  Pre-requisites

| Requirement | Description | Link |
|-------------|-------------|------|
| AWS Console Access | Ensure you have access to AWS Management Console with Billing permissions | N/A |
| Cost Allocation Tags | Tags should be created and applied to AWS resources | [AWS Cost Allocation Tag POC](https://github.com/snatakSneha/sprint-02/tree/main/cat) |
| Documentation Reference | Last sprint's Cost Tag Reports guide | [Last Sprint Documentation](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-118-Sneha-Joshi/Cost-Optimization/Cost-Tag-Reports/README.md) |
| Cost Explorer Enabled | Ensure Cost Explorer is activated in AWS | [AWS Cost Explorer Docs](https://docs.aws.amazon.com/cost-explorer/latest/userguide/ce-user-guide.html) |

---

##  Step-by-Step Setup Guide

### 1- Enable Cost Allocation Tags
1. Sign in to the **AWS Management Console**.
2. Navigate to **Billing → Cost Allocation Tags**.
3. Search for the tag you want to track (e.g., `Project`).
4. Select the checkbox next to your tag.
5. Click **Activate**.
6. Wait up to **24 hours** for the tag to appear in Cost Explorer.

---

### 2- Activate Tags in Billing
1. Go to **Billing → Cost Allocation Tags**.
2. Verify that your tag’s **Status** is set to **Active**.
3. If not, select the tag and click **Activate** again.

---

### 3- Access Cost Explorer
1. In the AWS Management Console, go to **Cost Explorer**.
2. Click **Launch Cost Explorer**.
3. Select a **Date Range** for your report (e.g., last month, this month-to-date).

---

### 4- Apply Filters and Generate Reports
1. In the **Filter** section, select **Tags**.
2. Choose the activated tag key (e.g., `Project`).
3. Select specific **tag values** (e.g., `POC-AWS-Cost`).
4. Group results by **Service** or **Linked Account** for deeper insights.
5. Click **Apply Filters** to refresh the report.

---

### 5- Save and Export Reports
1. Once the desired view is ready, click **Save As** to store the report in AWS.
2. Optionally, click **Download CSV** to export the data.
3. Share the exported file with stakeholders for review.

---

##  Example Use Case
If your organization has multiple projects running in AWS, you can:
- Tag all resources for **Project: Alpha**.
- Use Cost Explorer to generate a monthly report filtering only **Project: Alpha** costs.
- Share the data with finance teams for budgeting.

---

##  Best Practices
| Practice                                | Benefit                                  |
|-----------------------------------------|-------------------------------------------|
| Use a **consistent tagging strategy**   | Ensures uniform cost reporting            |
| Enable only **relevant tags**           | Reduces clutter in reports                |
| Review reports monthly                  | Helps detect unusual spending early       |
| Combine with **Budgets & Alerts**       | Enables proactive cost management         |

---

## Contact information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Sneha Joshi | [sneha.joshi.snaatak@mygurukulam.co](mailto:sneha.joshi.snaatak@mygurukulam.co) |


---

##  References

| Reference Name | Link |
| -------------- | ---- |
| AWS Cost Allocation Tags Documentation | [https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) |
| AWS Cost Explorer Documentation | [https://docs.aws.amazon.com/cost-explorer/latest/userguide/ce-user-guide.html](https://docs.aws.amazon.com/cost-explorer/latest/userguide/ce-user-guide.html) |
| Cost Allocation Tag POC (Previous Sprint) | [https://github.com/snatakSneha/sprint-02/tree/main/cat](https://github.com/snatakSneha/sprint-02/tree/main/cat) |
| Last Sprint Cost Tag Reports Documentation | [https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-118-Sneha-Joshi/Cost-Optimization/Cost-Tag-Reports/README.md](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-118-Sneha-Joshi/Cost-Optimization/Cost-Tag-Reports/README.md) |

