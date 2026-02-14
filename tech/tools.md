# TOOLS.md — Graventum Tools & Integrations

Skills define _how_ tools work. This file is for _your_ specifics — what's unique to this setup.

## Active Infrastructure

### n8n (Automation Engine)
- URL: automation.graventum.com
- 10 active workflows, 71 nodes
- 7 Google credentials + 7 Microsoft credentials configured
- Used for: agent orchestration, triggers, integrations, webhook endpoints

### Graventum API (Express.js)
- Server: 46.225.79.76:3000
- Process manager: pm2 (process name: graventum-api)
- Routes: /health, /gerar-campanha, /microsoft/send-email, /agent/*
- Reusable AI service for all agent interactions

### Supabase (Database + Backend)
- PostgreSQL + pgvector for embeddings
- Auth, Storage, Realtime, Edge Functions
- Row Level Security (RLS) for multi-tenant isolation
- Tables: conversations, leads, automation_metrics, organizations

### Evolution API (WhatsApp)
- Docker container on Hetzner
- WhatsApp Business API (open-source, WHATSAPP-BAILEYS)
- Send/receive messages, webhook integration with n8n

### Microsoft Graph API
- OAuth2 Client Credentials
- Corporate email: contato@graventum.com
- OneDrive: 1TB storage (E5 license)
- Calendar access

### Google Workspace
- Gmail (personal email)
- Google Drive (file storage)
- Google Calendar
- Google Analytics GA4 (graventum.com)
- Google Sheets (client data)

### Hetzner Cloud Server
- Ubuntu 24.04, 8GB RAM
- IP: 46.225.79.76
- Hosts: n8n, Evolution API, graventum-api
- Access: SSH

### Vercel
- Hosts: graventum.com (Next.js 14)
- Auto-deploy from main branch
- PR = preview deployment

### GitHub
- Repos: graventum.com, graventum-api, graventum-automations
- https://github.com/robsoncaldeira/graventum-api
- https://github.com/robsoncaldeira/graventum.com
- Conventional commits: feat:, fix:, docs:, chore:

## Conventions

- **Git:** conventional commits (feat:, fix:, docs:, chore:)
- **Files:** kebab-case naming
- **Code:** English for variables, Portuguese for content
- **Deploy:** Vercel auto-deploy (main = production, PR = preview)
- **Branches:** main = production, dev = development

## Local Notes

_(Add environment-specific details as you discover them: SSH keys, API keys location, service passwords, etc.)_
