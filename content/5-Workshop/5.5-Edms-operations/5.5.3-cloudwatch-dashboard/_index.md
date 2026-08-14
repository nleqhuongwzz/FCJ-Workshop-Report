---
title : "CloudWatch Dashboard"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

A **CloudWatch Dashboard** gives you a single view of the health and performance of the EDMS application by combining metrics from Lambda, API Gateway, and Step Functions on one page.

### 5.5.3.1 Create a dashboard

1. Open the **CloudWatch console** → **Dashboards** → **Create dashboard**.
2. Give the dashboard a name, e.g. `edms-dashboard`, and click **Create**.


### 5.5.3.2 Add a Lambda widget

1. Click **Add widget** → choose **Line**.
2. Under **Metrics** → **AWS namespaces**, select **Lambda**.
3. Choose the metric dimensions for your function `edms-lambda-stack-EdmsApiFunction` and add:
   + **Invocations** — how often the function ran.
   + **Errors** — how many executions failed.
4. Configure the **period** (e.g. 5 minutes) and click **Create widget**.

### 5.5.3.3 Add an API Gateway widget

1. Click **Add widget** → **Line**.
2. Under **AWS namespaces**, select **API Gateway** → your API.
3. Add the metrics:
   + **Count** — total requests.
   + **4XXError** — client errors (bad requests / auth failures).
   + **5XXError** — server errors.
4. Click **Create widget**.

### 5.5.3.4 Add a Step Functions widget

1. Click **Add widget** → **Line**.
2. Under **AWS namespaces**, select **Step Functions** → your state machine.
3. Add the metric **ExecutionsSucceeded** — how many approval workflows completed successfully.
4. Click **Create widget**.

### 5.5.3.5 Arrange and save the dashboard

1. Add several widgets and drag them to arrange the layout.
2. Click **Save dashboard**.
3. Re-open the dashboard later to see a live view of application health.


> **Note:** A dashboard is **read-only** and very cheap. It does not raise alarms by itself — it simply helps you spot anomalies at a glance before they affect users.
