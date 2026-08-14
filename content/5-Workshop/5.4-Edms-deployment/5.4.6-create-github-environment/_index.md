---
title : "Create GitHub Environment 'production'"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.4.6 </b> "
---

A **GitHub Environment** groups environment-specific settings and can require **manual approval** before deployment. You create one named `production` to gate real deploys.

### 5.4.6.1 Create the environment

1. Open your repository on GitHub → **Settings** → **Environments**.
2. Click **New environment**.
3. Name it `production` and click **Configure environment**.


### 5.4.6.2 Add required reviewers (optional but recommended)

1. In the **Deployment branches and tags** section, choose **Selected branches** and pick `main`.
2. In the **Protection rules**, tick **Required reviewers**.
3. Add one or more GitHub users/teams that must approve before a production deploy runs.
4. Click **Save protection rules**.

Now, when a job targeting `production` starts, GitHub pauses it until a reviewer approves. This is a common control for production deployments.

### 5.4.6.3 Reference the environment in the workflow

Add the `environment: production` line to the `deploy` job so it is gated by this environment:

```yaml
deploy:
  needs: [test-backend, build-frontend]
  environment: production
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Configure AWS credentials (OIDC)
      uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
        aws-region: ap-southeast-1
```

> **Note:** Secrets stored at the **environment** level are only available to jobs that declare that environment. For simplicity this workshop keeps secrets at the repository level (5.4.5), so `environment` is used mainly for the approval gate.
