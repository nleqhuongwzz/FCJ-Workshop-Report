---
title : "Verify CI/CD Pipeline"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.4.9 </b> "
---

After pushing the code and configuring the workflow, verify that the pipeline runs and deploys successfully. This confirms the OIDC role, the secrets, and the SAM template are all wired up correctly.

### 5.4.9.1 Open the workflow runs

1. In your repository, open the **Actions** tab.
2. You should see the `EDMS CI/CD` workflow listed. If there are runs, click the most recent one.

![Figure 25. Workflow runs](/images/5-Workshop/5.4-Edms-deployment/workflow-runs.png)

> **Note:** If no run appears, make sure the `deploy.yml` file was committed and pushed to `main`. The workflow only triggers on pushes to that branch (5.4.2).

### 5.4.9.2 Verify each job

Click into the latest run and confirm all three jobs succeed:

+ `test-backend` — ✅ runs `mvn test`.
+ `build-frontend` — ✅ runs `npm ci && npm run build`.
+ `deploy` — ✅ authenticates via OIDC and runs `sam deploy`.

![Figure 26. Jobs pass](/images/5-Workshop/5.4-Edms-deployment/jobs-pass.png)

If you configured **required reviewers** in 5.4.6, the `deploy` job will show a **Review deployments** button — approve it to let the job continue.

### 5.4.9.3 Verify the stack update

1. Open the **CloudFormation console** → **Stacks**.
2. Find `edms-lambda-stack` (the stack SAM deployed to).
3. Confirm its status is **UPDATE_COMPLETE** (or **CREATE_COMPLETE** on the first run).

![Figure 27. Stack updated](/images/5-Workshop/5.4-Edms-deployment/stack-updated.png)

4. Open the **Resources** tab and confirm the Lambda function and any other resources (API Gateway, Step Functions) were created.

> **Note:** If a job fails, click it to view the logs and fix the issue (often a missing secret, an OIDC trust-policy mismatch, or a SAM template error), then push again to retrigger the pipeline.
