---
title: "Blog 3"
date: 2026-08-14
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Exploring the "Read Code to Produce Docs" Multi-Agent Model: Amazon Bedrock AgentCore and MCP

If there is one job that 99% of software engineers or data engineers dread, it is writing and updating documentation. The code has been refactored to a new version, the API has gained 5 new parameters, but the Wiki on Confluence or Notion still stops at the story of... last year. Outdated documentation is more dangerous than having no documentation, because it directly wastes the time of new members and causes countless misunderstandings when handing over the system.

Recently, I spent time exploring a very interesting solution from the AWS ecosystem: using a **Multi-Agent** architecture combined with **Amazon Bedrock AgentCore** and the **MCP** protocol to automatically create and maintain technical documentation in real time.

## 1. WHY THIS PROBLEM NEEDS MULTI-AGENT?

If you just use a regular AI Chatbot and throw the whole code folder into it, you will get a very generic summary that often misses context. To create a proper set of documentation for a business, the system needs to divide the work among specialized agents:

- **Code Analyzer Agent**: Reads the folder structure, analyzes function flow, and extracts API/Data Pipeline endpoints.
- **Architecture Diagram Agent**: Automatically reads the infrastructure/code structure to redraw flow diagrams (Flowchart, Sequence Diagram) in Mermaid.js format.
- **Technical Writer Agent**: Aggregates information and rewrites it in a standard, easy-to-understand style for both Developers and Product Owners.
- **Doc Sync Agent**: Compares existing documentation with the latest code in the Pull Request to update only the parts that actually changed (Delta update).

## 2. HIGHLIGHTS FROM AMAZON BEDROCK AGENTCORE

When I dug deeper into the implementation on AWS, I found that these 3 components smoothly solve the technical barriers:

- **Multi-platform connection via AgentCore Gateway (MCP)**: Thanks to the Model Context Protocol (MCP) standard, agents can both read from GitHub/GitLab and write directly to Notion, Confluence, or MkDocs without writing cumbersome integration code.
- **Context memory thanks to AgentCore Memory**: The AI does not rewrite the documentation from scratch on every new commit. It remembers the old document structure and only adds/edits the parts of the code that actually changed.
- **Absolute safety for the Codebase**: The entire code-reading process happens in the isolated environment of the Bedrock Runtime. The company's internal code is completely not leaked or used to train public models.

## 3. PERSPECTIVES GAINED FROM EXPLORING THIS TOPIC

What I like most about this model is that it completely renews the mindset of doing documentation: from Static Documentation to Living Documentation.

Documentation now becomes part of the CI/CD Pipeline. When a Developer merges code (Merge PR), the AI Agent automatically runs in the background, reviews the changes, and sends a Pull Request to update the corresponding README.md file or Confluence page. The developer just clicks "Approve" and it's done.

## REFERENCES

- AWS Agentic Workflows: Amazon Web Services (2025). _Building Living Documentation Pipelines using Amazon Bedrock AgentCore._ AWS Architecture Center.
- Protocol & Framework: Anthropic (2024). _Model Context Protocol (MCP): Connecting AI Models to Enterprise Knowledge Bases & Developer Tools._
- LangChain / LangGraph Docs. _Multi-Agent Orchestration for Code Graph Analysis._

Ho Chi Minh City, August 2026
Huynh Minh Quan

[Blog link at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/multi_permalinks/2234417337323226/)
