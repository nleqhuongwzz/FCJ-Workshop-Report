---
title : "Create Step Functions State Machine"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.4.8 </b> "
---

EDMS orchestrates the **document approval workflow** with **AWS Step Functions**. This section creates the SNS notification topic and the `DocumentApprovalStateMachine`, which uses the **Wait for Task Token** pattern to pause until a manager makes a decision.

### 5.4.8.1 Create the SNS topic

1. Open the **SNS console** → **Topics** → **Create topic**.
2. **Type:** Standard.
3. **Name:** `edms-notifications`.
4. Leave the other settings at their defaults and click **Create topic**.


5. Copy the **topic ARN** — you will set it as the `SNS_TOPIC_ARN` secret (5.4.5) and reference it in the state machine.

### 5.4.8.2 Create an email subscription

1. Open the topic `edms-notifications` you just created.
2. Open the **Subscriptions** tab.
3. Click **Create subscription**.
4. **Protocol:** Email. **Endpoint:** your email address.
5. Click **Create subscription**.
6. Check your inbox and click **Confirm subscription** in the email AWS sends you.

> **Note:** The subscription must be confirmed before the state machine can deliver notification emails. The status should change to **Confirmed**.

### 5.4.8.3 Understand the Wait for Task Token pattern

A Lambda function has a **maximum timeout** (by default up to 15 minutes), so it cannot wait hours for a human decision. The **Wait for Task Token** pattern solves this:

1. The workflow starts an execution and invokes your Lambda with `"Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken"`.
2. It passes a unique **task token** to the Lambda as part of the payload (`$$.Task.Token`).
3. The Lambda stores the document as `PENDING` and **returns immediately**, but the state machine **does not move on** — it waits at the `CaptureToken` state.
4. Later, when a manager calls `POST /approval/approve` or `POST /approval/reject`, the Lambda calls `SendTaskSuccess` (or `SendTaskFailure`) with the saved task token.
5. That callback tells Step Functions to continue to the next state.

```
CaptureToken (waitForTaskToken)  ── waits indefinitely
   → Decision (Choice)            ── APPROVE / REJECT
   → MarkApproved / MarkRejected  ── update DB via Lambda
   → NotifyApproved / NotifyRejected  ── publish to SNS
![Figure 24. State machine flow](/images/5-Workshop/5.4-Edms-deployment/statemachine.png)
```


### 5.4.8.4 The ASL definition

Create the state machine by pasting this **Amazon States Language (ASL)** definition (replace the `${...}` placeholders with your real ARNs):

```json
{
  "Comment": "Document approval workflow",
  "StartAt": "CaptureToken",
  "States": {
    "CaptureToken": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken",
      "Parameters": {
        "FunctionName": "${BACKEND_LAMBDA_ARN}",
        "Payload": {
          "taskToken.$": "$$.Task.Token",
          "documentId.$": "$.documentId",
          "action": "SUBMIT"
        }
      },
      "Next": "Decision"
    },
    "Decision": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.decision",
          "StringEquals": "APPROVE",
          "Next": "MarkApproved"
        },
        {
          "Variable": "$.decision",
          "StringEquals": "REJECT",
          "Next": "MarkRejected"
        }
      ],
      "Default": "MarkRejected"
    },
    "MarkApproved": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "${BACKEND_LAMBDA_ARN}",
        "Payload": {
          "documentId.$": "$.documentId",
          "action": "MARK_APPROVED"
        }
      },
      "Next": "NotifyApproved"
    },
    "MarkRejected": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "${BACKEND_LAMBDA_ARN}",
        "Payload": {
          "documentId.$": "$.documentId",
          "action": "MARK_REJECTED"
        }
      },
      "Next": "NotifyRejected"
    },
    "NotifyApproved": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "${SNS_TOPIC_ARN}",
        "Message": "Document approved",
        "Subject": "EDMS - Approval"
      },
      "End": true
    },
    "NotifyRejected": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "${SNS_TOPIC_ARN}",
        "Message": "Document rejected",
        "Subject": "EDMS - Approval"
      },
      "End": true
    }
  }
}
```

### 5.4.8.5 Create the state machine in the console

1. Open the **Step Functions console** → **State machines** → **Create state machine**.
2. Choose **Author with code**, then choose the **Standard** type (required for `.waitForTaskToken` and long waits).
3. Paste the ASL definition from above.
4. **Permissions:** create a new role and ensure it allows `lambda:InvokeFunction` and `sns:Publish`.
5. **Name:** `DocumentApprovalStateMachine`.
6. Click **Create state machine**.

### 5.4.8.6 Note down the ARNs

1. Copy the **state machine ARN** (e.g. `arn:aws:states:ap-southeast-1:<account>:stateMachine:DocumentApprovalStateMachine`). The Lambda needs it (`STEP_FUNCTIONS_ARN`) to start executions.
2. Confirm the **SNS topic ARN** from 5.4.8.1 is stored as the `SNS_TOPIC_ARN` secret.

> **Key pattern:** The `CaptureToken` state uses `.waitForTaskToken`, so it can wait indefinitely for a manager's decision — something a single Lambda invocation cannot do because of the 15-minute timeout limit.
