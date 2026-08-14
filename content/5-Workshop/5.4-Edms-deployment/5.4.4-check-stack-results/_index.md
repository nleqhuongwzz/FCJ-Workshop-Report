---
title : "Verify Stack Results and Retrieve Outputs"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

After the stack is created, verify that the resources exist and retrieve the **deploy role ARN** that the pipeline will assume.

### 5.4.4.1 Verify the stack status

1. Open the **CloudFormation console** → **Stacks**.
2. Find `edms-iam-stack` and confirm its **Status** is **CREATE_COMPLETE**.

![Figure 17. Stack complete](/images/5-Workshop/5.4-Edms-deployment/stack-complete.png)

3. If the status is **CREATE_FAILED** or **ROLLBACK_COMPLETE**, click **Events** to see the error (often a ThumbprintList or IAM acknowledgement issue), fix it, and recreate the stack.

### 5.4.4.2 Inspect the created resources

1. Select the stack `edms-iam-stack`.
2. Open the **Resources** tab.
3. Confirm the following resources exist and have logical IDs matching the template:

+ `GithubOidcProvider` — an `AWS::IAM::OIDCProvider`.
+ `GithubActionsDeployRole` — an `AWS::IAM::Role` named `github-actions-deploy-role`.

### 5.4.4.3 Retrieve the deploy role ARN

1. Open the **Outputs** tab of the stack.
2. Copy the value of `DeployRoleArn`, which looks like:

```
arn:aws:iam::<account-id>:role/github-actions-deploy-role
```


3. Keep this ARN handy — you will store it as the `AWS_DEPLOY_ROLE_ARN` secret on GitHub in the next section.

> **Note:** Alternatively, the OIDC role can be created manually in the IAM console (see 5.3.3). CloudFormation is preferred here because the configuration is versioned and reproducible.
