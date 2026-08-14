---
title : "CloudWatch Logs and Log Insights"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5.5 </b> "
---

Every Lambda function writes its output to **CloudWatch Logs**. You can inspect those logs and run **Logs Insights** queries to troubleshoot failures in the EDMS API.

### 5.5.5.1 View Lambda logs

1. Open the **CloudWatch console** → **Log groups**.
2. Find the log group `/aws/lambda/edms-lambda-stack-EdmsApiFunction...` (the exact name ends with the function name).
3. Open the latest **log stream** (each stream corresponds to an execution environment).
4. Read the log events to see the function output, timestamps, and any messages printed by your code.

![Figure 48. Log groups](/images/5-Workshop/5.5-Edms-operations/log-groups.png)

> **Note:** Logs are retained indefinitely by default. You can set a **retention period** in **Log groups** → **Actions** → **Edit retention** to control storage cost.

### 5.5.5.2 Run a Logs Insights query

Logs Insights lets you search and aggregate log data with a SQL-like syntax:

1. Open **CloudWatch** → **Logs Insights**.
2. Select the Lambda log group from the dropdown.
3. Set the **time range** to cover the failure you want to investigate.
4. Run a query that filters for errors:

```sql
fields @timestamp, @message
| filter @message like /ERROR|Exception/
| sort @timestamp desc
| limit 20
```

5. Review the matching log events, which are sorted newest first.

> **Note:** Querying logs helps you understand why a request failed — for example an **invalid Cognito token** (401) or a **database connection error** in the Lambda that calls Aurora.

### 5.5.5.3 Common troubleshooting examples

+ **Throttling / concurrency errors** — look for messages about concurrency or throttles in the log.
+ **Cognito 401** — the access token is missing, expired, or from the wrong client.
+ **Database timeout** — a connection error to Aurora usually appears as an exception stack trace.
+ **Step Functions failure** — check the state machine's own execution history and the Lambda logs it invoked.
