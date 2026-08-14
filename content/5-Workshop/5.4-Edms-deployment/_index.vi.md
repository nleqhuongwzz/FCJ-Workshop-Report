---
title : "Triển khai EDMS trên AWS"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

Trong phần này, bạn sẽ triển khai EDMS lên AWS bằng **infrastructure as code** (AWS SAM + CloudFormation) và **GitHub Actions** CI/CD pipeline xác thực qua OIDC.

#### Nội dung

1. [Chuẩn bị Mã nguồn và Cấu trúc Project](5.4.1-prepare-project-structure/)
2. [Cấu hình GitHub Actions Workflow](5.4.2-github-actions-workflow/)
3. [Tạo IAM Stack qua CloudFormation](5.4.3-cloudformation/)
4. [Xác minh Kết quả Stack và Lấy Outputs](5.4.4-check-stack-results/)
5. [Khai báo Secrets và Variables trên GitHub](5.4.5-declare-secrets-variables/)
6. [Tạo GitHub Environment 'production'](5.4.6-create-github-environment/)
7. [Tạo Cognito User Pool](5.4.7-create-cognito/)
8. [Tạo Step Functions State Machine](5.4.8-create-stepfunctions/)
9. [Xác minh CI/CD Pipeline](5.4.9-verify-cicd-pipeline/)
10. [Kích hoạt Pipeline](5.4.10-trigger-pipeline/)
11. [Tạo API Gateway](5.4.11-create-api-gateway/)
12. [Liên kết Lambda với API Gateway](5.4.12-link-lambda-apigateway/)
13. [Health Checks và Smoke Tests sau Deploy](5.4.13-health-checks-smoke-tests/)
