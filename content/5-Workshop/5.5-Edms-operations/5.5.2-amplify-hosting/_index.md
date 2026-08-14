---
title : "Amplify Hosting + HTTPS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

**AWS Amplify** hosts the React frontend over **HTTPS** and connects to your **Git** repository, so every push to the branch triggers a rebuild and deployment. This section covers connecting GitHub, configuring the build spec for the `frontend/` folder, adding environment variables, and deploying.

### 5.5.2.1 Create an Amplify app

1. Open the **Amplify console** → **All apps** → **New app** → **Host web app**.
2. Choose your **Git provider** (e.g. **GitHub**) and click **Continue**.
3. Authorize Amplify to access your GitHub account, if prompted.
4. Select the **EDMS repository** and the **`main`** branch.
5. Click **Next**.


> **Note:** The frontend lives in the `frontend/` subfolder of the repository, so you must point the build at that directory.

### 5.5.2.2 Configure the build settings

In the **Build settings** section, replace the build spec with one that targets the `frontend/` folder. The key points are:

+ `preBuild` — change into `frontend` and install dependencies with `npm ci` (a clean, lock-file based install).
+ `build` — change into `frontend` and run `npm run build` to produce the static bundle.
+ `baseDirectory` — set to `frontend/build` so Amplify serves the generated output.

```yaml
version: 1
applications:
  - frontend:
      phases:
        preBuild:
          commands:
            - cd frontend && npm ci
        build:
          commands:
            - cd frontend && npm run build
      artifacts:
        baseDirectory: frontend/build
        files:
          - '**/*'
```

> **Note:** If the build cannot find `package-lock.json`, run `npm install` in `frontend/` and commit the lock file before deploying.

### 5.5.2.3 Add environment variables

In **Environment variables**, add the values from the previous sections. These variables tell the built app where the API and Cognito live:

```
REACT_APP_API_URL=<API Gateway invoke URL>
REACT_APP_COGNITO_USER_POOL_ID=<pool-id>
REACT_APP_COGNITO_CLIENT_ID=<client-id>
REACT_APP_COGNITO_REGION=ap-southeast-1
```

+ `REACT_APP_API_URL` — the API Gateway invoke URL (section 5.4).
+ `REACT_APP_COGNITO_USER_POOL_ID` — your Cognito User Pool id.
+ `REACT_APP_COGNITO_CLIENT_ID` — your Cognito app client id.
+ `REACT_APP_COGNITO_REGION` — the region, `ap-southeast-1`.

### 5.5.2.4 Deploy the app

1. Click **Save and deploy**.
2. Amplify runs the build pipeline (source → preBuild → build → deploy).
3. Wait for the build status to become **Available**.

![Figure 42. Amplify deployed](/images/5-Workshop/5.5-Edms-operations/amplify-deployed.png)

> **Note:** The **default domain** (e.g. `https://main.d3xxxx.amplifyapp.com`) is the public URL of the application and is served automatically over **HTTPS** with a valid certificate.
