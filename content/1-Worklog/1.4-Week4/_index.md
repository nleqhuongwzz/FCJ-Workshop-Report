---
title: "Worklog Week 4"
date: 2026-07-24
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:
- Research, analyze, and select a suitable practical project topic to comprehensively integrate and apply foundational AWS skills accumulated from previous weeks.
- Build and design a preliminary Serverless architecture, and thoroughly research to accurately identify the core services and technologies constituting the system for EDMS (Enterprise Document Collaboration Platform).

### Tasks to be carried out this week:

| Day | Detailed Tasks | Start Date | Completion Date |
| :---: | :--- | :---: | :---: |
| 1 | Project Ideation<br>- Surveyed practical problems regarding internal document management and collaboration in small and medium enterprises (SMEs).<br>- Reached a consensus to build the Enterprise Document Collaboration Platform (EDMS) using a modern Serverless model on AWS to optimize costs and auto-scale based on load. | 12/07/2026 | 12/07/2026 |
| 2 | Technology & AWS Services Selection<br>- Chlected Amazon Cognito (authentication & group authorization for ADMIN/MANAGER/USER), Amazon S3 (original file storage via pre-signed URLs), and Aurora MySQL (relational metadata storage via Spring Data JPA).<br>- Integrated AWS Lambda to run a Spring Boot Monolith application (Java 17, fat-jar) via Amazon API Gateway (REST API) and AWS Step Functions to orchestrate the approval workflow. | 13/07/2026 | 13/07/2026 |
| 3 | System Architecture Design<br>- Outlined an overall Cloud-Native Serverless architecture diagram clearly separating the Frontend tier (React 18 on AWS Amplify), Backend tier (Lambda), and Database tier (Aurora MySQL inside a VPC).<br>- Planned task allocation, Infrastructure as Code workflows using AWS SAM, and automated CI/CD via GitHub Actions (secured through OIDC). | 14/07/2026 | 14/07/2026 |
| 4 | Event Processing & Approval Workflow Research<br>- Designed the document approval workflow using AWS Step Functions with the waitForTaskToken pattern (supporting human-in-the-loop business processes with unlimited waiting time).<br>- Integrated Amazon SNS to automatically send email notifications for approval results (approve/reject).<br>- Defined granular document permission mechanisms by access level (OWNER, EDITOR, VIEWER). | 15/07/2026 | 15/07/2026 |
| 5 | Roadmap Finalization, Documentation & Evaluation<br>- Finalized core technical documentation and the Hexagonal Clean Architecture package structure.<br>- Prepared technical materials and prepared for the system architecture review meeting prior to the product realization phase. | 16/07/2026 | 16/07/2026 |

### Week 5 Achievements:
- Completed the selection of the EDMS (Enterprise Document Collaboration Platform) topic and established a detailed deployment plan for the product implementation phase.
- Successfully constructed the system design blueprint utilizing optimal AWS services (API Gateway, Lambda, Aurora MySQL, S3, Cognito, Step Functions, SNS, Amplify), ensuring seamless auto-scaling and minimized operational costs.