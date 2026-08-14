---
title : "Introduction"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

### 5.1.1 What you will build

**EDMS** is a document collaboration platform where users can upload documents, organize them in folders, manage versions, share them with controlled permissions, and submit them for an approval workflow. It is fully serverless: you only pay for what you use, it scales automatically, and there is no server to manage.

The system has three account roles:

- **ADMIN** — manages users, departments, and the whole system.
- **MANAGER** — approves / rejects documents.
- **USER** — creates, uploads, edits, and submits documents.

### 5.1.2 Architecture

The following diagram shows the architecture of the platform we will build:

{{% mermaid %}}
flowchart LR
    subgraph Client["Client"]
        U[User / Browser]
        A[Amplify - React Frontend]
    end

    subgraph AWS["AWS Cloud"]
        G[API Gateway]
        L[Lambda - Spring Boot]
        DB[(Aurora MySQL)]
        S3[(S3 - Documents)]
        C[Cognito - Auth]
        SF[Step Functions - Approval]
        SNS[SNS - Notification]
        CW[CloudWatch]
    end

    U -->|sign-in| C
    U --> A
    A -->|HTTPS + JWT| G
    G --> L
    L -->|validate token| C
    L <-->|read / write| DB
    L <-->|store / get files| S3
    L -->|submit for approval| SF
    SF -->|notify| SNS
    L -.->|logs / metrics| CW
{{% /mermaid %}}

The system is composed of the following services:

| Layer | AWS Service | Responsibility |
|-------|-------------|----------------|
| Frontend | **AWS Amplify** | Hosts the React SPA over HTTPS |
| API | **Amazon API Gateway** | REST endpoint that routes requests to Lambda |
| Compute | **AWS Lambda** | Runs the Spring Boot (Java 17) backend monolith |
| Database | **Amazon Aurora MySQL** | Stores all relational metadata |
| Storage | **Amazon S3** | Stores the original document files |
| Auth | **Amazon Cognito** | Sign-in and role-based authorization |
| Workflow | **AWS Step Functions** | Orchestrates the document approval flow |
| Notification | **Amazon SNS** | Sends email notifications on approve / reject |
| Observability | **Amazon CloudWatch** | Logs and metrics |
| CI/CD | **GitHub Actions + AWS SAM** | Build, test, and deploy infrastructure as code |

### 5.1.3 How the services work together

1. A user signs in with **Cognito**, receives a **JWT token**.
2. The React frontend calls **API Gateway** with the token.
3. **API Gateway** forwards the request to **Lambda**, which validates the token.
4. **Lambda** reads / writes metadata in **Aurora** and stores files in **S3**.
5. When a document is submitted for approval, **Lambda** starts a **Step Functions** execution.
6. **Step Functions** orchestrates the approval and publishes notifications via **SNS** (email).

> **Note:** In the next sections we will build each service one by one, following the same order.
