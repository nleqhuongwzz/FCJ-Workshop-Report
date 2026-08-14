---
title: "Worklog Week 7"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

## Week 7 Objectives:
- Set up an end-to-end automated CI/CD pipeline using GitHub Actions combined with AWS SAM and AWS Amplify.
- Deploy the React 18 SPA frontend to AWS Amplify Hosting.
- Review IAM permission policies based on the least-privilege principle and optimize system security.

## Tasks to be carried out this week:
| Day | Detailed Tasks | Start Date | Completion Date |
| :---: | :--- | :---: | :---: |
| 1 | Set up CI/CD Pipeline (GitHub Actions)<br>- Configured automation workflow  to run backend unit tests.<br>- Integrated OIDC assume role for secure authentication with AWS without hard-coding static AWS keys. | 03/08/2026 | 04/08/2026 |
| 2 | Deploy Frontend to AWS Amplify<br>- Built the React 18 SPA interface integrating Amazon Cognito Identity SDK, React Router, and Axios.<br>- Automated the build and deployment of the frontend to AWS Amplify. | 05/08/2026 | 05/08/2026 |
| 3 | Automate Backend Deployment via SAM<br>- Integrated the `sam deploy` command step into the CI/CD pipeline to automatically update the Lambda monolith (Spring Boot fat-jar) build to the Production environment. | 06/08/2026 | 07/08/2026 |
| 4 | Test VPC & Aurora MySQL Connectivity<br>- Configured Lambda to run inside a VPC sharing the same Security Group as the Aurora MySQL cluster to ensure absolute secure JDBC connectivity. | 08/08/2026 | 08/08/2026 |
| 5 | Overall Pipeline Testing & Evaluation<br>- Performed end-to-end testing of the CI/CD pipeline from code commit to successful update on the API Gateway endpoint. | 09/08/2026 | 09/08/2026 |

## Week 7 Achievements:
- Successfully built an automated pipeline via GitHub Actions, enabling continuous and secure testing and deployment of both backend and frontend code changes to AWS.
- The entire EDMS platform is now running stably on real AWS services complete with the API Gateway endpoint and Amplify interface.