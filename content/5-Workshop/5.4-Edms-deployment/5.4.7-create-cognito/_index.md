---
title : "Create Cognito User Pool"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7 </b> "
---

The Cognito User Pool created in 5.3.4 provides authentication. In this section you create **users** and assign them to **groups** so you can test the different role behaviors (`ADMIN`, `MANAGER`, `USER`).

### 5.4.7.1 Open your User Pool

1. Open the **Cognito console** → **User pools**.
2. Select your user pool (the one from 5.3.4).
3. Open the **Users** tab on the left.

### 5.4.7.2 Create a test user

1. Click **Create user**.
2. Fill in the email address as the **username** and set a **temporary password**.
3. Tick **Mark as verified** for the email (optional, avoids the verification flow during testing).
4. Click **Create user**.


Repeat for at least three users: one for each role (`ADMIN`, `MANAGER`, `USER`).

### 5.4.7.3 Assign a user to a group

1. In the list of users, click the email of the user you just created.
2. Open the **Groups** tab of that user.
3. Click **Add user to group**.
4. Select one of `ADMIN`, `MANAGER`, `USER` and click **Add user to group**.


Repeat so that each of your test users belongs to the intended role group.

### 5.4.7.4 Verify via the AWS CLI (optional)

You can confirm the users and groups from the terminal:

```bash
aws cognito-idp list-users --user-pool-id <COGNITO_USER_POOL_ID>
aws cognito-idp admin-list-groups-for-user \
  --user-pool-id <COGNITO_USER_POOL_ID> \
  --username admin@edms.vn
```

> **Note:** Create at least one user in each group (`ADMIN`, `MANAGER`, `USER`) so you can test the different role behaviors later in the approval smoke test (5.4.13).
