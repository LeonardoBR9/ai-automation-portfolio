# AI Automation Portfolio

**English** | [Português](README.pt-BR.md)

## Profile

**Leonardo Lima Chaves**

Specialties: AI automation, conversational AI agents, n8n, messaging/WhatsApp integrations, RAG, and automation operations.

## Summary

This repository presents small, didactic examples of AI-driven automation built with n8n: conversational agents, retrieval-augmented answers, message routing, scheduling logic and reminders. The examples are intentionally generic and use fictional data. They illustrate ideas, not production systems.

## What this portfolio demonstrates

- An AI agent in n8n with a chat model, short-term conversation memory and a simple in-memory knowledge base (RAG).
- Receiving messages through a generic webhook, normalizing them and replying over HTTP.
- Routing messages by topic and branching on a simulated availability check.
- A scheduled branch that produces reminders.

## Stack and concepts

- **Used in these examples:** n8n (native nodes only), AI Agent, OpenAI chat and embeddings models, window buffer memory, in-memory vector store, Webhook, Schedule Trigger, Switch, IF, Set, Wait, HTTP Request.
- **Concepts:** conversational agents, RAG, message normalization, intent routing, scheduling, operational safety of automations.
- **Context:** production versions of flows like these use messaging gateways (for example Evolution API or WAHA), Postgres/Supabase with pgvector, Redis and Chatwoot, among others. They are not included here.

## Examples

### 01 — Support Agent with Simple RAG

Goal: answer customer questions using a small knowledge base and conversation memory.
Flow: webhook → normalize → short pause → AI agent (model + memory + vector store tool) → reply.
Files: [`workflows/01-support-agent-example.json`](workflows/01-support-agent-example.json) · [`workflows/01-support-agent-example.md`](workflows/01-support-agent-example.md)

### 02 — Enrollment and Visit Scheduling

Goal: route messages, collect enrollment details, check a simulated slot and confirm, plus a reminder branch.
Flow: webhook → normalize → switch → AI agent → simulated check → confirm or offer alternatives; schedule trigger → reminder.
Files: [`workflows/02-enrollment-scheduling-example.json`](workflows/02-enrollment-scheduling-example.json) · [`workflows/02-enrollment-scheduling-example.md`](workflows/02-enrollment-scheduling-example.md)

## How to import and use safely

1. In n8n choose **Workflows → Import from file** and select a `.json` from `workflows/`.
2. Add your own credentials (for example your own OpenAI key). No credentials or secrets are included.
3. Keep workflows inactive until you reviewed them; test with the webhook test URL.
4. Replace every placeholder URL, and never expose test webhooks publicly without authentication.

## Full versions

Full versions available on request.

## Rights

All rights reserved. For portfolio viewing only. No license is granted for reuse, redistribution, commercial use, or derivative works.

## Contact

- Email: contato@proll.ia.br
- LinkedIn: https://www.linkedin.com/in/leonardolc99
- Website: https://proll.ia.br
- GitHub: https://github.com/LeonardoBR9
