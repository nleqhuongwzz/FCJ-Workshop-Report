---
title : "Create API Gateway"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.4.11 </b> "
---

**API Gateway** exposes the Lambda backend as a public REST API. In production this is defined in `template.yaml` and created automatically by SAM. Here you create it manually to understand the pieces, using a **REST API** with a `{proxy+}` catch-all resource.

### 5.4.11.1 Create a REST API

1. Open the **API Gateway console** → **Create API**.
2. Under **REST API** (not HTTP API), click **Build**.


3. **Choose the protocol:** REST. **Create new API.**
4. **API name:** `edms-api`.
5. Leave **Endpoint Type** as **Regional** (or choose Edge for CDN-based).
6. Click **Create API**.

### 5.4.11.2 Add a catch-all proxy resource

A REST API resource corresponds to a path. To forward **any** path to the Lambda, add a `{proxy+}` resource:

1. On the **Resources** page of your API, click **Actions** → **Create Resource**.
2. Tick **Configure as proxy resource**.
3. In **Resource Path**, the value becomes `{proxy+}`.
4. Click **Create Resource**.

### 5.4.11.3 Configure the ANY method

The proxy resource is automatically given an **ANY** method. Configure it to point at the Lambda:

1. Select the `ANY` method under `{proxy+}`.
2. In the **Integration Request**, set:
   + **Integration type:** Lambda Function
   + **Lambda Region:** `ap-southeast-1`
   + **Lambda Function:** your EDMS Lambda (e.g. `EdmsBackendFunction`)
3. Click **Save**.


4. API Gateway will prompt you to grant permission to invoke the Lambda — click **OK** to allow it.

### 5.4.11.4 Add a root method

For a health check or the API root, add an **ANY** (or `GET`) method on the root `/` resource:

1. Select the root `/` resource.
2. Click **Actions** → **Create Method** → select **ANY** (or `GET`).
3. Set the same Lambda integration and click **Save**.


> **Note:** The `{proxy+}` resource lets API Gateway forward any path (`/auth/login`, `/documents`, ...) to the Lambda, which routes it inside the Spring Boot app. This is what makes the whole backend reachable through a single API.
