---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Serverless Document Management with EDMS - Document Platform

### Overview

This workshop guides you through the entire process of building, developing, and operating **EDMS (Enterprise Document Collaboration Platform)** - a document management and collaboration application - on the Amazon Web Services (AWS) platform. The project employs a modern Cloud & DevOps model, featuring a fully automated CI/CD pipeline and a comprehensive serverless architecture.

The workshop is divided into key stages:

- **Infrastructure**: Setting up the AWS service foundation (S3, Aurora, IAM, Cognito)
- **Deployment**: Building a CI/CD pipeline using GitHub Actions and deploying the application to Lambda + API Gateway
- **Operations**: Configuring auto-scaling, monitoring, cost alerts, and end-to-end testing
- **Illustrations**: Reference list of all screenshots used in the workshop

### Architecture Summary

The system is organized into the following main layers:

| Layer | Components |
|-------|------------|
| CI/CD | GitHub Actions, OIDC, AWS STS, AWS SAM / CloudFormation |
| Presentation | AWS Amplify (React Frontend) |
| Application | Amazon API Gateway, AWS Lambda (Spring Boot) |
| Data | Amazon Aurora MySQL, Amazon S3 |
| Workflow | AWS Step Functions, Amazon SNS |
| Monitoring | Amazon CloudWatch, AWS Budgets |

### Contents

1. [Introduction](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequisite/)
3. [Design and Build EDMS Infrastructure on AWS](5.3-Edms-infrastructure/)
4. [Deploying EDMS on AWS](5.4-Edms-deployment/)
5. [Testing, Operations, and Continuous Deployment](5.5-Edms-operations/)
