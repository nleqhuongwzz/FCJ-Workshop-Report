---
title : "Link Lambda with API Gateway"
date : 2024-01-01
weight : 12
chapter : false
pre : " <b> 5.4.12 </b> "
---

This section finalizes the API Gateway ↔ Lambda integration and **deploys** the API to a stage so it has a public invoke URL.

### 5.4.12.1 Configure the Lambda integration

1. In the API Gateway console, open your `edms-api` and select the `ANY` method under `{proxy+}`.
2. Open **Integration Request**.
3. Confirm **Integration type = Lambda Function**, the correct **region**, and the EDMS Lambda function name.
4. Click **Save**.
5. If prompted, grant API Gateway permission to invoke the Lambda by clicking **OK**.


> **Note:** The invoke permission (an `AWS::Lambda::Permission` resource) is what lets API Gateway call the function. Without it, calls return `403 Forbidden` with `Missing Authentication Token` or an access-denied error.

### 5.4.12.2 Deploy the API

An API Gateway API needs to be **deployed to a stage** before it has a usable URL:

1. Click **Deploy API**.
2. **Stage:** select `Prod`, or create a **New stage** named `Prod`.
3. (Optional) Add a stage description.
4. Click **Deploy**.


### 5.4.12.3 Retrieve the invoke URL

1. After deploying, the console shows the **Invoke URL** for the `Prod` stage:

```
https://xxxx.execute-api.ap-southeast-1.amazonaws.com/Prod
```


2. Copy this URL — it is the public entry point for the whole backend.

### 5.4.12.4 Test with curl

Confirm the API responds before wiring up the frontend:

```bash
curl https://xxxx.execute-api.ap-southeast-1.amazonaws.com/Prod/health
```

A `200 OK` (or the Spring Boot health JSON) means the link is working.

> **Note:** Set this URL as `REACT_APP_API_URL` for the frontend and as the backend's public API base so the app knows where to send requests.
