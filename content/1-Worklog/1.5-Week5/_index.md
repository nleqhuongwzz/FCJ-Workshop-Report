---
title: "Worklog Tuần 5"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:
* Design and implement a Polyglot Persistence database architecture using Amazon Aurora and DynamoDB.
* Configure secure object storage for physical files and set up centralized user authentication.
* Develop core document management APIs (CRUD) using Java 17 and AWS Lambda.
* Implement Versioning Control, Role-Based Access Control (RBAC), and optimize performance.

### Tasks to be carried out this week:

| Day | Task Details | Start Date | Completion Date |
| :--- | :--- | :--- | :--- |
| **1** | **Database Schema Design**<br>- Designed the Polyglot Persistence schema: Aurora Serverless v2 (MySQL) for relational data (Documents, Versions, Tags) and DynamoDB for AuditLogs. | 19/07/2026 | 19/07/2026 |
| **2** | **Identity Management with Cognito & S3 Storage**<br>- Integrated Amazon Cognito User Pool to handle login, authentication, and user grouping (e.g., HR, SALES) for role-based access control.<br>- Set up an Amazon S3 bucket for physical file storage using Presigned URLs with a short expiration window (5-10 minutes) to prevent bandwidth abuse. | 20/07/2026 | 20/07/2026 |
| **3** | **Core API Development & Lambda Layers**<br>- Programmed Lambda functions using Java 17 (Amazon Corretto) to handle document creation and retrieval.<br>- Extracted shared libraries into AWS Lambda Layers to reduce deployment package size. | 21/07/2026 | 21/07/2026 |
| **4** | **Versioning Control & RBAC**<br>- Implemented version control logic to automatically generate a new version number upon editing, along with a Rollback feature for restoration.<br>- Finalized the Permissions module to enforce strict access boundaries for Owners, Editors, and Viewers directly at the API level. | 22/07/2026 | 22/07/2026 |
| **5** | **Cold Start Mitigation (SnapStart)**<br>- Resolved Java 17 cold start delays (1-3s) by enabling AWS Lambda SnapStart, taking pre-initialized JVM snapshots to accelerate response times. | 23/07/2026 | 23/07/2026 |

### Week 5 Achievements:

* **Scalable Database Architecture & Security:** Successfully decoupled the write-heavy audit log stream to DynamoDB to resolve performance bottlenecks, while establishing a robust security perimeter combining Cognito and S3 Presigned URLs.
* **Robust Backend Logic & High Performance:** Successfully deployed a fully functional, secure, and version-controlled backend API using Java 17, while dramatically improving response times by leveraging Lambda Layers and SnapStart.
