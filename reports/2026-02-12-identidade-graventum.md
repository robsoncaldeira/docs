# Relatório: Identidade, Modelo de Negócio e Proposta de Valor da Graventum

**Data:** 12/02/2026 | **Autor:** Helix (COO Virtual)

---

## 1. O QUE ESTÁ SENDO CONSTRUÍDO

### Descrição objetiva

A Graventum é uma **consultoria de automação e inteligência artificial focada em operações de PMEs brasileiras**. Não é uma plataforma SaaS. Não é uma software house. Não é uma agência de marketing.

É uma **empresa de serviços especializados** que diagnostica ineficiências operacionais em empresas pequenas e médias, e implementa soluções sob medida usando IA e automação — resolvendo problemas que essas empresas nem sabiam que tinham nome.

A melhor definição: **Consultoria operacional com implementação técnica**. Ou, de forma mais direta: **um departamento de eficiência terceirizado**.

### O que NÃO é a Graventum

| Modelo | Por que NÃO é isso |
|--------|-------------------|
| **SaaS/Plataforma** | Não existe um software único que o cliente acessa. Cada entrega é customizada para o contexto do cliente. |
| **Software House** | Não desenvolvemos aplicativos ou sistemas do zero. Orquestramos ferramentas existentes (n8n, WhatsApp, IA, APIs). |
| **Agência de Marketing** | Não vendemos posts, tráfego pago ou branding. O foco é operação interna, não aquisição de clientes. |
| **Consultoria tradicional** | Não entregamos PowerPoint com recomendações. Entregamos automações funcionando. |

### O que MELHOR descreve

**"Boutique de automação inteligente"** — uma empresa que combina:
- Diagnóstico consultivo (entender o problema)
- Engenharia de automação (construir a solução)
- IA aplicada (classificação, geração, análise)
- Operação continuada (manter, monitorar, evoluir)

O modelo mais próximo no mercado seria o de uma **MSP (Managed Service Provider) de automação e IA**, adaptado para PMEs brasileiras.

---

## 2. O QUE VAMOS VENDER

### Produto principal: Tempo de volta

O cliente não compra "automação" ou "IA". Ele compra **horas de volta**, **erros eliminados**, **processos que funcionam sozinhos** e **paz de espírito**. A tecnologia é o meio, não o produto.

### Os 3 entregáveis concretos

**A) Diagnóstico Operacional (entrada)**
- Sprint de 1-2 semanas
- Mapeamento de processos, gargalos, retrabalho
- Roadmap priorizado por impacto
- **Preço sugerido:** R$ 1.500-3.000 (ou gratuito como estratégia de entrada)

**B) Implementação de Automações (projeto)**
- Construção de workflows customizados
- Integração com ferramentas do cliente (WhatsApp, Drive, email, ERP)
- Copilotos de IA para atendimento ou operações
- Setup completo em 15-30 dias
- **Preço sugerido:** R$ 3.000-8.000 por projeto (ou incluso no trial)

**C) Retainer Mensal (receita recorrente)**
- Manutenção e monitoramento das automações
- Suporte dedicado
- Melhorias contínuas (1-2 novas automações/mês)
- Relatórios de métricas
- **Preço sugerido:** R$ 997-2.497/mês dependendo do escopo

### Exemplos concretos do que entregamos

| Entrega | Para quem | Resultado |
|---------|-----------|-----------|
| Bot WhatsApp que classifica documentos com IA | Contabilidade | -4h/semana de trabalho manual |
| Lembrete automático de vencimentos | Contabilidade | Zero multas por esquecimento |
| Qualificação automática de leads | Imobiliária | Resposta em 2min ao invés de 2h |
| Confirmação e lembrete de consultas | Clínica | -80% de no-shows |
| Onboarding automático de clientes | Qualquer PME | -3h por cliente novo |
| Classificador inteligente de emails | Qualquer PME | Inbox zero automático |
| Dashboard de saúde do cliente | Qualquer PME | Visão em tempo real do pipeline |

---

## 3. COMO VAMOS ATENDER O CLIENTE

### A jornada completa do cliente

```
ATRAÇÃO → DIAGNÓSTICO → TRIAL → IMPLEMENTAÇÃO → RETAINER → EXPANSÃO
```

**Fase 1: Atração (o cliente nos encontra)**
- LinkedIn (conteúdo de nicho)
- Indicação de clientes existentes
- Site + SEO
- Ferramenta gratuita (calculadora de tempo perdido, diagnóstico automatizado)

**Fase 2: Diagnóstico (1-2 semanas)**
- Reunião inicial (30-60min via Google Meet/Teams)
- Mapeamento dos processos atuais
- Identificação de gargalos e oportunidades
- Entrega: roadmap com 3-5 automações priorizadas
- **Ferramenta:** Google Meet + Notion/Doc + análise manual

**Fase 3: Trial (14 dias)**
- Implementação da automação #1 (a de maior impacto)
- Cliente testa com dados reais
- Acompanhamento diário dos resultados
- **Ferramentas:** n8n, Evolution API (WhatsApp), Supabase, Claude

**Fase 4: Implementação completa (15-30 dias)**
- Deploy das automações restantes do roadmap
- Integração com ferramentas do cliente
- Treinamento do time
- **Ferramentas:** n8n, graventum-api, Supabase, Evolution API

**Fase 5: Retainer mensal (ongoing)**
- Monitoramento de todas as automações
- Suporte via WhatsApp (SLA 4h úteis)
- 1-2 novas micro-automações por mês
- Relatório mensal de métricas
- Revisão trimestral de processos
- **Ferramentas:** n8n (monitoramento), Supabase (métricas), WhatsApp (suporte)

**Fase 6: Expansão**
- Novos departamentos do mesmo cliente
- Automações mais complexas (IA conversacional, análise preditiva)
- Upsell natural conforme confiança cresce

### Stack de ferramentas por fase da jornada

| Fase | Ferramentas |
|------|------------|
| **Atração/Marketing** | LinkedIn, Instagram, graventum.com (Next.js/Vercel), Google Analytics |
| **Comercial/Vendas** | WhatsApp (Evolution API), email (Microsoft Exchange), Supabase (CRM) |
| **Diagnóstico** | Google Meet, Google Docs, templates internos |
| **Implementação** | n8n (orquestração), Claude/OpenAI (IA), Supabase (banco), Evolution API (WhatsApp), Google Drive/OneDrive (arquivos) |
| **Operação/Retainer** | n8n (monitoramento), Supabase (métricas), graventum-api (serviço central), WhatsApp (suporte) |
| **Interno/Backoffice** | GitHub (código), Hetzner (servidor), OpenClaw (assistente IA), Microsoft 365 |

---

## 4. O QUE O CLIENTE BUSCA RESOLVER

### A dor real (o que o cliente SENTE)

O dono de uma PME não acorda pensando "preciso de automação com IA". Ele acorda pensando:

- **"Meu time gasta tempo demais em coisas burras"** — tarefas repetitivas que não exigem cérebro
- **"A gente perde cliente por demora"** — leads esfriam, follow-up não acontece
- **"Eu sou o gargalo"** — tudo passa por mim, nada anda sem eu empurrar
- **"Não sei onde tá cada coisa"** — documentos perdidos, informações espalhadas
- **"Tô pagando gente cara pra fazer trabalho de robô"** — profissionais qualificados fazendo copiar-colar
- **"Esquecemos prazos, perdemos dinheiro"** — multas por atraso, oportunidades perdidas

### A dor por nicho

**Contabilidade:**
- Clientes enviam documentos por WhatsApp sem padrão → tempo enorme organizando
- Vencimentos de obrigações acessórias → risco de multa por esquecimento
- Onboarding de novos clientes → processo manual, demorado, inconsistente
- Comunicação com clientes → reativa, não proativa

**Imobiliárias:**
- Leads dos portais (ZAP, OLX) → resposta lenta = venda perdida
- Corretores odeiam CRM → dados desatualizados
- Follow-up inconsistente → oportunidades morrem no pipeline
- Agendamento de visitas → ida e volta manual por WhatsApp

**Clínicas:**
- No-show de pacientes → 15-30% de consultas perdidas
- Recepção sobrecarregada com telefone → agendamento manual
- Confirmação de consultas → ligação por ligação
- Prontuário/documentação → papelada, falta de organização

**Agências de turismo:**
- Orçamentos manuais → demora pra responder = cliente vai pro concorrente
- Follow-up pós-orçamento → ninguém faz
- Sazonalidade → picos de demanda com equipe fixa
- Documentação de viagem → cobrar passaporte, visto, seguro manualmente

---

## 5. ATIVOS DA EMPRESA

### Ativos técnicos (o que já temos construído)

| Ativo | Descrição | Status |
|-------|-----------|--------|
| **graventum.com** | Site institucional (Next.js 14, Vercel, 3 idiomas, GA4, Clarity) | ✅ Produção |
| **graventum-api** | API central Express.js com auth, IA service, rotas de agente | ✅ Produção |
| **n8n** | 29 workflows (5 ativos), 208 nós, self-hosted Hetzner | ✅ Produção |
| **Supabase** | PostgreSQL + pgvector, auth, storage, RLS multi-tenant | ✅ Configurado |
| **Servidor Hetzner** | Ubuntu 24.04, 8GB RAM, fail2ban, backup automático, SSH | ✅ Produção |
| **Microsoft 365 E5** | Email corporativo, OneDrive 1TB, Calendar, Teams | ✅ Ativo |
| **Google Workspace** | Gmail, Drive, Calendar, Sheets, Analytics | ✅ Ativo |
| **GitHub** | 3 repos (site, API, automações), CI/CD com Vercel | ✅ Ativo |
| **OpenClaw (Helix)** | Assistente IA autônomo (COO virtual), cron jobs, heartbeat | ✅ Ativo |

### Ativos intelectuais (templates e frameworks)

| Ativo | Descrição |
|-------|-----------|
| **Kit Automação Fantasma** | 3 workflows n8n prontos para contabilidade, proposta comercial, sequência de follow-up, roteiro de vídeo demo, estudo de caso |
| **Schema Supabase para contabilidade** | 5 tabelas (clients, documents, tax_obligations, notification_log, scheduled_messages) |
| **Rota classify-document** | API com Claude Vision para classificação automática de documentos contábeis |
| **Proposta comercial template** | Modelo com pricing R$ 1.297/mês, ROI calculado, trial de 14 dias |
| **Análise competitiva** | Mapeamento de Integgri, Zap Contábil, Omie, DropDesk — nenhum faz o que a Graventum faz |
| **11 workflows pré-construídos** | Alerta de lead, registro de reuniões, organização de arquivos, classificação de email, analytics, rastreador de propostas, briefing de reuniões, gerador de campanhas, dashboard de saúde, kickoff automatizado |

### Ativos de marca e presença

| Ativo | Status |
|-------|--------|
| Domínio graventum.com | ✅ |
| Email contato@graventum.com | ✅ |
| LinkedIn company page | ✅ (precisa de conteúdo regular) |
| Instagram @graventum | ✅ (precisa de conteúdo regular) |
| Logo e identidade visual | ✅ |
| Site em 3 idiomas (PT/EN/ES) | ✅ |

---

## 6. O QUE DE FATO VAMOS FAZER

### O trabalho no dia a dia

De forma prática e sem romantizar, o trabalho diário da Graventum se resume a:

**1. Construir automações com n8n**
- Criar workflows visuais que conectam as ferramentas do cliente
- Exemplo: "Quando chega mensagem no WhatsApp com foto → IA classifica → salva no Drive → confirma pro cliente"
- Isso é o core técnico. É onde a magia acontece.

**2. Implementar IA aplicada**
- Claude/GPT para classificar documentos, responder perguntas, gerar textos
- Embeddings para busca semântica em bases de conhecimento do cliente
- Não é chatbot genérico — é IA treinada no contexto específico do negócio

**3. Integrar ferramentas que o cliente já usa**
- WhatsApp (via Evolution API) — o canal mais importante no Brasil
- Google Drive / OneDrive — onde os arquivos já estão
- Email (Gmail / Outlook) — comunicação existente
- ERP/Software do cliente — via API ou webhook
- Não forçamos nenhuma ferramenta nova. Plugamos nas que já existem.

**4. Montar fluxos de comunicação automatizados**
- Lembretes, follow-ups, onboarding, confirmações
- Tudo via WhatsApp (o canal que o cliente brasileiro vive)

**5. Criar dashboards e relatórios automáticos**
- Métricas de operação em tempo real
- Supabase como banco + visualização simples

### O que NÃO vamos fazer (pelo menos agora)

| Atividade | Por quê não |
|-----------|-------------|
| Desenvolver aplicativo mobile | Complexidade alta, custo alto, desnecessário para o valor que entregamos |
| Criar plataforma SaaS | Requer investimento, time de produto, suporte em escala — não combina com fase atual |
| Análise de dados avançada (BI) | Pode ser upsell futuro, mas não é o core |
| Consultoria estratégica pura | Entregamos estratégia COM implementação, nunca só PowerPoint |
| Desenvolvimento web/e-commerce | Fora do escopo — somos operação, não marketing |

---

## 7. QUEM SOMOS E QUAL O NOSSO PROPÓSITO

### Identidade

**Graventum** é uma empresa de uma pessoa (Robson Caldeira) amplificada por tecnologia (Helix como COO virtual, stack de automação como força de trabalho). A proposta é demonstrar na prática o que vendemos: **um operador solo que entrega resultado de equipe porque automatizou inteligentemente**.

### Propósito

**Libertar PMEs brasileiras do trabalho que não deveria ser feito por humanos.**

Não com discurso inspiracional. Com automações que funcionam no dia seguinte.

### Valor que nos propomos a entregar

1. **Tempo** — devolver horas por semana para o dono e equipe focarem no que importa
2. **Confiabilidade** — processos que rodam sem falha humana, sem esquecimento, sem "ops"
3. **Velocidade** — resposta ao cliente em minutos ao invés de horas/dias
4. **Visibilidade** — saber o que está acontecendo na operação em tempo real
5. **Escalabilidade** — crescer sem contratar proporcionalmente

### De que maneira

- **Sob medida, não genérico** — cada implementação é customizada para o negócio
- **Usando o que o cliente já tem** — WhatsApp, Drive, email, ERP existente
- **Com implementação rápida** — 15-30 dias, não 6 meses
- **Com baixo risco** — trial de 14 dias gratuito
- **Com acompanhamento contínuo** — retainer mensal, não projeto e tchau
- **Com IA de verdade** — não chatbot de menu, mas classificação, geração e análise inteligente

---

## 8. RESUMO EXECUTIVO

```
┌─────────────────────────────────────────────────────────────────┐
│                        GRAVENTUM                                 │
│                                                                  │
│  TIPO: Consultoria de automação e IA para PMEs brasileiras       │
│                                                                  │
│  VENDE: Tempo de volta + processos confiáveis                    │
│         (via automações com IA + WhatsApp + ferramentas do       │
│          cliente)                                                │
│                                                                  │
│  MODELO: Diagnóstico → Implementação → Retainer mensal           │
│          (R$ 997-2.497/mês por cliente)                          │
│                                                                  │
│  NICHOS: Contabilidade, Imobiliárias, Clínicas, Ag. Turismo     │
│                                                                  │
│  DIFERENCIAL: Implementação rápida (15-30 dias), stack           │
│               proprietária, IA aplicada (não genérica),          │
│               trial sem risco, operação contínua                 │
│                                                                  │
│  MARGEM: 93-97% (operador solo + automação)                      │
│                                                                  │
│  META: 5-8 clientes em 6 meses = R$ 5.000-20.000/mês           │
│        recorrente                                                │
│                                                                  │
│  STACK: n8n + Claude + WhatsApp (Evolution) + Supabase +         │
│         Next.js + Hetzner + Microsoft 365                        │
│                                                                  │
│  EQUIPE: 1 humano (Robson) + 1 IA (Helix) + automações          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. O QUE FALTA DEFINIR (Pendências estratégicas)

1. **Nicho primário** — Contabilidade é o mais desenvolvido (kit pronto), mas vale validar com o primeiro cliente real
2. **Precificação final** — R$ 1.297/mês é o ponto de partida, mas pode variar por escopo
3. **Primeiro cliente** — Tudo está construído. Falta um escritório/empresa alvo para começar
4. **Presença online** — Site está pronto mas os contadores de "horas economizadas" e "empresas atendidas" estão em zero. Precisamos do primeiro caso real
5. **Conteúdo regular** — LinkedIn e Instagram precisam de publicação consistente para gerar inbound
6. **Processo comercial** — Como vai ser a venda? Indicação, conteúdo, ferramenta gratuita? Precisa escolher 1 e executar

---

*Relatório salvo em `outputs/reports/2026-02-12-identidade-graventum.md`*
