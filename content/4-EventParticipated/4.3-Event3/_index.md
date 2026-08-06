---
title: "Event 3"
date: 2026-08-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# SUMMARY REPORT: AGENT FORGE - DEEPDIVE DAY 1

## Event Objectives

- **Objective:** Provide a focused introduction to Amazon Bedrock AgentCore and the foundational components required to build, deploy and secure AI agents on AWS.
- **Learning Focus:** Understand the AgentCore runtime model, gateway integration, identity layer and the first hands-on steps for deploying an agent with external tools, Knowledge Bases, Web UI and Amazon Cognito authentication.

## Event Information

- **Event Name:** Agent Forge - Deepdive Day 1
- **Date & Time:** August 1, 2026, 09:00 - 12:00
- **Location:** Bitexco Financial Tower, Ho Chi Minh City
- **Role:** Attendee
- **Main Topics:** Amazon Bedrock AgentCore, agent runtime, gateway, identity, external tool integration, Knowledge Bases, Web UI and Amazon Cognito authentication

## Speakers

- **Nghia Tran:** Agentic SA. The speaker covered the theory-oriented part of the session, including the overview of Amazon Bedrock AgentCore and the core components used to operate AI agents on AWS.
- **Anh Pham:** Cloud Consultant, G-AsiaPacific VietNam. The speaker supported the practical and cloud implementation perspective, connecting the AgentCore concepts with deployment and integration activities.

## Event Agenda

### Session 1: Introduction to Amazon Bedrock AgentCore

**Time:** 09:00 - 10:00

This session introduced the technical foundation of Amazon Bedrock AgentCore and explained why agent-based applications require more than a model invocation layer. A production-ready agent needs a runtime environment, controlled tool access, identity handling and integration points with external systems.

Key topics included:

- **Workshop Introduction:** Overview of the 3-day AgentForge workshop and the goals of Day 1.
- **Amazon Bedrock AgentCore Overview:** Positioning AgentCore as a platform layer for building and operating AI agents.
- **Runtime:** How an agent is executed, how requests are handled and how agent behavior is organized during runtime.
- **Gateway:** How external tools and services can be exposed to an agent through a managed integration layer.
- **Identity:** How authentication and identity context are important when an agent interacts with user-specific data or protected business systems.

### Session 2: Hands-on Lab

**Time:** 10:00 - 11:00

The hands-on part focused on the first implementation flow for an agent-based application. The lab was designed to connect the theoretical components from the first session with a working implementation path.

Main lab activities:

- **Deploy a basic agent in AgentCore:** Create and run an initial agent to understand the deployment flow.
- **Connect external tools and Knowledge Bases:** Extend the agent so it can retrieve information and perform actions through connected resources.
- **Build a Web UI:** Provide a basic interface for users to interact with the agent.
- **Integrate Amazon Cognito authentication:** Add an authentication layer so the application can manage user access in a more controlled way.

## Key Highlights

### AgentCore as an Operational Layer for AI Agents

The session clarified that an AI agent should not be treated as only a prompt connected to a foundation model. A useful agent system needs supporting infrastructure around execution, integration, authentication and observability. Amazon Bedrock AgentCore addresses this direction by providing components that help standardize how agents are built and operated.

### Runtime, Gateway and Identity as Core Building Blocks

Three concepts were especially important:

- **Runtime:** Defines how the agent runs and processes requests.
- **Gateway:** Provides a controlled mechanism for connecting tools and external systems.
- **Identity:** Ensures that agent actions can be associated with proper user context and access control.

These components are important because enterprise AI applications often need to call internal tools, retrieve company knowledge and enforce user-level authorization.

### From Prototype to Controlled Application

The lab showed the difference between a simple AI demo and a more controlled agent application. Adding a Web UI and Amazon Cognito authentication made the system closer to a real user-facing application, where access control and identity management are required from the beginning.

## Main Learning Points

### Agentic Applications Need Architecture Discipline

Building an agent is not only about choosing a capable model. The surrounding architecture must define what the agent can access, how it authenticates users, how tools are exposed and how knowledge sources are managed.

### Tool Access Must Be Designed Carefully

When an agent can call external tools, the design should clearly define allowed actions, security boundaries and expected outputs. Without a controlled gateway and identity layer, tool integration can become difficult to audit and secure.

### Knowledge Bases Improve Context, but Require Governance

Connecting Knowledge Bases helps the agent answer with domain-specific context. However, the quality of the response depends on source quality, retrieval design and permission boundaries around the stored knowledge.

### Authentication Is Part of the Agent Design

Amazon Cognito integration highlighted that user identity is not an optional feature for practical applications. If an agent interacts with user-specific data, identity and authorization should be included early in the architecture.

## Comparative Discussion

| Criteria | Simple LLM Chatbot | AgentCore-based Agent Application |
| :--- | :--- | :--- |
| **Execution Model** | Mainly prompt-response interaction. | Structured runtime for agent execution and orchestration. |
| **Tool Integration** | Often hardcoded or manually connected. | Integrated through a controlled gateway layer. |
| **Knowledge Access** | Depends mostly on model knowledge or static prompt context. | Can connect to Knowledge Bases for domain-specific retrieval. |
| **Identity Handling** | Often missing in prototypes. | Designed with authentication and user context through services such as Amazon Cognito. |
| **Enterprise Readiness** | Suitable for demonstration and exploration. | More suitable for controlled application development with security and integration requirements. |

## Event Experience

- **Practical Experience:** The event connected theory and hands-on practice in a clear sequence: understand AgentCore concepts first, then deploy an agent and connect it with tools, Knowledge Bases, Web UI and authentication.
- **Technical Experience:** The most valuable part was seeing how runtime, gateway and identity work together as building blocks for a secure agent architecture.
- **Community Value:** The session provided a practical starting point for participants who want to move from basic generative AI usage to more structured agentic application development on AWS.
- **Lesson Learned:** Agent architecture should be designed with security, identity, integration boundaries and operational control from the first stage, especially when the agent can access external tools or business data.

## Applying to Work

- Treat AI agents as cloud applications that require architecture design, not only as model prompts.
- Define tool permissions and access boundaries before allowing an agent to perform actions.
- Use Knowledge Bases when the agent needs controlled domain-specific context.
- Include authentication and identity management early when designing user-facing agent applications.
- Evaluate agent systems through both functionality and operational concerns such as security, governance and maintainability.

## Event Evidence

![Agent Forge Deepdive Day 1 attendance evidence](/images/4-EventParticipated/event3-agent-forge-day1.jpg)
