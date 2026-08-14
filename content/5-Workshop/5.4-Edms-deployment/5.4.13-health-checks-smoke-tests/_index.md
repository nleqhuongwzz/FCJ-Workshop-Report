---
title : "Health Checks and Smoke Tests Post-Deploy"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.4.13 </b> "
---

After deployment, run health checks and smoke tests to verify the API works end-to-end: from API Gateway through Lambda, into Aurora, and across the Step Functions approval workflow.

### 5.4.13.1 Test the health endpoint

1. Confirm the API is reachable with a simple health check:

```bash
curl https://<invoke-url>/health
```

2. Expect a `200 OK` with a JSON body from Spring Boot Actuator (e.g. `{"status":"UP"}`).

### 5.4.13.2 Test the login endpoint

1. Call the login endpoint with one of the Cognito users you created in 5.4.7:

```bash
curl -X POST https://<invoke-url>/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@edms.vn","password":"your-password"}'
```

2. A successful response returns a **token** and the **user's role** (e.g. `ADMIN`).


> **Note:** Use the actual temporary password you set in Cognito. If the account requires a password change, run the change-password flow first.

### 5.4.13.3 Smoke test the approval workflow

Now test the full serverless orchestration (Step Functions + SNS):

1. Using a **USER** account token, call `POST /approval/submit` with a `documentId`. This starts the state machine and leaves it waiting at `CaptureToken`.
2. Confirm the document becomes `PENDING`.
3. Using a **MANAGER** account token, call `POST /approval/approve` with the same `documentId`. The Lambda calls `SendTaskSuccess`, resuming the workflow.
4. Confirm the document becomes `APPROVED`.
5. Check your inbox for the **SNS email** `EDMS - Approval`.

![Figure 37. Approval smoke test](/images/5-Workshop/5.4-Edms-deployment/approval-test.png)

### 5.4.13.4 Check Step Functions execution

1. Open the **Step Functions console** → your state machine `DocumentApprovalStateMachine` → **Executions**.
2. You should see a **SUCCEEDED** execution for the document you approved.
3. Open it and verify the states visited: `CaptureToken` → `Decision` → `MarkApproved` → `NotifyApproved`.

![Figure 38. Execution succeeded](/images/5-Workshop/5.4-Edms-deployment/execution-succeeded.png)

> **Note:** The full ASL definition is in the repository (`backend/template.yaml`, resource `DocumentApprovalStateMachine`). It uses `waitForTaskToken` so the workflow can pause and wait for a manager's decision, then resume via `SendTaskSuccess`.
