---
title : "Cost Monitoring & Alerts"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

Because this workshop builds several AWS services, it is important to **monitor and control cost** so you are not surprised by a bill. This section sets up **AWS Budgets** with **SNS email alerts**, explores costs with **Cost Explorer**, and adds a **CloudWatch alarm** for API errors.

### 5.5.4.1 Create an AWS Budget with email alerts

1. Open **Billing** → **Budgets** → **Create budget**.
2. Choose **Cost budget**.
3. Set the budget amount, e.g. **$10/month**.
4. Configure **Budget alerts** at multiple thresholds:
   + **50%** of the budget — a heads-up.
   + **80%** of the budget — close to the limit.
   + **100%** of the budget — over budget.
5. For each threshold, add the email address to **notify** (this delivers an email via **Amazon SNS**).
6. Click **Create budget**.

> **Note:** Multiple thresholds at 50 / 80 / 100% give you an early warning so you can act (stop or delete resources) before the bill grows.

### 5.5.4.2 Explore costs with Cost Explorer

1. Open **Billing** → **Cost Explorer**.
2. Choose a **date range** covering your workshop activity.
3. **Group by Service** to see which service costs the most.
4. Review the breakdown across Lambda, API Gateway, S3, Cognito, Step Functions, and Aurora.

> **Tip:** In this architecture, **Aurora** is the main cost driver because it is a database cluster that runs continuously. **Stop or delete it when not in use** to keep the bill near zero.

### 5.5.4.3 Create a CloudWatch alarm (optional)

Create a CloudWatch alarm on an operational metric, for example a **5XX error** threshold on the API:

1. Open **CloudWatch** → **Alarms** → **Create alarm**.
2. Select the metric `AWS/ApiGateway` → `5XXError` → your API.
3. Set the condition, e.g. **Greater than 0** for 1 datapoint (any server error triggers the alarm).
4. Choose an **SNS topic** as the notification action.
5. Add an alarm name (e.g. `edms-api-5xx`) and click **Create alarm**.

> **Note:** Together with the budget, this alarm alerts you to operational problems that could also indicate a costly runaway workload.
