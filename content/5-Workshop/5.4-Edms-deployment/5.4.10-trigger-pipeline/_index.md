---
title : "Trigger Pipeline"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.4.10 </b> "
---

The pipeline runs automatically on every push to `main`. It can also be triggered manually using `workflow_dispatch`.

### 5.4.10.1 Trigger by pushing code

The workflow's `on.push.branches: [main]` trigger starts automatically when you push to `main`. From the repository root:

```bash
git add .
git commit -m "chore: trigger deployment"
git push origin main
```

The push to `main` starts the `EDMS CI/CD` workflow automatically. Open the **Actions** tab to watch it run.

> **Note:** Only commits to the `main` branch trigger the deploy. Pushes to other branches run `test-backend` and `build-frontend` but the `deploy` job is skipped because of its `if: github.ref == 'refs/heads/main'` condition (5.4.2).

### 5.4.10.2 Trigger manually (workflow_dispatch)

Because the workflow declares `workflow_dispatch` in its `on` block, you can start it manually:

1. Open the **Actions** tab of your repository.
2. Select the `EDMS CI/CD` workflow from the left sidebar.
3. Click **Run workflow**.
4. Choose the **branch** (e.g. `main`).
5. Click **Run workflow**.


### 5.4.10.3 Monitor the run

1. A new run appears under the workflow. Click it to follow the three jobs.
2. If you enabled **required reviewers** (5.4.6), approve the `deploy` job when prompted.
3. Confirm the run ends with a green checkmark and the SAM deploy step reports `Successfully created/updated stack - edms-lambda-stack`.

> **Note:** Only jobs that satisfy the `if` condition on the deploy job will actually deploy. The deploy job runs only on the `main` branch, regardless of how the workflow was triggered.
