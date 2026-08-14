---
title: "Worklog Week 5"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives
- Design and implement the Aurora MySQL relational database architecture combined with secure object storage on S3.
- Configure centralized user authentication with Amazon Cognito User Pools and group-based authorization (ADMIN/MANAGER/USER).
- Develop core document and folder management APIs (CRUD) using Java 17 (Spring Boot monolith) on AWS Lambda.
- Implement document version control, rollback functionality, and fine-grained access control (`OWNER`, `EDITOR`, `VIEWER`).

### Tasks to be carried out this week:
| Day | Detailed Tasks | Start Date | Completion Date |
| :---: | :--- | :---: | :---: |
| 1 | Aurora MySQL Database Schema Design<br>- Designed a normalized schema to store relational metadata: Users, Departments, Documents, Versions, Folders, Permissions, Tags, Shares, ApprovalHistory, AuditLog, and OcrResult (using Spring Data JPA / Hibernate and Flyway migration). | 19/07/2026 | 19/07/2026 |
| 2 | Identity Management with Cognito & S3 Storage<br>- Integrated Amazon Cognito User Pool to handle user login, JWT authentication, and user grouping (`ADMIN`/`MANAGER`/`USER`).<br>- Configured an S3 bucket to store physical files securely using Pre-signed URLs without exposing credentials. | 20/07/2026 | 20/07/2026 |
| 3 | Core API Development & Hexagonal Architecture<br>- Programmed the Spring Boot backend (Java 17, fat-jar) following Hexagonal Architecture (Ports & Adapters) with `api/controller`, `application/service`, `domain`, and `infrastructure` packages.<br>- Integrated `StreamLambdaHandler` to handle incoming events from Amazon API Gateway and Step Functions. | 21/07/2026 | 21/07/2026 |
| 4 | Version Control & Role-Based Access Control (RBAC)<br>- Implemented document versioning logic (version + rollback) and tagging features.<br>- Finalized the document permission module based on access levels (`OWNER`, `EDITOR`, `VIEWER`) combined with Cognito role-based `@PreAuthorize` API security mechanisms. | 22/07/2026 | 22/07/2026 |
| 5 | Unit Testing & Module Finalization<br>- Wrote unit and integration tests using JUnit 5 + Mockito (MVC Test) to ensure API workflows operate stably across local (`mysql`) and AWS (`aws`) configuration profiles. | 23/07/2026 | 23/07/2026 |

### Week 5 Achievements:
- Completed a fully normalized Aurora MySQL schema for all metadata and established a robust security perimeter integrating Cognito JWT with S3 Pre-signed URLs.
- Successfully built and deployed a Spring Boot monolith running on AWS Lambda, meeting all standards for Hexagonal architecture, version control, and granular access management.