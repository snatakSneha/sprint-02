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

<details>
<summary>Click here to view Screenshot</summary>
 <img width="1853" height="573" alt="image" src="https://github.com/user-attachments/assets/a54d0805-37df-4a06-a048-9d1707dd5b67" />
</details>

3. Search for the tag you want to track (e.g., `Project`).
4. Select the checkbox next to your tag.
   
<details>
<summary>Click here to view Screenshot</summary>
<img width="1853" height="573" alt="Screenshot from 2025-08-12 10-58-29" src="https://github.com/user-attachments/assets/7c3aa625-4279-43e9-bce6-ff5bb4bbcfc2" />
</details>

5. Click **Activate**.

<details>
<summary>Click here to view Screenshot</summary>
 <img width="1853" height="573" alt="image" src="https://github.com/user-attachments/assets/cb97e21d-c2b0-45c5-bea8-98f31d8fe92b" />
</details>


6. Wait up to **24 hours** for the tag to appear in Cost Explorer.



---

### 2- Save the Report in AWS

1. Click Save to report library (top right, orange button).

<details>
<summary>Click here to view Screenshot</summary>
<img width="1486" height="584" alt="Screenshot from 2025-08-13 09-12-24" src="https://github.com/user-attachments/assets/16b12a22-67c6-4aa5-9807-590f412dd20f" />
</details>

2. Give your report a name (e.g., Project_Tag_Cost_Report).

<details>
<summary>Click here to view Screenshot</summary>
<img width="1861" height="633" alt="Screenshot from 2025-08-13 13-04-25" src="https://github.com/user-attachments/assets/813bb53b-b69c-4f15-9430-c3d4b944cc6c" />
</details>

3. Add an optional description for clarity.

4. Click Save.
   
<details>
<summary>Click here to view Screenshot</summary>
<img width="1861" height="782" alt="Screenshot from 2025-08-13 13-05-17" src="https://github.com/user-attachments/assets/c3e3695b-67b0-484d-b4c6-d86493fbf8bb" />
</details>

---

### 3- Save and Export Reports
1. Once the desired view is ready, click **Save As** to store the report in AWS.
   
<details>
<summary>Click here to view Screenshot</summary>
<img width="1851" height="691" alt="image" src="https://github.com/user-attachments/assets/8ddd41c8-89fe-48d8-8ff4-d36e8258665e" />
</details>

2. Optionally, click **Download CSV** to export the data.

<details>
<summary>Click here to view Screenshot</summary>
<img width="1861" height="384" alt="Screenshot from 2025-08-13 13-05-53" src="https://github.com/user-attachments/assets/9819661e-56aa-4d70-851c-f8f550c66d11" />
</details>

3. Share the exported file with stakeholders for review.


---
[Upl"Project","No tag key: Project($)","Total costs($)"
"Project total","4.8249706407","4.8249706407"
"2025-02-01","","0"
"2025-03-01","","0"
"2025-04-01","","0"
"2025-05-01","","0"
"2025-06-01","","0"
"2025-07-01","4.8249706407","4.8249706407"oading costs.csv…]()

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

