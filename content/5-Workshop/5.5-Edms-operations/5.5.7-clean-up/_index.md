---
title : "Clean Up Resources"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7 </b> "
---

To avoid ongoing costs, delete every resource you created in this workshop. Follow the order below so that dependent resources are removed first.

### 5.5.7.1 Delete the SAM / CloudFormation stack

The Lambda, API Gateway, Step Functions, and IAM resources created by SAM can be removed with one command. Run it from the folder containing your `template.yaml`:

```bash
sam delete --stack-name edms-lambda-stack --no-prompts
```

This command also deletes the **CloudFormation stack** and the artifacts (Lambda layers, code bundles) stored in the deployment bucket it created.

> **Note:** You can achieve the same result in the console by opening **CloudFormation** → select the stack → **Delete**. Confirm the stack status changes to `DELETE_COMPLETE`.

![Figure 53. Delete stack](/images/5-Workshop/5.5-Edms-operations/delete-stack.png)

### 5.5.7.2 Delete remaining resources manually

The following were created outside SAM, so delete them in the console:

1. **Amplify** — delete the app from the Amplify console.
2. **Step Functions** — delete the state machine.
3. **SNS** — delete the `edms-notifications` topic (and any subscriptions).
4. **Cognito** — delete the User Pool (and the app client).
5. **S3** — **empty** and delete the buckets, including any **object versions** and lifecycle policies.
6. **Aurora** — **delete the cluster**; this is the main source of cost, so do not skip it.
7. **IAM** — delete the deploy role, after removing any CloudFormation stack that still references it.

![Figure 54. Delete resources](/images/5-Workshop/5.5-Edms-operations/delete-resources.png)

### 5.5.7.3 Verify zero cost

1. Open **Billing** → **Cost Explorer**.
2. Confirm no service is still incurring charges for your account.
3. (Optional) Set a **$0 budget** alert so you are notified by email if anything still generates cost.

> **Best practice:** After cleanup, confirm the **CloudFormation stack list**, **Amplify apps**, and **EC2/RDS** listings are all empty so you do not leave a running bill. Budget data lags up to 24 hours, so re-check the next day.
