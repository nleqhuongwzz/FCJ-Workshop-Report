---
title : "Clean Up Resources"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7 </b> "
---

To avoid ongoing costs, delete every resource you created in this workshop. Follow the order below so that dependent resources are removed first — delete the backend stack first, then the manually-created services, and finally verify that nothing is left running.

### 5.5.7.1 Delete the SAM / CloudFormation stack

The Lambda, API Gateway, Step Functions, and IAM resources created by SAM are all removed by deleting the CloudFormation stack. There are two ways:

**Option A — AWS CLI (quickest):**

Open a terminal in the folder containing your `template.yaml` and run:

```bash
sam delete --stack-name edms-lambda-stack --no-prompts
```

This command:
+ Deletes the **CloudFormation stack** (`edms-lambda-stack`).
+ Deletes the **Lambda function** and its code.
+ Deletes the **API Gateway** REST API.
+ Deletes the **Step Functions** state machine.
+ Deletes the **IAM roles** created by SAM.
+ Deletes the **artifacts** (code bundles) stored in the SAM deployment bucket.

**Option B — AWS Console:**

1. Open the **CloudFormation console**.
2. Select the stack **`edms-lambda-stack`**.
3. Click **Delete**.
4. AWS asks you to confirm — click **Delete stack**.
5. Wait until the stack status changes from `DELETE_IN_PROGRESS` to **`DELETE_COMPLETE`**.

> **Note:** If the stack fails to delete (e.g. an S3 bucket is not empty), check the **Events** tab for the error, fix the blocking resource, and try again.

### 5.5.7.2 Delete remaining resources manually

The following services were created **outside SAM**, so you must delete them individually in the console. Do them in this order:

**1. Delete the S3 bucket (do this before Cognito/Aurora, since it stores files)**

1. Open the **S3 console**.
2. Select the `edms-docs-bucket-...` bucket.
3. Click **Empty** → type `permanently delete` → confirm. This removes all objects **and** any object versions (otherwise the bucket cannot be deleted).
4. Back in the bucket list, select the bucket → **Delete** → type the bucket name → confirm.

**2. Delete the Aurora cluster (the main source of cost)**

1. Open the **RDS console** → **Databases**.
2. Select the cluster **`edms-cluster`**.
3. Click **Actions** → **Delete**.
4. Uncheck **Create final snapshot** (unless you want a backup).
5. **Important:** check **"I acknowledge that I will lose..."** and **disable delete protection** if it is enabled (you turned it on in 5.3.2).
6. Click **Delete**. Wait for the status to become `DELETED`.

> **Note:** Aurora charges even when stopped, so it is important to **delete** (not just stop) the cluster when you are finished.

**3. Delete the Cognito User Pool**

1. Open the **Cognito console** → **User pools**.
2. Select **`edms-user-pool`**.
3. Click **Delete**.
4. Confirm — this removes the pool, the app client, and all users.

**4. Delete the Step Functions state machine**

1. Open the **Step Functions console** → **State machines**.
2. Select **`DocumentApprovalStateMachine`**.
3. Click **Delete** → confirm. (Any running executions are aborted.)

**5. Delete the SNS topic**

1. Open the **SNS console** → **Topics**.
2. Select **`edms-notifications`**.
3. Click **Delete** → confirm. This also removes its subscriptions (the email subscription).

**6. Delete the AWS Amplify app (the frontend)**

1. Open the **Amplify console** → **All apps**.
2. Select the app.
3. Click **Actions** → **Delete app**.
4. Type `delete` to confirm. This removes the deployed frontend and the Amplify domain URL.

**7. Delete the IAM roles**

1. Open the **IAM console** → **Roles**.
2. Delete **`github-actions-deploy-role`** and **`edms-lambda-role`** (after the CloudFormation stack is gone, they are no longer needed).
3. Also delete the **OIDC provider** (`token.actions.githubusercontent.com`) in **IAM → Identity providers** if you no longer need it.

### 5.5.7.3 Verify zero cost

1. Open **Billing** → **Cost Explorer**.
2. Confirm no service is still incurring charges for your account.
3. (Optional) Set a **$0 budget** alert so you are notified by email if anything still generates cost.

> **Best practice:** After cleanup, confirm the **CloudFormation stack list**, **Amplify apps**, and **RDS databases** listings are all empty so you do not leave a running bill. Budget data lags up to 24 hours, so re-check the next day.
