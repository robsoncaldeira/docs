# BOOTSTRAP.md — Graventum Business Context

> This file is permanent reference. Do NOT delete it.
> Loaded every session to provide business context without repeating in prompts.

## The Company

**Graventum** = AI-powered automation for Brazilian SMBs
- Website: https://www.graventum.com
- Email: contato@graventum.com
- Founder: Robson Caldeira (solo operator)
- Phase: Early traction — seeking first paying clients

## Target Niches (Priority Order)

1. **Accounting firms** — tax automation, document classification, operational efficiency
2. **Real estate agencies** — lead qualification, automated follow-up, intelligent CRM
3. **Clinics/healthcare** — AI scheduling, no-show reduction, WhatsApp patient support
4. **Agencias de turismo** — 


## Tech Stack

| Layer | Technology | Role |
|-------|-----------|------|
| LLM | Claude Sonnet 4 (Anthropic) | Reasoning, classification, text generation |
| Orchestration | n8n self-hosted (Hetzner) | Workflow engine, triggers, integrations |
| Database | Supabase (PostgreSQL + pgvector) | Conversations, profiles, metrics, RAG |
| WhatsApp | Evolution API (Docker) | Message send/receive |
| API | Express.js (graventum-api) | Reusable AI service, agent routes |
| Embeddings | OpenAI text-embedding-3-small | Vectorization for RAG |
| Frontend | Next.js 14 + Vercel | Website SSG/SSR |
| Corporate Email | Microsoft Exchange (@graventum.com) | Business communications |
| Personal Email | Gmail | Personal communications |
| Files | OneDrive (1TB E5) + Google Drive | Document storage |
| Server | Hetzner Cloud (Ubuntu 24.04, 8GB) — 46.225.79.76 | n8n, Evolution API, graventum-api |

## What Graventum Delivers Today

1. Complete marketing campaign generation (3 variations + landing page)
2. Programmatic corporate email sending
3. Automatic new lead alerts
4. AI-powered email classification
5. Automated client onboarding (diagnosis + roadmap + scheduling)
6. 10 ready-made automation workflows (71 nodes)

## Business Model

- **Diagnosis** → Implementation → **Monthly retainer**
- Target margin: 93-97%
- Recurring revenue via retainers
- Differentiator: fast implementation (15-30 days), proprietary stack, niche specialization

## Repositories

- `graventum.com` → Website + frontend (Next.js, Vercel)
 https://github.com/robsoncaldeira/graventum.com
- `graventum-api` → Central API (Express.js, Hetzner)
 https://github.com/robsoncaldeira/graventum-api
- `graventum-automations` → n8n templates

## Current State

- **Revenue:** Pre-revenue, seeking first paying clients
- **Content:** Needs consistent LinkedIn, Instagram, and online presence
- **SEO:** graventum.com needs regular content for organic traffic
- **Prospecting:** Active via LinkedIn (organic)
- **Goal:** 5-8 active clients in 6 months
