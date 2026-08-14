---
title: "Proposal"
date: 2026-08-14
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Enterprise Document Collaboration Platform (EDMS)

## Serverless Document Management on the AWS Platform

### 1. Project Overview

EDMS (Enterprise Document Management System) is a cloud-native document collaboration platform that allows enterprises to store, version, share, and approve documents through a central, secure, web-based system. The system includes features such as role-based access control (ADMIN / MANAGER / USER), document and folder management, versioning, sharing with controlled permissions, tags, search, and an automated approval workflow with email notifications. Because the system must serve many users across departments while keeping operational cost low, it requires a flexible, scalable, highly available, and easy-to-maintain infrastructure.

This proposal presents a solution for deploying the EDMS system on the Amazon Web Services (AWS) platform using a fully **serverless** architecture that meets requirements for scalability, high availability, security, and automated release processes. The objective is to build a reusable serverless infrastructure that supports iterative deployments and standardizes operational procedures according to DevOps practices in a production environment.

The proposal focuses on building an AWS serverless architecture with **AWS Lambda** and **Amazon API Gateway** for compute, **Amazon Aurora MySQL** for the database, **Amazon S3** for file storage, **Amazon Cognito** for authentication, **AWS Step Functions** for the approval workflow, **Amazon SNS** for notifications, and **AWS Amplify** for frontend hosting. Source code is managed on GitHub, with automated Build–Test–Deploy workflows through GitHub Actions and OpenID Connect (OIDC), and infrastructure provisioned by AWS SAM / CloudFormation. The solution aims to establish a unified, secure, and scalable deployment workflow for the project.

---

### 2. Problem Statement

#### Current Status

Prior to implementing the proposal, the enterprise document management workflow was fragmented across emails, personal Google Drive, and on-premise file servers. Specifically:

- **Lack of centralized control:** There was no control over who could access which document, and no audit history of actions.
- **No approval process:** Documents could be published without an approval gate before becoming official.
- **Fixed infrastructure cost:** On-premise file servers incurred fixed costs regardless of actual usage.
- **Manual, non-automated deployment:** The application was not standardized into a cloud-native, automated deployment pipeline.

#### Objectives

The proposal aims for the following technical objectives:

- Provide centralized, role-based access control for documents.
- Automate the document approval workflow (submit → approve/reject → notify).
- Eliminate the use of AWS Access Keys in GitHub via OpenID Connect (OIDC).
- Standardize the deployment process using infrastructure as code (AWS SAM).
- Ensure high availability and automatic scaling of the system.
- Establish centralized monitoring, logging, and alerting mechanisms.
- Follow the DevOps model and improve reusability.

#### Solution

- Design the AWS serverless architecture.
- Build the CI/CD pipeline with GitHub Actions + SAM.
- Deploy the Spring Boot backend as a single Lambda behind API Gateway.
- Store documents in Amazon S3 and metadata in Aurora MySQL.
- Provide authentication and role-based access with Amazon Cognito.
- Orchestrate approval with AWS Step Functions and send email via SNS.
- Host the React frontend on AWS Amplify.
- Build logging and monitoring systems with CloudWatch.

#### Return on Investment (ROI)

System standardization and automation deliver practical value:

- **Cost Efficiency:** The serverless model ensures payment only for actual resources used, with idle cost near zero.
- **Time-to-Market:** Automated CI/CD pipelines reduce the time required to release new features.
- **High Availability:** Automatically scaling, managed services achieve high uptime and minimize downtime.
- **Security and Better Control:** AWS security standards combined with role-based access control and monitoring protect data and proactively detect vulnerabilities.

---

### 3. Solution Architecture

#### Overall Architecture

![EDMS System Architecture](../images/2-Proposal/edms_architecture.png)

The deployment architecture is fully serverless:

- **Frontend hosting:** The React SPA is hosted on AWS Amplify and served over HTTPS.
- **API processing:** Amplify forwards requests to Amazon API Gateway, which routes them to a single AWS Lambda running the Spring Boot backend.
- **Authentication:** Amazon Cognito issues JWT tokens and provides role-based authorization (ADMIN / MANAGER / USER).
- **Database:** Amazon Aurora MySQL stores all relational metadata (users, documents, versions, folders, permissions, tags, shares, approval history).
- **Storage:** Amazon S3 stores the original document files, accessed privately via pre-signed URLs.
- **Approval workflow:** AWS Step Functions orchestrates the document approval flow using the Wait for Task Token pattern; Amazon SNS sends email notifications.
- **CI/CD:** Source code is pushed to GitHub. GitHub Actions uses OIDC to authenticate to AWS STS and runs `sam deploy`.
- **Security and infrastructure:** AWS IAM manages access permissions; AWS SAM / CloudFormation standardizes infrastructure provisioning.
- **System observability:** Amazon CloudWatch collects logs and metrics.

#### Architecture Components

| AWS Service | Service Type | Role & Function in the System |
| ----------- | ------------ | ----------------------------- |
| **AWS IAM** | Identity & Access Management | Manages users, groups, roles, and security policies; used for the Lambda execution role and the OIDC deploy role. |
| **AWS Lambda** | Serverless Compute | Runs the Spring Boot (Java 17) backend monolith. |
| **Amazon API Gateway** | API Gateway | Exposes the backend as a REST API and routes requests to Lambda. |
| **Amazon Cognito** | Authentication | Provides sign-in and role-based authorization via JWT. |
| **Amazon Aurora** | Relational Database | Stores relational metadata (MySQL-compatible). |
| **Amazon S3** | Object Storage | Stores original document files, accessed via pre-signed URLs. |
| **AWS Step Functions** | Workflow Orchestration | Orchestrates the document approval workflow (waitForTaskToken). |
| **Amazon SNS** | Notification Service | Sends email notifications on approve/reject. |
| **AWS Amplify** | Frontend Hosting | Hosts the React frontend over HTTPS. |
| **Amazon CloudWatch** | Monitoring & Observability | Collects logs and metrics, and configures dashboards and alarms. |

#### AWS Well-Architected Framework

| Pillar | Applied Solution |
| ------ | ---------------- |
| Operational Excellence | GitHub Actions CI/CD, AWS SAM / CloudFormation, CloudWatch. |
| Security | IAM Least Privilege, Cognito auth, private S3 bucket, no AWS keys in GitHub (OIDC). |
| Reliability | Serverless managed services, Step Functions retries, CloudWatch monitoring. |
| Performance Efficiency | Lambda + API Gateway auto scaling, S3 + pre-signed URLs. |
| Cost Optimization | Pay-as-you-go serverless, stop/delete Aurora when idle. |
| Sustainability | Scale on demand; only pay for actual usage. |

---

### 4. Timeline & Milestones

| Phase | Duration | Main Tasks |
| ----- | -------- | ---------- |
| **Week 1: Research & Design** | 22/06/2026 - 26/06/2026 | - Explore AWS Foundations (Global Infrastructure, IAM, EC2, S3). <br> - Design the system architecture and data flow. |
| **Week 2: Storage & Security** | 29/06/2026 - 03/07/2026 | - Learn Amazon S3, IAM, and Git. <br> - Practice S3 + IAM + Git. |
| **Week 3: Database & Design** | 06/07/2026 - 10/07/2026 | - Learn Aurora MySQL and design the EDMS data model. <br> - Create S3, Aurora, IAM, Cognito. |
| **Week 4: Backend Development** | 13/07/2026 - 17/07/2026 | - Set up Spring Boot backend. <br> - Implement Cognito auth + JWT. <br> - Implement document and folder CRUD. |
| **Week 5: Backend Advanced** | 20/07/2026 - 24/07/2026 | - Implement permissions, versioning, tags, search, sharing, dashboard. <br> - Write unit tests. |
| **Week 6: Approval Workflow** | 27/07/2026 - 31/07/2026 | - Learn Step Functions (waitForTaskToken). <br> - Create SNS topic. <br> - Build the approval state machine. |
| **Week 7: CI/CD & Deploy** | 03/08/2026 - 07/08/2026 | - Package backend as Lambda (SAM). <br> - Configure OIDC + GitHub secrets. <br> - Write GitHub Actions workflow and deploy. |
| **Week 8: Hosting & Go-Live** | 10/08/2026 - 15/08/2026 | - Host frontend on Amplify. <br> - Run end-to-end tests. <br> - Finalize the report and demo. |

---

### 5. Estimated Budget

The system makes maximum use of the **AWS Free Tier** and **Serverless Pay-As-You-Go** model, paying only for the resources actually used.

| AWS Service | Estimated Usage / Phase | Estimated Cost (USD) |
| ----------- | ----------------------- | -------------------- |
| **AWS Lambda** | Spring Boot monolith, invoked via API Gateway | **~$0 - $5** |
| **Amazon API Gateway** | REST API requests | **~$0 - $1** |
| **Amazon Aurora MySQL** | Relational metadata database | **~$5 - $15** (main cost driver) |
| **Amazon S3** | Document storage + pre-signed URLs | **~$1 - $3** |
| **Amazon Cognito** | User pool (free tier) | **~$0** |
| **AWS Step Functions** | Approval workflow executions | **~$0 - $2** |
| **Amazon SNS** | Email notifications (free tier) | **~$0** |
| **AWS Amplify** | Frontend hosting | **~$1 - $3** |
| **Amazon CloudWatch** | Logs and metrics | **~$1 - $3** |
| **Estimated total per month** | | **~$8 - $30** |

In addition, the proposal applies cost optimization measures such as:

- Configuring **AWS Budgets** and SNS alerts at 50%, 80%, and 100% of the monthly budget.
- Stopping or deleting **Aurora** when not in use (the main cost driver).
- Using the AWS Free Tier where possible.
- Deleting or stopping unused resources in the staging environment after testing.

---

### 6. Risk Assessment

#### Risk Matrix

| Risk | Likelihood | Impact |
| ---- | ---------- | ------ |
| AWS costs exceed forecast (mainly Aurora) | Medium | Medium |
| Lambda cold start latency | Medium | Low |
| Approval workflow failure | Low | Medium |
| Sensitive information exposure | Low | Very High |
| Sudden traffic spike | Medium | Low |
| Insufficient logs or alerts | Medium | Medium |
| Error during new version deployment | Medium | Medium |

#### Contingency and Response Plan

- Address cost alerts immediately upon reaching budget thresholds; stop or delete Aurora when not in use.
- When API errors occur, check CloudWatch Logs and Step Functions executions before rolling back or deploying a fix.
- Upon detecting signs of credential exposure, revoke or rotate the secret, review IAM permissions, and audit the deployment history.
- Use Step Functions retries and CloudWatch alarms to handle workflow and traffic issues.

---

### 7. Expected Outcomes

After completing the deployment process, the system is expected to achieve the following results:

- **Technical improvement:** Replacing manual document handling and scattered storage with a centralized, secure, serverless document platform that can be monitored, scaled, and automatically deployed on AWS, including an automated approval workflow.
- **Long-term value:** Establishing a reusable serverless architecture and infrastructure as code, laying the groundwork for expanding features such as advanced analytics, OCR, and integrations with other enterprise systems in the future.

---

### 8. References

[1]: [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
[2]: [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)
[3]: [AWS Documentation](https://docs.aws.amazon.com/)
