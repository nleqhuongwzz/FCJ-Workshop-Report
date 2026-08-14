---
title : "Cấu hình GitHub Actions Workflow"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

CI/CD pipeline được định nghĩa trong `.github/workflows/deploy.yml`. Nó chạy mỗi lần push lên nhánh `main` và deploy EDMS lên AWS bằng **OIDC** — không lưu access key dài hạn nào trong GitHub.

### 5.4.2.1 Tổng quan workflow

Workflow gồm ba job chạy tuần tự (job deploy chờ hai job đầu tiên):

1. `test-backend` — chạy `mvn test` để kiểm tra các unit test Java.
2. `build-frontend` — chạy `npm ci && npm run build` để biên dịch SPA React.
3. `deploy` — xác thực AWS qua OIDC và chạy `sam deploy`.

```
push lên main
   → test-backend
   → build-frontend
   → deploy (OIDC + SAM)   [chờ cả hai]
```

### 5.4.2.2 File deploy.yml

Tạo `.github/workflows/deploy.yml` với nội dung sau:

```yaml
name: EDMS CI/CD

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  id-token: write   # cần cho OIDC
  contents: read

jobs:
  test-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: corretto
          java-version: "17"
      - run: mvn -B test
        working-directory: backend

  build-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "18"
      - run: npm ci && npm run build
        working-directory: frontend

  deploy:
    needs: [test-backend, build-frontend]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure AWS credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
          aws-region: ap-southeast-1
      - run: mvn -B clean package -DskipTests
        working-directory: backend
      - uses: aws-actions/setup-sam@v2
      - name: Deploy via SAM
        working-directory: backend
        run: |
          sam deploy --stack-name edms-lambda-stack \
            --no-confirm-changeset --no-fail-on-empty-changeset \
            --capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND \
            --parameter-overrides "CognitoUserPoolId=${{ secrets.COGNITO_USER_POOL_ID }} CognitoClientId=${{ secrets.COGNITO_CLIENT_ID }} AuroraEndpoint=${{ secrets.AURORA_ENDPOINT }} S3BucketName=${{ secrets.AWS_S3_BUCKET }} DbUserName=${{ secrets.DB_USER_AWS }} DbUserPass=${{ secrets.DB_PASS_AWS }} SnsTopicArn=${{ secrets.SNS_TOPIC_ARN }} BackendLambdaArn=${{ secrets.BACKEND_LAMBDA_ARN }}"
```

### 5.4.2.3 Xác thực OIDC hoạt động thế nào

Job deploy **không nhận** AWS credentials tĩnh:

1. Runner yêu cầu một **ID token** OIDC từ GitHub (lý do cần `permissions.id-token: write`).
2. `configure-aws-credentials` gọi `sts:AssumeRoleWithWebIdentity` với token đó, giả định vai trò trong `AWS_DEPLOY_ROLE_ARN`.
3. AWS kiểm tra claim `sub` của token đối chiếu với trust policy trong IAM role.
4. Credentials ngắn hạn, giới hạn được trả về và chỉ dùng trong thời gian job chạy.

> **Điểm mấu chốt:** Vì vai trò được giả định qua OIDC, không có secret access key nào được lưu trong GitHub — một best practice bảo mật.

### 5.4.2.4 Tham số overrides

Danh sách `--parameter-overrides` truyền các giá trị theo môi trường vào template SAM. Mỗi giá trị được đọc từ một GitHub **secret** tại thời điểm deploy:

+ `CognitoUserPoolId`, `CognitoClientId` — xác thực (từ 5.3.4).
+ `AuroraEndpoint`, `DbUserName`, `DbUserPass` — kết nối Aurora database (từ 5.3.2).
+ `S3BucketName` — bucket lưu file (từ 5.3.1).
+ `SnsTopicArn`, `BackendLambdaArn` — tạo sau ở 5.4.8 / 5.4.12.

> **Ghi chú:** Toàn bộ secret này được khai báo ở mục 5.4.5. Nếu thiếu một secret, bước SAM deploy sẽ fail với lỗi thay thế (substitution error).
