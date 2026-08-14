---
title : "Design and Build EDMS Infrastructure on AWS"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

In this section, you will design and build the **core infrastructure** of EDMS on AWS: storage, database, IAM, and authentication.

#### Deployment architecture

The EDMS infrastructure follows a layered, serverless model:

- **Amazon S3** — stores the original document files
- **Amazon Aurora (MySQL)** — hosts the relational metadata (users, documents, permissions, approval history)
- **IAM Role + OIDC** — grants secure deploy permissions to GitHub Actions and the Lambda execution role
- **Amazon Cognito** — provides authentication and role-based authorization

#### Content

1. [Initialize and Configure S3](5.3.1-set-up-s3/)
2. [Initialize and Configure Aurora RDS](5.3.2-set-up-rds/)
3. [Initialize and Configure IAM](5.3.3-set-up-iam/)
4. [Initialize and Configure Cognito](5.3.4-set-up-cognito/)
