---
title : "Tạo Step Functions State Machine"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.4.8 </b> "
---

EDMS điều phối **quy trình phê duyệt tài liệu** bằng **AWS Step Functions**. Phần này tạo SNS notification topic và `DocumentApprovalStateMachine`, sử dụng mẫu **Wait for Task Token** để tạm dừng cho đến khi manager quyết định.

### 5.4.8.1 Tạo SNS topic

1. Mở **SNS console** → **Topics** → **Create topic**.
2. **Type:** Standard.
3. **Name:** `edms-notifications`.
4. Giữ nguyên các cài đặt khác và bấm **Create topic**.


5. Sao chép **topic ARN** — bạn sẽ đặt nó làm secret `SNS_TOPIC_ARN` (5.4.5) và tham chiếu trong state machine.

### 5.4.8.2 Tạo email subscription

1. Mở topic `edms-notifications` vừa tạo.
2. Mở tab **Subscriptions**.
3. Bấm **Create subscription**.
4. **Protocol:** Email. **Endpoint:** địa chỉ email của bạn.
5. Bấm **Create subscription**.
6. Kiểm tra hộp thư và bấm **Confirm subscription** trong email AWS gửi đến.

> **Ghi chú:** Subscription phải được xác nhận trước khi state machine có thể gửi email thông báo. Trạng thái sẽ chuyển thành **Confirmed**.

### 5.4.8.3 Hiểu về mẫu Wait for Task Token

Một hàm Lambda có **giới hạn thời gian chạy tối đa** (mặc định tối đa 15 phút), nên không thể chờ hàng giờ một quyết định của con người. Mẫu **Wait for Task Token** giải quyết điều này:

1. Workflow bắt đầu một execution và invoke Lambda của bạn với `"Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken"`.
2. Nó truyền một **task token** duy nhất cho Lambda như một phần của payload (`$$.Task.Token`).
3. Lambda lưu tài liệu ở trạng thái `PENDING` và **trả về ngay**, nhưng state machine **không chuyển tiếp** — nó chờ tại state `CaptureToken`.
4. Sau đó, khi manager gọi `POST /approval/approve` hoặc `POST /approval/reject`, Lambda gọi `SendTaskSuccess` (hoặc `SendTaskFailure`) với task token đã lưu.
5. Callback đó báo Step Functions tiếp tục sang state tiếp theo.

```
CaptureToken (waitForTaskToken)  ── chờ vô hạn
   → Decision (Choice)            ── APPROVE / REJECT
   → MarkApproved / MarkRejected  ── cập nhật DB qua Lambda
   → NotifyApproved / NotifyRejected  ── publish lên SNS
![Figure 24. Luồng state machine](/images/5-Workshop/5.4-Edms-deployment/statemachine.png)
```


### 5.4.8.4 Định nghĩa ASL

Tạo state machine bằng cách dán định nghĩa **Amazon States Language (ASL)** này (thay các placeholder `${...}` bằng ARN thật của bạn):

```json
{
  "Comment": "Document approval workflow",
  "StartAt": "CaptureToken",
  "States": {
    "CaptureToken": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken",
      "Parameters": {
        "FunctionName": "${BACKEND_LAMBDA_ARN}",
        "Payload": {
          "taskToken.$": "$$.Task.Token",
          "documentId.$": "$.documentId",
          "action": "SUBMIT"
        }
      },
      "Next": "Decision"
    },
    "Decision": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.decision",
          "StringEquals": "APPROVE",
          "Next": "MarkApproved"
        },
        {
          "Variable": "$.decision",
          "StringEquals": "REJECT",
          "Next": "MarkRejected"
        }
      ],
      "Default": "MarkRejected"
    },
    "MarkApproved": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "${BACKEND_LAMBDA_ARN}",
        "Payload": {
          "documentId.$": "$.documentId",
          "action": "MARK_APPROVED"
        }
      },
      "Next": "NotifyApproved"
    },
    "MarkRejected": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "${BACKEND_LAMBDA_ARN}",
        "Payload": {
          "documentId.$": "$.documentId",
          "action": "MARK_REJECTED"
        }
      },
      "Next": "NotifyRejected"
    },
    "NotifyApproved": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "${SNS_TOPIC_ARN}",
        "Message": "Document approved",
        "Subject": "EDMS - Approval"
      },
      "End": true
    },
    "NotifyRejected": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "${SNS_TOPIC_ARN}",
        "Message": "Document rejected",
        "Subject": "EDMS - Approval"
      },
      "End": true
    }
  }
}
```

### 5.4.8.5 Tạo state machine trong console

1. Mở **Step Functions console** → **State machines** → **Create state machine**.
2. Chọn **Author with code**, rồi chọn loại **Standard** (bắt buộc cho `.waitForTaskToken` và chờ lâu).
3. Dán định nghĩa ASL ở trên.
4. **Permissions:** tạo role mới và đảm bảo nó cho phép `lambda:InvokeFunction` và `sns:Publish`.
5. **Name:** `DocumentApprovalStateMachine`.
6. Bấm **Create state machine**.

### 5.4.8.6 Ghi lại các ARN

1. Sao chép **state machine ARN** (ví dụ `arn:aws:states:ap-southeast-1:<account>:stateMachine:DocumentApprovalStateMachine`). Lambda cần nó (`STEP_FUNCTIONS_ARN`) để khởi động execution.
2. Xác nhận **SNS topic ARN** từ 5.4.8.1 đã được lưu làm secret `SNS_TOPIC_ARN`.

> **Mẫu mấu chốt:** State `CaptureToken` dùng `.waitForTaskToken`, nên nó có thể chờ vô hạn quyết định của manager — điều một invocation Lambda đơn lẻ không thể làm được vì giới hạn timeout 15 phút.
