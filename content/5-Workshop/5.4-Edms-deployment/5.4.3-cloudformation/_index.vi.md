---
title : "Tạo IAM Stack qua CloudFormation"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

**deploy role** OIDC được tạo một lần dưới dạng một **CloudFormation stack**. Đưa trust policy và permissions vào template nghĩa là chúng được version hóa và tái tạo được như infrastructure as code.

### 5.4.3.1 Định nghĩa template CloudFormation

Tạo file `iam-stack.yaml` với nội dung sau. Nó định nghĩa hai tài nguyên: OIDC provider cho GitHub Actions và deploy role mà GitHub sẽ giả định.

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
                token.actions.githubusercontent.com:sub: 'repo:<account-cua-ban>/Enterprise-Document-Collaboration-Platform:*'
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/PowerUserAccess
        - arn:aws:iam::aws:policy/IAMFullAccess

Outputs:
  DeployRoleArn:
    Description: ARN of the GitHub Actions deploy role
    Value: !GetAtt GithubActionsDeployRole.Arn
```

### 5.4.3.2 Hiểu về OIDC provider

Tài nguyên `AWS::IAM::OIDCProvider` đăng ký issuer **GitHub** vào tài khoản AWS của bạn:

+ `Url` — OIDC endpoint công khai của GitHub Actions.
+ `ClientIdList` — `sts.amazonaws.com`, audience mà AWS STS mong đợi.
+ `ThumbprintList` — vân tay TLS certificate cho endpoint token (CloudFormation mới tự thêm; ghi ra đây cho đầy đủ).

Tài nguyên này là cầu nối cho phép runner GitHub xác thực vào AWS với vai trò IAM của bạn.

### 5.4.3.3 Hiểu về role trust policy

`GithubActionsDeployRole` tin tưởng OIDC provider, nhưng chỉ trong một điều kiện nghiêm ngặt:

+ `Condition` dùng `StringLike` trên claim `sub`: `repo:<account-cua-ban>/Enterprise-Document-Collaboration-Platform:*`.
+ Nghĩa là **chỉ** các workflow run xuất phát từ repository đó mới được giả định vai trò. Hậu tố `:*` bao phủ tất cả nhánh và tag.
+ `PowerUserAccess` + `IAMFullAccess` cho pipeline quyền rộng để deploy các tài nguyên SAM.

> **Ghi chú:** Cấp `PowerUserAccess` + `IAMFullAccess` là tiện lợi cho workshop này. Trong production, thu hẹp vai trò về đúng các action pipeline cần (Lambda, API Gateway, S3, CloudFormation, Step Functions, SNS).

### 5.4.3.4 Deploy stack trong console

1. Mở **CloudFormation console** → **Create stack** → **With new resources (standard)**.
2. Trong **Template source**, chọn **Upload a template file** và upload `iam-stack.yaml`.
3. Bấm **Next**.
4. **Stack name:** `edms-iam-stack`. Bấm **Next**.
5. Trên trang **Review**, kéo xuống và tick **I acknowledge that AWS CloudFormation might create IAM resources**.
6. Bấm **Submit** (hoặc **Create stack**).


> **Ghi chú:** Deploy stack tạo IAM roles yêu cầu bạn xác nhận rõ ràng capability IAM. Stack mất khoảng một phút để hoàn tất.
