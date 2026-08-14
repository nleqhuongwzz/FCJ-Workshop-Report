---
title: "Event 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: “FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!”

### Event Objectives

- Summarize and award prizes for the "Agentic AI Build Week" Hackathon.
- Create a hands-on playground under high pressure for developers, students, and experts to brainstorm, build architectures, and demo technology products applying Agentic AI.
- Inspire and orient product-making mindsets in the artificial intelligence era through advice from speakers.
- Provide opportunities for competing teams to pitch their solutions directly to a professional judging panel, proving the feasibility of technology in solving real-world business pain points.

### List of Speakers and Teams

- **Giuseppe Marazzotta** - Head of Tech & Solution Architecture, ASEAN
- **One Team (1st Prize)** - Automated Food Ordering AI Chatbot Project.
- **Signal Scout (2nd Prize - FPT Student Group)** - Multi-Agent Competitor Strategy Analysis Project.
- **Team Plan** - Cloud Infrastructure Design AI Assistant Project.
- **Team 3K** - Crowd Tracking AI Computer Vision Project (Sheper).
- **Team Six Pillars** - Anti-Money Laundering (AML) AI Workflow Project for Banks.

### Key Highlights

#### Technology Vision from AWS Leadership

- The shift in Release speed: If 20 years ago banking systems needed a quarter to release a product, in the era of AI Agents, releases can happen by the minute.
- The advantage of the younger generation: Young developers are not hindered by legacy "technical debt". AI and hardware (like Amazon's autonomous robot fleets) are merely lifeless tools without "Human-in-the-loop" – young engineers who can orient, evaluate, and grant permissions for AI to operate.

#### Breakthrough AI Solutions from Competing Teams

- **Food Ordering System via Zalo with AI Agent (One Team)**: Solved the "must download App" barrier when ordering. Customers simply chat naturally on Zalo/WhatsApp, and the AI (AWS Bedrock Agent Core) automatically scrapes the menu, remembers order history, processes the cart, and applies promotions. Infrastructure cost was optimized to an extremely low level (~$0.006 USD/order).
- **Multi-Agent Enterprise Analysis (Signal Scout)**: A crawler system using Tiny Fish and AWS Amplify to bypass Login Walls. The system automatically gathers scattered data about competitor structures and finances, then scores it using Langfuse and produces strategic consulting reports.
- **Cloud Architecture Design Assistant (Team Plan)**: Helps Solutions Architects (SA) automate drawing architecture diagrams. From Natural Language requests or documents, the AI draws the architecture on Draw.io, calculates pricing, and generates Infrastructure as Code (IaC - Terraform) standardized according to company policy.
- **Crowd Tracking AI Camera (Team 3K)**: Applied Yolo v26 combined with ByteTrack to recognize and track crowds in real-time. Video data is pushed through Kinesis Video Streams and analyzed by AI to calculate density and wait times, thereby automatically dispatching staff to congested areas in supermarkets or airports.
- **Anti-Money Laundering System - AML (Six Pillars)**: Solved the 90-95% False Positive transaction alerts in banks. Combined Machine Learning XGBoost for quick transaction classification and 3 Sub-agents (KYC Check, Money Flow Check, Sanction Check) to automate profile investigation. The output is an Evidence File for humans to make the final decision.

### What I Learned

#### Regarding Product & Startup Mindset
- **Start with the "Pain Point"**: No matter how complex the technology is, it's meaningless if it doesn't solve a practical problem. Don't just focus on frameworks/code; be able to answer: "Who is this product for and what problem does it solve?".
- **Scope Down for MVP**: In short-term projects, controlling the scope is a matter of survival. Don't build a massive but buggy system; focus on a core scenario (MVP) that can be demoed smoothly from start to finish.

#### Regarding Soft Skills & Teamwork
**The power of Teamwork under high pressure:** Working continuously for 24 hours requires members to put aside their "egos", clearly divide roles (Front-end, Back-end, Pitching, UI/UX), and completely trust each other's decisions.

**Pitching Skills:** Presenting a project should not only revolve around technology architecture but must also highlight the Business Model, Operational Costs, and Security.

#### Regarding Cloud & AI Architecture
- Understood how to combine traditional logic flows (Rule-based) and artificial intelligence (LLM/Agents) to minimize AI "Hallucination".
- Instead of building everything from scratch, utilizing Cloud Managed Services (Kinesis, DynamoDB, Bedrock, Cognito) accelerates project development many times over.

### Applications to Work / Internship Project

- **Applying the MVP strategy to Projects**: Don't ambitiously cram in too many features. Focus on building a stable core business flow with a visual Demo before researching auxiliary technologies.
- **Data Modeling and LLM Integration**: Applied the Six Pillars team's method to AI projects — using AI to generate a summarized "Evidence File/Report" to assist user decision-making, rather than letting AI automatically execute sensitive tasks related to the database.
- **Infrastructure Automation (IaC)**: Inspired by Team Plan, researched more about Terraform and AWS CloudFormation to automate infrastructure deployment for internship projects instead of manual setup, saving time and easing rollbacks.
- **Practicing Pitching Skills**: Prepare response scripts related to the applicability, security, and cost of the system — classic questions frequently asked by project evaluation panels/recruiters.

### Personal Experience at the Event

- Following the **Agentic AI Build Week** closing event brought me immense respect for young builders. Memorable shared experiences like: staying up until 4 AM, arguing fiercely over architectural disagreements, accidentally pushing `.env` files to GitHub, or the camera losing network connection right during the demo... are all invaluable combat experiences that books cannot teach.
- The most inspiring thing was the **Show Up. Build. Pitch.** spirit — just step up, register, and figure the rest out later. All fears about "not being skilled enough" are erased when you are put in an environment that forces you to think and break limits. Through the presentations, I not only gained countless new insights on designing Multi-Agents or handling Real-time Video Streams but also promised myself to step out of my comfort zone and participate in at least one Hackathon in the near future to gain experience and expand my own network.

#### Some photos from the event
![Event Image](../../images/4-Event/event3-1.jpg)
![Event Image](../../images/4-Event/event3-2.jpg)

> Overall, the event not only provided technical knowledge but also helped me reshape my thinking about application design, system modernization, and cross-team collaboration.