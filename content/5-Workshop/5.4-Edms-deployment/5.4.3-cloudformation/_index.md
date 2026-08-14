---
title : "Create IAM Stack via CloudFormation"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

The OIDC **deploy role** is created once as a **CloudFormation stack**. Putting the trust policy and permissions into a template means they are versioned and reproducible as infrastructure as code.

### 5.4.3.1 Define the CloudFormation template

Create a file named `iam-stack.yaml` with the following content. It defines two resources: an OIDC provider for GitHub Actions and the deploy role that GitHub will assume.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: IAM resources for GitHub Actions OIDC deployment

Resources:
  GithubOidcProvider:
    Type: AWS::IAM::OIDCProvider
    Properties:
      Url: https://token.actions.githubusercontent.com
      ClientIdList:
        - sts.amazonaws.com
      ThumbprintList:
        - 6938fd4d98bab03faadb97b34396831e3780aea1

  GithubActionsDeployRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: github-actions-deploy-role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Federated: !Sub 'arn:aws:iam::${AWS::AccountId}:oidc-provider/token.actions.githubusercontent.com'
            Action: sts:AssumeRoleWithWebIdentity
            Condition:
              StringLike:
                token.actions.githubusercontent.com:sub: 'repo:<your-account>/Enterprise-Document-Collaboration-Platform:*'
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/PowerUserAccess
        - arn:aws:iam::aws:policy/IAMFullAccess

Outputs:
  DeployRoleArn:
    Description: ARN of the GitHub Actions deploy role
    Value: !GetAtt GithubActionsDeployRole.Arn
```

### 5.4.3.2 Understand the OIDC provider

The `AWS::IAM::OIDCProvider` resource registers **GitHub's** OIDC issuer with your AWS account:

+ `Url` — the well-known OIDC endpoint for GitHub Actions.
+ `ClientIdList` — `sts.amazonaws.com`, the audience that AWS STS expects.
+ `ThumbprintList` — the TLS certificate thumbprint for the token endpoint (added automatically by recent CloudFormation; shown here for completeness).

This resource is the bridge that lets GitHub runners authenticate to AWS as your IAM role.

### 5.4.3.3 Understand the role trust policy

The `GithubActionsDeployRole` trusts the OIDC provider, but only under a strict condition:

+ The `Condition` uses `StringLike` on the `sub` claim: `repo:<your-account>/Enterprise-Document-Collaboration-Platform:*`.
+ This means **only** workflow runs originating from that repository can assume the role. The `:*` suffix covers all branches and tags.
+ `PowerUserAccess` + `IAMFullAccess` give the pipeline broad rights to deploy SAM resources.

> **Note:** Granting `PowerUserAccess` + `IAMFullAccess` is convenient for this workshop. In production, scope the role down to the exact actions the pipeline needs (Lambda, API Gateway, S3, CloudFormation, Step Functions, SNS).

### 5.4.3.4 Deploy the stack in the console

1. Open the **CloudFormation console** → **Create stack** → **With new resources (standard)**.
2. Under **Template source**, choose **Upload a template file** and upload `iam-stack.yaml`.
3. Click **Next**.
4. **Stack name:** `edms-iam-stack`. Click **Next**.
5. On the **Review** page, scroll down and tick **I acknowledge that AWS CloudFormation might create IAM resources**.
6. Click **Submit** (or **Create stack**).


> **Note:** Deploying a stack that creates IAM roles requires you to explicitly acknowledge the IAM capability. The stack takes about a minute to finish.
