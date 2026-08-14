---
title : "Configure GitHub Actions Workflow"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

The CI/CD pipeline is defined in `.github/workflows/deploy.yml`. It runs on every push to the `main` branch and deploys EDMS to AWS using **OIDC** — no long-term access keys are stored in GitHub.

### 5.4.2.1 Workflow overview

The workflow is made of three jobs that run in sequence (the deploy job waits for the first two):

1. `test-backend` — runs `mvn test` to validate the Java unit tests.
2. `build-frontend` — runs `npm ci && npm run build` to compile the React SPA.
3. `deploy` — authenticates to AWS via OIDC and runs `sam deploy`.

```
push to main
   → test-backend
   → build-frontend
   → deploy (OIDC + SAM)   [waits for both]
```

### 5.4.2.2 The deploy.yml file

Create `.github/workflows/deploy.yml` with this content:

```yaml
name: EDMS CI/CD

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  id-token: write   # needed for OIDC
  contents: read

jobs:
  test-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: corretto
          java-version: "17"
      - run: mvn -B test
        working-directory: backend

  build-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "18"
      - run: npm ci && npm run build
        working-directory: frontend

  deploy:
    needs: [test-backend, build-frontend]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure AWS credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
          aws-region: ap-southeast-1
      - run: mvn -B clean package -DskipTests
        working-directory: backend
      - uses: aws-actions/setup-sam@v2
      - name: Deploy via SAM
        working-directory: backend
        run: |
          sam deploy --stack-name edms-lambda-stack \
            --no-confirm-changeset --no-fail-on-empty-changeset \
            --capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND \
            --parameter-overrides "CognitoUserPoolId=${{ secrets.COGNITO_USER_POOL_ID }} CognitoClientId=${{ secrets.COGNITO_CLIENT_ID }} AuroraEndpoint=${{ secrets.AURORA_ENDPOINT }} S3BucketName=${{ secrets.AWS_S3_BUCKET }} DbUserName=${{ secrets.DB_USER_AWS }} DbUserPass=${{ secrets.DB_PASS_AWS }} SnsTopicArn=${{ secrets.SNS_TOPIC_ARN }} BackendLambdaArn=${{ secrets.BACKEND_LAMBDA_ARN }}"
```

### 5.4.2.3 How OIDC authentication works

The deploy job does **not** receive static AWS credentials:

1. The runner requests an OIDC **ID token** from GitHub (this is why `permissions.id-token: write` is required).
2. `configure-aws-credentials` calls `sts:AssumeRoleWithWebIdentity` with that token, assuming the role in `AWS_DEPLOY_ROLE_ARN`.
3. AWS verifies the token's `sub` claim against the trust policy in the IAM role.
4. Short-lived, scoped credentials are returned and used only for the duration of the job.

> **Key point:** Because the role is assumed via OIDC, no secret access key is ever stored in GitHub — a security best practice.

### 5.4.2.4 The parameter overrides

The `--parameter-overrides` list passes environment-specific values into the SAM template. Each value is read from a GitHub **secret** at deploy time:

+ `CognitoUserPoolId`, `CognitoClientId` — authentication (from 5.3.4).
+ `AuroraEndpoint`, `DbUserName`, `DbUserPass` — Aurora database connection (from 5.3.2).
+ `S3BucketName` — file storage bucket (from 5.3.1).
+ `SnsTopicArn`, `BackendLambdaArn` — created later in 5.4.8 / 5.4.12.

> **Note:** All of these secrets are declared in section 5.4.5. If a secret is missing, the SAM deploy step will fail with a substitution error.
