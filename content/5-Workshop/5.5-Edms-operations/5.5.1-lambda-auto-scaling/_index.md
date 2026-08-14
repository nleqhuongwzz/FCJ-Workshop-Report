---
title : "Lambda Concurrency & Auto-Scaling"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

Lambda is a **serverless** service: AWS runs it for you and **scales it automatically** in response to traffic. You do not manage servers or instance counts. This section explains how that scaling works, how to control it with **reserved concurrency**, and how to monitor it.

### 5.5.1.1 Understand how Lambda auto-scaling works

1. When a request arrives, AWS Lambda starts an **execution environment** to run your function.
2. If a second request arrives **while the first is still running**, Lambda starts a **second concurrent execution** in a separate environment.
3. As traffic grows, Lambda keeps adding concurrent executions up to the **account concurrency limit** (default 1000 concurrent executions per region).
4. When requests finish, the unused environments are **reclaimed** automatically, so you only pay for compute time actually used.

Because there is **no server to patch or manage**, the platform scales up and down on its own based purely on the request rate.

> **Note:** This automatic scaling is what makes Lambda ideal for unpredictable workloads like the EDMS API.

### 5.5.1.2 Configure reserved concurrency (optional)

By default Lambda can burst up to the account limit. If you want to **cap** how many concurrent executions your function uses — for example to protect a downstream database such as Aurora from a traffic spike — configure **reserved concurrency**:

1. Open the **Lambda console** → select your function, e.g. `edms-lambda-stack-EdmsApiFunction`.
2. Open the **Configuration** tab → **Concurrency**.
3. Click **Edit**.
4. Select **Reserve concurrency** and set a value (e.g. `5`).
5. Click **Save**.


> **Note:** In `template.yaml` this is set with the `ReservedConcurrentExecutions` property on the function resource. A value of `0` disables the function; any other value becomes the hard limit for concurrent executions.

### 5.5.1.3 Understand provisioned concurrency (advanced)

**Provisioned concurrency** pre-warms a fixed number of environments so they are ready immediately, avoiding **cold starts**:

+ It is useful for latency-sensitive functions.
+ It is **not required** for this workshop and **costs extra**, so leave it disabled unless you need it.

### 5.5.1.4 Monitor scaling behavior

1. In the Lambda console open the **Monitor** tab of your function.
2. View the **Invocations** graph to see how many times the function ran.
3. View the **Concurrent executions** graph to confirm Lambda scaled in/out with the traffic.
4. Optionally compare the two graphs with the CloudWatch Dashboard you build in section 5.5.3.

![Figure 39. Lambda monitoring](/images/5-Workshop/5.5-Edms-operations/lambda-monitor.png)

> **Note:** If you ever see **Throttles** on the monitoring page, your reserved concurrency is too low or you are hitting the account limit — increase the value accordingly.
