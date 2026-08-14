---
title: "Worklog Week 6"
date: 2026-08-07
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:
- Develop and finalize advanced business modules (Approval, Link Sharing, Trash Management).
- Automate the document approval workflow using AWS Step Functions combined with Amazon SNS for email notifications.
- Package and deploy the entire Serverless system using Infrastructure as Code via AWS SAM.

### Tasks to be carried out this week:
| Day | Detailed Tasks | Start Date | Completion Date |
| :---: | :--- | :---: | :---: |
| 1 | Develop Approval Module & Workflow (Step Functions)<br>- Built the DocumentApprovalStateMachine State Machine on AWS Step Functions following the human approval pattern with waitForTaskToken.<br>- Programmed the submit flow, token capture, choice (approved/rejected), mark status, and notify logic. | 27/07/2026 | 27/07/2026 |
| 2 | Event Notification (Amazon SNS)<br>- Integrated the Amazon SNS topic edms-notifications to trigger automated email notifications upon document approval completion (approve/reject). | 28/07/2026 | 28/07/2026 |
| 3 | Secure Document Sharing & Lifecycle Management<br>- Programmed secure time-limited link sharing functionality for documents.<br>- Implemented auxiliary features such as search, OCR text extraction, and folder/department category management. | 29/07/2026 | 30/07/2026 |
| 4 | Statistical Dashboard & Audit Log<br>- Developed a department-wise statistical dashboard module to support enterprise management.<br>- Built an audit logging system for operation tracking via CloudWatch. | 31/07/2026 | 31/07/2026 |
| 5 | Infrastructure as Code & Cloud Deployment (AWS SAM)<br>- Configured the `template.yaml` file using AWS SAM to declare all resources (Lambda, API Gateway, Aurora, S3, Cognito, Step Functions, SNS).<br>- Executed build and deploy commands to provision the infrastructure stack onto AWS Cloud. | 01/08/2026 | 02/08/2026 |

### Week 6 Achievements:
- Successfully operated the asynchronous approval workflow via Step Functions and SNS, handling human-in-the-loop tasks without hitting Lambda's 15-minute timeout limit.
- Defined the entire infrastructure as code, enabling rapid and synchronized provisioning or management of the complete EDMS system on AWS.