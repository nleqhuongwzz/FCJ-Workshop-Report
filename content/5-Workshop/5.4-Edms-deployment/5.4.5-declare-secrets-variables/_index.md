---
title : "Declare Secrets and Variables on GitHub"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

The CI/CD pipeline needs configuration values (ARNs, IDs, credentials). Store them as **GitHub Secrets** so they are never committed to the repository.

### 5.4.5.1 Required secrets

The workflow in 5.4.2 reads these secrets at deploy time. Add them under **GitHub** → **Settings** → **Secrets and variables** → **Actions**:

| Secret | Value |
|--------|-------|
| `AWS_DEPLOY_ROLE_ARN` | The deploy role ARN from 5.4.4 |
| `COGNITO_USER_POOL_ID` | The Cognito User Pool ID from 5.3.4 |
| `COGNITO_CLIENT_ID` | The Cognito Client ID from 5.3.4 |
| `AURORA_ENDPOINT` | The Aurora endpoint from 5.3.2 |
| `DB_USER_AWS` | The Aurora master username |
| `DB_PASS_AWS` | The Aurora master password |
| `AWS_S3_BUCKET` | The S3 bucket name from 5.3.1 |
| `SNS_TOPIC_ARN` | The SNS topic ARN (created in 5.4.8) |
| `BACKEND_LAMBDA_ARN` | The Lambda function ARN (created after deploy in 5.4.9) |

![Figure 19. GitHub secrets](/images/5-Workshop/5.4-Edms-deployment/secrets.png)

### 5.4.5.2 Add a repository secret

1. Open your repository on GitHub → **Settings** → **Secrets and variables** → **Actions**.
2. Click **New repository secret**.
3. Enter the **Name** (e.g. `AWS_DEPLOY_ROLE_ARN`) and the **Value**.
4. Click **Add secret**.

Repeat for each row in the table above.

> **Note:** The last two secrets (`SNS_TOPIC_ARN`, `BACKEND_LAMBDA_ARN`) depend on resources you create later. You can add the others now and the last two after 5.4.8 and 5.4.9.

### 5.4.5.3 Verify a secret was stored

1. After adding, the secret appears in the **Actions secrets and variables** list, but its value is **masked**.
2. You cannot read it back — this is by design. To change it, delete and recreate it.

> **Best practice:** Never commit secrets or the `.env` file. All secrets are injected at deploy time by the workflow, and GitHub automatically masks their values in run logs.
