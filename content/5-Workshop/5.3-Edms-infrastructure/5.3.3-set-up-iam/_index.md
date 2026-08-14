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

![Figure 8. Add OIDC provider](../../../images/5-Workshop/5.3-Edms-infrastructure/oidc-provider.png)

2. In the **Add identity provider** page:

**Provider type**
+ Select **OpenID Connect** (this establishes trust between your AWS account and an identity provider such as GitHub). Do **not** choose SAML.

**Provider details**
+ **Provider name:** enter `token.actions.githubusercontent.com` (this is the GitHub OIDC issuer URL). It must match exactly.
+ **Audience:** enter `sts.amazonaws.com`.
+ The **thumbprint** is fetched automatically by AWS when you add the provider — no need to paste it manually.

3. (Optional) Under **Add tags**, you can add a tag such as `Project=EDMS`.
4. Click **Add provider**.

> **Note:** This step is done only once per AWS account. The provider URL must match exactly `https://token.actions.githubusercontent.com`.

### 5.3.3.2 Create the GitHub Actions deploy role

Open **IAM** → **Roles** → **Create role**. The wizard has 3 steps:

#### Step 1: Select trusted entity

+ **Trusted entity type:** select **Web identity** — *"Allows users federated by the specified external web identity provider to assume this role to perform actions in this account."*

**Web identity configuration**

+ **Identity provider:** select `token.actions.githubusercontent.com` (the GitHub OIDC provider you created in 5.3.3.1). If it is not listed, click **Create new** and register it first.
+ **Audience:** select `sts.amazonaws.com`.
+ **GitHub organization (required):** enter your GitHub organization or username, e.g. `hminhquaan`. AWS uses this to scope the trust policy to that organization.
+ **GitHub repository (optional):** enter `Enterprise-Document-Collaboration-Platform` to restrict the role to that repo.
+ **GitHub branch (optional):** enter `main` to restrict to the main branch.
+ Click **Next**.

> **Note:** The **GitHub organization** field is **required**. These fields let AWS generate a scoped trust policy automatically. You can refine it further after the role is created (see 5.4).

#### Step 2: Add permissions

+ Attach a broad deploy policy covering CloudFormation, Lambda, S3, API Gateway, IAM, and other services EDMS deploys. For a workshop you can use **PowerUserAccess**, or a custom inline policy for least-privilege.

#### Step 3: Name, review, and create

+ **Role name:** `github-actions-deploy-role`.
+ Review the **trust policy** (it should allow the GitHub OIDC provider to assume the role for your repository — see 5.4 for the exact policy).
+ Click **Create role**.

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
