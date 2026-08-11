---
title: "Week 6 Worklog"
date: 2026-08-07
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:
* Continue developing and finalizing advanced business modules for the system.
* Automate document approval workflows using AWS Step Functions integrated with Amazon SNS for automated email notifications.
* Package and deploy the entire Serverless backend using Infrastructure as Code (AWS SAM).

### Tasks to be carried out this week:

| Day | Task Details | Start Date | Completion Date |
| :--- | :--- | :--- | :--- |
| **1** | **Module Development & Approval Workflow (Step Functions)**<br>- Expanded and developed auxiliary functional modules for the EDMS system.<br>- Built a State Machine on AWS Step Functions to orchestrate the strict document approval workflow. | 27/07/2026 | 27/07/2026 |
| **2** | **Event Notifications (Amazon SNS)**<br>- Integrated Amazon SNS to trigger automated email alerts when documents are approved or shared. | 28/07/2026 | 28/07/2026 |
| **3** | **Secure Document Sharing & Lifecycle Management**<br>- Programmed controlled sharing features, generating time-limited Pre-signed URLs.<br>- Implemented a Soft Delete mechanism, moving documents to a TRASH state with a 30-day recovery window. | 29/07/2026 | 30/07/2026 |
| **4** | **Automated Permanent Deletion (Hard Delete)**<br>- Configured TTL attributes on DynamoDB so the system automatically permanently deletes expired documents without manual intervention. | 31/07/2026 | 31/07/2026 |
| **5** | **Infrastructure as Code & Cloud Deployment (AWS SAM)**<br>- Authored the `template.yaml` file using AWS SAM to declare all Lambda functions, API Gateway, and DynamoDB tables.<br>- Executed SAM build and deploy commands to automatically provision the entire EDMS architecture onto the AWS Cloud. | 01/08/2026 | 02/08/2026 |

### Week 6 Achievements:

* **Module Expansion & Business Automation:** Completed extended functional modules, successfully orchestrating complex enterprise workflows via Step Functions and ensuring users receive instant notifications through SNS.
* **Streamlined Deployment:** Replaced manual console configurations with AWS SAM. The entire project can now be spun up or torn down in minutes using code, drastically reducing operational overhead and garbage collection costs.
