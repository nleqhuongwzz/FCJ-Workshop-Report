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
![Figure 11. Create user pool](../../../images/5-Workshop/5.3-Edms-infrastructure/create-userpool.png)

2. In the **Set up resources for your application** page, configure:

**Define your application**
+ **Application type:** choose **Single-page application (SPA)** — EDMS is a React SPA. (Traditional web app, Mobile, and Machine-to-machine are also available.)
+ **Name your application:** `edms-client`.

**Configure options**

*Options for sign-in identifiers*
+ Select **Email** as the sign-in identifier (users sign in with email and password).

*Self-registration*
+ **Enable self-registration:** keep this **off** if you want only admins to create users (recommended for a private enterprise tool). If you want to open sign-up to the public, turn it on.
+ **Required attributes for sign-up:** if you enable self-registration, select **Email** as a required attribute.

*Add a return URL (optional)*
+ **Return URL:** for local development you can enter `http://localhost:3000`. In production, set it to your **Amplify** URL (e.g. `https://main.d3xxxx.amplifyapp.com`). Cognito redirects here after a successful sign-in.

3. Click **Create user directory**.

![Figure 12. User pool created](../../../images/5-Workshop/5.3-Edms-infrastructure/userpool-created.png)

> **Note:** With the new Cognito console flow, the user pool and app client are created together. The **app client has no secret**, so it can be used in browser-based (SPA) flows.

### 5.3.4.3 Create groups (roles)

Cognito groups map directly to the application roles (`ADMIN` / `MANAGER` / `USER`).

1. Open your User Pool (`edms-user-pool`).
2. Open the **Groups** tab.
3. Click **Create group** and create three groups:
+ `ADMIN`
+ `MANAGER`
+ `USER`

![Figure 13. Groups](../../../images/5-Workshop/5.3-Edms-infrastructure/groups.png)

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
