---
title : "Tạo GitHub Environment 'production'"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.4.6 </b> "
---

Một **GitHub Environment** nhóm các cài đặt theo môi trường và có thể yêu cầu **phê duyệt thủ công** trước khi deploy. Bạn tạo một môi trường tên `production` để chặn các lần deploy thật.

### 5.4.6.1 Tạo environment

1. Mở repository của bạn trên GitHub → **Settings** → **Environments**.
2. Bấm **New environment**.
3. Đặt tên `production` và bấm **Configure environment**.


### 5.4.6.2 Thêm required reviewers (tùy chọn nhưng nên dùng)

1. Trong phần **Deployment branches and tags**, chọn **Selected branches** và chọn `main`.
2. Trong **Protection rules**, tick **Required reviewers**.
3. Thêm một hoặc nhiều user/team GitHub phải phê duyệt trước khi deploy production chạy.
4. Bấm **Save protection rules**.

Giờ, khi một job nhắm tới `production` bắt đầu, GitHub sẽ tạm dừng nó cho đến khi reviewer phê duyệt. Đây là một control phổ biến cho deploy production.

### 5.4.6.3 Tham chiếu environment trong workflow

Thêm dòng `environment: production` vào job `deploy` để nó bị chặn bởi môi trường này:

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

> **Ghi chú:** Secret lưu ở cấp **environment** chỉ khả dụng cho các job khai báo môi trường đó. Workshop này giữ secret ở cấp repository (5.4.5) cho đơn giản, nên `environment` chủ yếu dùng làm cổng phê duyệt.
