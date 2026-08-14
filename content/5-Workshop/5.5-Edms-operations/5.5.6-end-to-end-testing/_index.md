---
title : "End-to-End Testing"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6 </b> "
---

Perform an **end-to-end test** through the deployed web application to verify that the whole flow — login, document upload, submission, and approval — works across the real AWS services.

### 5.5.6.1 Open the application

1. Open the **Amplify** URL (the default domain, e.g. `https://main.d3xxxx.amplifyapp.com`).
2. You should see the EDMS **login page**, served over HTTPS.

![Figure 50. Login page](/images/5-Workshop/5.5-Edms-operations/login.png)

### 5.5.6.2 Sign in as a USER

1. Sign in with a **USER** account created in Cognito (section 5.4).
2. Confirm you reach the document dashboard.

![Figure 51. Sign in](/images/5-Workshop/5.5-Edms-operations/signin.png)

### 5.5.6.3 Create and upload a document

1. Click **Create / upload** a new document.
2. Enter a title and attach a file.
3. Save the document — its status should be **`DRAFT`**.

### 5.5.6.4 Submit for approval

1. Select the document and click **Submit**.
2. Confirm the status changes to **`PENDING`** (awaiting manager approval).
3. This submission triggers the **Step Functions** approval workflow.

### 5.5.6.5 Approve as a MANAGER

1. Sign out of the USER account.
2. Sign in with a **MANAGER** account created in Cognito.
3. Open the **pending approvals** list.
4. **Approve** the document — confirm the status changes to **`APPROVED`**.
5. Check your inbox for the **SNS email notification** confirming the approval.

![Figure 52. Document approved](/images/5-Workshop/5.5-Edms-operations/approved.png)

### 5.5.6.6 Verify the Step Functions execution

1. Open the **Step Functions console**.
2. Find the execution created for this document.
3. Confirm its status is **`SUCCEEDED`** and that each state (including the notification step) completed.

> **Success criteria:** The document moves through `DRAFT → PENDING → APPROVED`, the Step Functions execution **succeeds**, and an **email** is delivered. If any step fails, use the Logs Insights queries from section 5.5.5 to diagnose it.
