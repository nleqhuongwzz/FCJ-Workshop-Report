---
title : "Initialize and Configure S3"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

Amazon S3 stores the original document files of EDMS. Because files are accessed privately via **pre-signed URLs**, the bucket is created with public access blocked.

### 5.3.1.1 Create the S3 bucket

1. Open the **Amazon S3 Console**.
2. In the **Buckets** list, click **Create bucket**.


3. In the **Create bucket** page, configure:
+ **Bucket name:** a globally unique name, for example `edms-docs-bucket-<your-account-id>`.
+ **AWS Region:** `ap-southeast-1`.
+ **Object Ownership:** keep **ACLs disabled**.
+ **Block Public Access settings for this bucket:** keep all four checkboxes **checked** (the bucket stays private).
+ **Bucket Versioning:** enable it (supports document version history).
+ **Tags (optional):** add a tag such as `Project=EDMS`.
4. Scroll down and click **Create bucket**.
![Figure 1. Create bucket](/images/5-Workshop/5.3-Edms-infrastructure/create-bucket.png)


### 5.3.1.2 Verify the bucket

1. Back in the bucket list, confirm your bucket appears with status **0 objects**.
2. Open the bucket — it should be empty and private (public access blocked).

![Figure 3. Bucket created](/images/5-Workshop/5.3-Edms-infrastructure/bucket-created.png)

### 5.3.1.3 Note the bucket name

The backend needs the bucket name to store and retrieve files. Note it down — you will set it as the `AWS_S3_BUCKET` environment variable / SAM parameter later.

> **Note:** For this workshop the bucket stays private. EDMS generates short-lived **pre-signed URLs** to let users upload and download files without making the bucket public.
