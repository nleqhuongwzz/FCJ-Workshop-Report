---
title: "Worklog Tuần 7"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 7 Objectives:
* Establish a fully automated CI/CD pipeline using GitHub Actions.
* Implement image upload functionality to S3 with metadata storage in the Aurora database.
* Deploy advanced security layers (AWS WAF, Secrets Manager) and review IAM policies.

### Tasks to be carried out this week:

| Day | Task Details | Start Date | Completion Date |
| :--- | :--- | :--- | :--- |
| **1** | **CI/CD Pipeline Setup (GitHub Actions)**<br>- Configured automated testing and infrastructure deployment workflows upon new code merges. | 03/08/2026 | 04/08/2026 |
| **2** | **Database Security (Secrets Manager)**<br>- Integrated AWS Secrets Manager to securely store and auto-rotate Aurora database credentials. | 05/08/2026 | 05/08/2026 |
| **3** | **Edge Protection (AWS WAF) & IAM Review**<br>- Configured AWS WAF to block spam bots and enforce API rate-limiting.<br>- Conducted a comprehensive review of all AWS IAM policies to ensure the Least Privilege principle. | 06/08/2026 | 07/08/2026 |
| **4** | **S3 Image Upload & Aurora Storage**<br>- Developed APIs to handle image uploads to Amazon S3.<br>- Configured Lambda functions to record file metadata and paths into the Aurora Serverless database. | 08/08/2026 | 08/08/2026 |
| **5** | **Overall Testing & Review**<br>- Performed comprehensive system testing, reviewed error logs on CloudWatch, and finalized the weekly report. | 09/08/2026 | 09/08/2026 |

### Week 7 Achievements:

* **Process Automation:** Successfully built a CI/CD pipeline to automate the build and deployment process, optimizing development and operational time.
* **Storage Feature Completion:** Successfully implemented the image upload workflow to S3 combined with secure metadata storage in the Aurora database.
* **Enterprise Security:** Comprehensively reinforced system security using AWS Secrets Manager for sensitive information management and AWS WAF to protect the application from malicious traffic.
