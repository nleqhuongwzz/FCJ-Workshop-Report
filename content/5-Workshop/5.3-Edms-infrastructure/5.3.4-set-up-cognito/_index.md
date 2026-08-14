---
title : "Initialize and Configure Cognito"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

Amazon Cognito provides authentication and role-based authorization for EDMS. Users sign in and receive a **JWT** (JSON Web Token) that the backend validates on every request. This section creates a **User Pool**, an **app client** without a secret, and the three groups that map to the application roles.

### 5.3.4.1 Open the Cognito console

1. In the AWS console, make sure the region is **ap-southeast-1**.
2. Open **Amazon Cognito console** → **User pools**.

### 5.3.4.2 Create a User Pool

1. Click **Create user pool**.
![Figure 11. Create user pool](/images/5-Workshop/5.3-Edms-infrastructure/create-userpool.png)


2. In **Configure sign-in experience**:
+ **Sign-in options:** select **Email**.
+ Click **Next**.
3. In **Configure security requirements**:
+ Set a **password policy** (minimum 8 characters, require at least one number).
+ Click **Next**.
4. In **Configure sign-up experience**:
+ Keep **Self-service sign-up** enabled (or disable it if you want only admin-created users).
+ Click **Next**.
5. In **Configure message delivery**:
+ Select **Send email with Cognito**.
+ Click **Next**.
6. In **Integrate your app**:
+ **User pool name:** `edms-user-pool`.
+ **App client name:** `edms-client`; **leave "Generate a client secret" unchecked** — the backend needs a public client for the sign-in flow.
+ Click **Next**.
7. Review the settings and click **Create user pool**.

![Figure 12. User pool created](/images/5-Workshop/5.3-Edms-infrastructure/userpool-created.png)

> **Note:** The app client has **no secret**, so it can be used in browser-based flows. Do not enable a client secret for the public client.

### 5.3.4.3 Create groups (roles)

Cognito groups map directly to the application roles (`ADMIN` / `MANAGER` / `USER`).

1. Open your User Pool (`edms-user-pool`).
2. Open the **Groups** tab.
3. Click **Create group** and create three groups:
+ `ADMIN`
+ `MANAGER`
+ `USER`

![Figure 13. Groups](/images/5-Workshop/5.3-Edms-infrastructure/groups.png)

4. Assign users to groups. A user in a group inherits that group's role in the application.

> **Note:** `ADMIN` has full access, `MANAGER` can manage documents of their team, and `USER` is a regular member with read/comment permissions.

### 5.3.4.4 Note the IDs

The backend and frontend need the following values — note them down:

```
COGNITO_USER_POOL_ID=<pool-id>     e.g. ap-southeast-1_XXXXX
COGNITO_CLIENT_ID=<client-id>
COGNITO_REGION=ap-southeast-1
```

> **Note:** The backend maps a Cognito group to an application role (`ADMIN` / `MANAGER` / `USER`) and validates the JWT on every request. Keep the pool and client IDs handy for the `.env` / SAM configuration in later sections.
