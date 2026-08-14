---
title : "Initialize and Configure Aurora RDS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

EDMS uses **Amazon Aurora (MySQL)** to store all relational metadata (users, folders, documents, comments, version history). In this section you create the Aurora cluster, wait for it to become available, create the initial `edms` database, and note the endpoint for later configuration.

### 5.3.2.1 Open the RDS console

1. In the AWS console, make sure the region is **ap-southeast-1**.
2. Open **Amazon RDS console**.
3. In the left menu, click **Databases** to see the (empty) list of clusters.

### 5.3.2.2 Create an Aurora MySQL cluster

1. Click **Create database**.

![Figure 4. Create database](../../../images/5-Workshop/5.3-Edms-infrastructure/create-database.png)

2. In the **Create database** page, configure the following:

**Engine options**
+ **Engine type:** choose **Aurora (MySQL Compatible)**.

**Database creation method**
+ Choose **Standard create** (Full configuration) to control all options yourself.

**Templates**
+ Choose a template. For a low-cost setup choose **Dev/Test** (recommended for the workshop). **Production** enables high-availability defaults and is more expensive.

**Cluster scalability type**
+ **Aurora Serverless v2** (recommended — scales with usage, ideal for the workshop) or **Provisioned** (fixed capacity).
+ If **Provisioned**, under **Instance configuration** choose a **Burstable class** (`db.t` series, e.g. `db.t3.medium` or `db.r7g.large`) — cheaper than memory-optimized (`db.r`).

**Settings**
+ **DB cluster identifier:** `edms`.
+ **Engine version:** keep the default **Aurora MySQL 3.x** (compatible with MySQL 8.x).

**Credentials Settings**
+ **Master username:** `admin`.
+ **Credentials management:** choose **Self managed** and set a strong **Master password**. Store it in a safe place (the backend needs it).

**Cluster storage configuration**
+ **Aurora Standard** (pay-per-request I/O) is usually enough and cheaper for a workshop. **Aurora I/O-Optimized** gives predictable pricing if your workload is I/O-heavy.

**Availability & durability**
+ **Don't create an Aurora Replica** (a single instance is enough for the workshop).

**Connectivity**
+ **Compute resource:** **Don't connect to an EC2 compute resource**.
+ **Network type:** **IPv4**.
+ **VPC:** keep the **Default VPC** (or the project VPC).
+ **Public access:** **No** (the backend runs in Lambda/VPC).
+ **VPC security group:** keep the **default** security group.
+ Leave **RDS Data API** off (the backend uses JDBC).

**Additional configuration (expand)**
+ **Tags (optional):** add a tag such as `Project=EDMS`.
+ **Monitoring:** enable **Database Insights – Standard** (free) and optionally **Enhanced Monitoring**.
+ **Log exports:** optionally select **Error log** and **Slow query log**.
+ **Delete protection:** leave it **on** during development; remember to disable it before deleting the cluster in cleanup (5.5.7).

3. Review the **Estimated monthly costs** (this depends heavily on the instance class and storage), then click **Create database**.

> **Note:** Aurora uses a **cluster** that can contain one or more instances. A single instance is enough for this workshop; you can add a read replica later if you need higher read throughput.

### 5.3.2.3 Wait for availability

The cluster takes several minutes to provision. 

1. Return to the **Databases** list.
2. Find `edms` and watch its **Status**.
3. Wait until the status changes from *Creating* to **Available**.
4. (Optional) Click the cluster name to see its writer and reader endpoints.

![Figure 6. Cluster available](../../../images/5-Workshop/5.3-Edms-infrastructure/aurora-available.png)

> **Note:** Do not proceed until the cluster shows **Available** — the endpoint is not usable before that.

### 5.3.2.4 Create the initial database

Amazon Aurora creates a default database. The application needs a dedicated database named `edms`.

1. Open the **Query Editor v2** (in the RDS console) or connect with a MySQL client.
2. Run the following SQL to create the database used by the application:

```sql
CREATE DATABASE IF NOT EXISTS edms
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

3. Verify it exists:

```sql
SHOW DATABASES;
```

> **Note:** The backend applies the schema automatically with **Flyway** migrations from `backend/src/main/resources/db/migration`, so you only need to create the empty database here.

### 5.3.2.5 Retrieve the endpoint

1. In the **Databases** list, click `edms`.
2. Open the **Connectivity & security** tab.
3. Copy the **Endpoint** (hostname) value — for example `edms.cluster-xxxx.ap-southeast-1.rds.amazonaws.com`.

![Figure 7. Endpoint](../../../images/5-Workshop/5.3-Edms-infrastructure/endpoint.png)

4. Save the endpoint, database user, and password into the project's `.env` / SAM configuration:

```
AURORA_ENDPOINT=<endpoint>
DB_USER_AWS=admin
DB_PASS_AWS=<password>
DB_NAME=edms
```

> **Cost note:** Aurora charges even when idle. Stop or delete the cluster when you finish the workshop (see 5.5.7) to avoid ongoing costs.
