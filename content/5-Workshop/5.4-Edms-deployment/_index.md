---
title : "Deploying EDMS on AWS"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

In this section, you will deploy EDMS to AWS using **infrastructure as code** (AWS SAM + CloudFormation) and a **GitHub Actions** CI/CD pipeline authenticated via OIDC.

#### Content

1. [Prepare Source Code and Project Structure](5.4.1-prepare-project-structure/)
2. [Configure GitHub Actions Workflow](5.4.2-github-actions-workflow/)
3. [Create IAM Stack via CloudFormation](5.4.3-cloudformation/)
4. [Verify Stack Results and Retrieve Outputs](5.4.4-check-stack-results/)
5. [Declare Secrets and Variables on GitHub](5.4.5-declare-secrets-variables/)
6. [Create GitHub Environment 'production'](5.4.6-create-github-environment/)
7. [Create Cognito User Pool](5.4.7-create-cognito/)
8. [Create Step Functions State Machine](5.4.8-create-stepfunctions/)
9. [Verify CI/CD Pipeline](5.4.9-verify-cicd-pipeline/)
10. [Trigger Pipeline](5.4.10-trigger-pipeline/)
11. [Create API Gateway](5.4.11-create-api-gateway/)
12. [Link Lambda with API Gateway](5.4.12-link-lambda-apigateway/)
13. [Health Checks and Smoke Tests Post-Deploy](5.4.13-health-checks-smoke-tests/)
