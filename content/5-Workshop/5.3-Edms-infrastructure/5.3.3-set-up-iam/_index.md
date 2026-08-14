---
title : "Initialize and Configure IAM"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

IAM provides the identities and permissions EDMS needs to run securely. Two roles are required:

1. **`github-actions-deploy-role`** — an **OIDC** (Web identity) role that GitHub Actions assumes at deploy time, so no long-term AWS keys are stored in the repository.
2. **`edms-lambda-role`** — the execution role used by the Lambda functions (S3, Cognito, SNS, Step Functions, CloudWatch Logs).

### 5.3.3.1 Create the OIDC provider

To let GitHub Actions assume a role instead of holding permanent AWS keys, register GitHub's OIDC provider with IAM:

1. Open the **IAM console** → **Identity providers** → **Add provider**.


2. **Provider type:** select **OpenID Connect**.
3. **Provider URL:** enter `https://token.actions.githubusercontent.com`.
4. IAM will show **Get thumbprint** — click it to fetch the certificate thumbprint.
5. **Audience:** enter `sts.amazonaws.com`.
6. Click **Add provider**.
![Figure 8. Add OIDC provider](/images/5-Workshop/5.3-Edms-infrastructure/oidc-provider.png)

> **Note:** This step is done only once per AWS account. The provider URL must match exactly `https://token.actions.githubusercontent.com`.

### 5.3.3.2 Create the GitHub Actions deploy role

1. Open **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** select **Web identity**.
3. **Identity provider:** choose the GitHub OIDC provider you just created.
4. **Audience:** select `sts.amazonaws.com`.
5. Click **Next**.
6. Add a trust policy that allows only your repository to assume the role (see 5.4 for the exact policy).
7. Under **Permissions**, attach a broad deploy policy covering CloudFormation, Lambda, S3, API Gateway, IAM, and other services EDMS deploys.
8. Name the role `github-actions-deploy-role`.
9. Click **Create role**.


> **Note:** Because the role is assumed via OIDC, the GitHub workflow uses `aws-actions/configure-aws-credentials` with `role-to-assume` — no access key ID or secret access key is ever committed.

### 5.3.3.3 Create the Lambda execution role

1. Open **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** select **AWS service** → **Lambda**.
3. Click **Next**.
4. Attach a custom or managed policy that allows at least:
+ `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, `s3:ListBucket`
+ `cognito-idp:InitiateAuth`, `cognito-idp:GetUser`
+ `sns:Publish`
+ `states:StartExecution`, `states:SendTaskSuccess`, `states:SendTaskFailure`
+ `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`
5. Name the role `edms-lambda-role`.
6. Click **Create role**.


> **Best practice:** Grant only the permissions each role needs (**least-privilege**). Never put AWS keys in code or configuration files.

### 5.3.3.4 Verify the roles

1. Open **IAM** → **Roles**.
2. Confirm both `github-actions-deploy-role` and `edms-lambda-role` are listed.
3. Open each role and check the **Trust relationships** and **Permissions** tabs match your setup.
4. Note the **role ARN** of the deploy role — the GitHub workflow and SAM template reference it.
