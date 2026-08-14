---
title : "Prepare Source Code and Project Structure"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

EDMS is stored in a Git repository with a **backend** (Spring Boot monolith packaged as a single Lambda), a **frontend** (React), and a **CI/CD** workflow. In this section you clone the source code, inspect its layout, and verify the backend compiles locally.

### 5.4.1.1 Clone the repository

Open a terminal and clone your fork of the project:

```bash
git clone https://github.com/<your-account>/Enterprise-Document-Collaboration-Platform.git
cd Enterprise-Document-Collaboration-Platform
```

Replace `<your-account>` with your GitHub username. Confirm the clone succeeded by listing the top-level files:

```bash
ls -la
```

### 5.4.1.2 Inspect the project structure

The repository layout is organised so that the backend, frontend, and deployment tooling live side by side:

```
Enterprise-Document-Collaboration-Platform/
├── backend/
│   ├── pom.xml                    # Maven build (Java 17)
│   ├── template.yaml              # AWS SAM - infrastructure as code
│   ├── src/main/java/com/edms/    # Spring Boot backend
│   └── src/main/resources/        # config + Flyway migrations + seed data
├── frontend/
│   └── src/                       # React 18 SPA
├── .github/workflows/deploy.yml   # CI/CD pipeline
└── .env                           # local-only config (gitignored)
```


Key files to remember:

+ `backend/pom.xml` — declares the Maven build for **Java 17** and produces the fat jar.
+ `backend/template.yaml` — the **AWS SAM** template that defines all AWS resources (Lambda, API Gateway, Step Functions, SNS).
+ `.github/workflows/deploy.yml` — the **CI/CD pipeline** that runs on GitHub Actions.
+ `.env` — local configuration only; it is **gitignored** and never uploaded.

### 5.4.1.3 Build the backend fat jar locally

Verify the backend compiles before touching anything in AWS. From the repository root:

```bash
cd backend
mvn clean package -DskipTests
```

This produces a single executable **fat jar** under `backend/target/`:

```
backend/target/backend-java-1.0.0-SNAPSHOT.jar
```

The jar name is important: it is the artifact that AWS SAM packages and deploys as a Lambda function.

### 5.4.1.4 Understand how the monolith maps to Lambda

The backend is a **monolith**: one Spring Boot application that contains the REST controllers, services, repositories, and the approval-logic handler. It is not split into many small functions. Instead, the whole application is wrapped by a single Lambda **handler** (`StreamLambdaHandler` or a `RequestHandler`) that AWS SAM attaches to API Gateway.

> **Note:** This "serverless monolith" trade-off is intentional for the workshop. It keeps the deployment simple (one Lambda) while still giving you a single source of truth for the backend. `template.yaml` defines how the jar is turned into a Lambda function.

