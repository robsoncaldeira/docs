---
name: Jornada do cliente Graventum — Relatório com fluxogramas e diagramas
overview: "Relatório da jornada do cliente da Graventum com fluxogramas e diagramas (Mermaid): visão geral, fases detalhadas, funnel e stack por fase."
todos: []
isProject: false
---

# Jornada do cliente Graventum — Fluxogramas e diagramas

**Tipo:** Relatório com fluxogramas e diagramas  
**Base:** Identidade Graventum, ciclo de vida do projeto, fluxograma de processos executivos.  
**Fonte:** [outputs/reports/2026-02-12-identidade-graventum.md](.openclaw/workspace/outputs/reports/2026-02-12-identidade-graventum.md), [outputs/diagramas/fluxograma-processos-executivos.md](.openclaw/workspace/outputs/diagramas/fluxograma-processos-executivos.md), [processes/ciclo-vida-projeto-graventum.md](.openclaw/workspace/docs-repo-https/processes/ciclo-vida-projeto-graventum.md).

---

## 1. Visão geral da jornada

A jornada do cliente segue seis fases em sequência:

**ATRAÇÃO → DIAGNÓSTICO → TRIAL → IMPLEMENTAÇÃO → RETAINER → EXPANSÃO**

Alternativa operacional (com qualificação e proposta explícitas):

**LEAD → QUALIFICAÇÃO → DIAGNÓSTICO → PROPOSTA → IMPLEMENTAÇÃO → OPERAÇÃO → EXPANSÃO**

---

## 2. Diagrama 1 — Jornada em sequência (alto nível)

```mermaid
flowchart LR
  A[Atração] --> B[Diagnóstico]
  B --> C[Trial]
  C --> D[Implementação]
  D --> E[Retainer]
  E --> F[Expansão]

  subgraph fases [Fases]
    A
    B
    C
    D
    E
    F
  end
```



---

## 3. Diagrama 2 — Fluxo detalhado por fase (com entradas e saídas)

```mermaid
flowchart TB
  subgraph atracao [Fase 1: Atração]
    A1[LinkedIn]
    A2[Indicação]
    A3[Site e SEO]
    A4[Ferramenta gratuita]
    A1 --> A0[Cliente nos encontra]
    A2 --> A0
    A3 --> A0
    A4 --> A0
  end

  subgraph diagnostico [Fase 2: Diagnóstico 1-2 sem]
    B1[Reunião inicial]
    B2[Mapeamento processos]
    B3[Gargalos e oportunidades]
    B4[Roadmap 3-5 automações]
    B1 --> B2 --> B3 --> B4
  end

  subgraph trial [Fase 3: Trial 14 dias]
    C1[Automação 1 maior impacto]
    C2[Cliente testa]
    C3[Acompanhamento diário]
    C1 --> C2 --> C3
  end

  subgraph impl [Fase 4: Implementação 15-30 dias]
    D1[Deploy automações roadmap]
    D2[Integração ferramentas cliente]
    D3[Treinamento time]
    D1 --> D2 --> D3
  end

  subgraph retainer [Fase 5: Retainer contínuo]
    E1[Monitoramento]
    E2[Suporte WhatsApp]
    E3[1-2 micro-automações/mês]
    E4[Relatório mensal]
    E1 --> E2
    E2 --> E3
    E3 --> E4
  end

  subgraph expansao [Fase 6: Expansão]
    F1[Novos departamentos]
    F2[Automações mais complexas]
    F3[Upsell]
    F1 --> F2 --> F3
  end

  atracao --> diagnostico
  diagnostico --> trial
  trial --> impl
  impl --> retainer
  retainer --> expansao
```



---

## 4. Diagrama 3 — Funnel comercial (do lead à conversão)

```mermaid
flowchart TB
  L[Leads capturados] --> Q{Qualificado?}
  Q -->|Sim| D[Diagnóstico]
  Q -->|Não| Desc[Descartado]
  D --> P[Proposta]
  P --> Acc{Aceita?}
  Acc -->|Sim| T[Trial 14 dias]
  Acc -->|Não| N[Negociação ou perda]
  T --> Go{Go-live?}
  Go -->|Sim| I[Implementação]
  Go -->|Não| Ajust[Ajustes]
  Ajust --> T
  I --> R[Retainer mensal]
  R --> Ex[Expansão]

  style L fill:#1565c0,stroke:#0d47a1,color:#fff
  style R fill:#2e7d32,stroke:#1b5e20,color:#fff
  style Ex fill:#ef6c00,stroke:#e65100,color:#fff
```



---

## 5. Diagrama 4 — Atividades por fase (timeline)

```mermaid
gantt
  title Jornada típica do cliente (primeiros 60 dias)
  dateFormat YYYY-MM-DD
  section Atração
  Conteúdo e captação :a1, 2026-01-01, 14d
  section Diagnóstico
  Reunião e mapeamento :b1, after a1, 7d
  Roadmap e priorização :b2, after b1, 5d
  section Trial
  Automação 1 e testes :c1, after b2, 14d
  section Implementação
  Deploy e integrações :d1, after c1, 15d
  Treinamento e handover :d2, after d1, 5d
  section Retainer
  Operação contínua :e1, after d2, 30d
```



---

## 6. Diagrama 5 — Stack e responsável por fase

```mermaid
flowchart LR
  subgraph stack [Ferramentas por fase]
    S1[LinkedIn, Site, GA]
    S2[Meet, Docs, Notion]
    S3[n8n, Evolution API, Supabase, Claude]
    S4[n8n, graventum-api, Supabase]
    S5[n8n, Supabase, WhatsApp]
    S6[Novos workflows, IA avançada]
  end

  subgraph fases [Fase]
    F1[Atração]
    F2[Diagnóstico]
    F3[Trial]
    F4[Implementação]
    F5[Retainer]
    F6[Expansão]
  end

  F1 --- S1
  F2 --- S2
  F3 --- S3
  F4 --- S4
  F5 --- S5
  F6 --- S6
```



### 6.1 Como funciona a implementação na prática (origem dos dados e acesso)

O Diagrama 5 mostra *quais* ferramentas entram em cada fase; abaixo fica claro **de onde vêm os dados** e **o que o cliente precisa dar** para cada tipo de entrega.

#### De onde vêm os dados?


| Cenário                                           | Quem alimenta o quê                                     | Como                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Planilhas (lista de clientes, produtos, etc.)** | Cliente envia arquivos (Google Sheet, Excel, CSV).      | Graventum importa para o DB (Supabase) no onboarding ou em atualizações combinadas (ex.: 1x por mês). Pode haver um processo manual (você baixa e roda script) ou, se o cliente tiver API/integração, ingestão automática.                                                                        |
| **Cliente “alimenta” o seu DB**                   | Dados ficam no sistema do cliente (ERP, CRM, planilha). | **Opção A:** Cliente exporta e manda; você coloca no DB. **Opção B:** Integração (webhook, API, Zapier/Make) do sistema do cliente para o seu Supabase/n8n. **Opção C:** Acesso read-only a uma planilha Google/Excel; n8n lê direto (sem cópia no seu DB) quando fizer sentido.                  |
| **Bot WhatsApp com IA (respostas inteligentes)**  | Precisa de base de conhecimento.                        | **RAG:** Cliente fornece o conteúdo (PDFs, FAQ, docs, site) que o bot deve usar. Você monta o RAG (embeddings + vetores no Supabase ou ferramenta de RAG). O cliente **não** precisa te dar “acesso a tudo”; só o que for **relevante para o bot** (ex.: manual do produto, políticas, catálogo). |
| **WhatsApp (envio/recebimento)**                  | Número e histórico de conversas.                        | Cliente usa um número (próprio ou novo); Graventum conecta via Evolution API (ou similar). Acesso ao número/instância; histórico pode ser importado ou começar do zero.                                                                                                                           |


#### O cliente precisa me dar acesso a tudo?

**Não.** Só ao que for **necessário para cada automação**:

- **Bot WhatsApp + RAG:** conteúdo (arquivos/links) para a base de conhecimento; acesso ao número/WhatsApp.
- **Automação com planilha:** acesso à planilha (view ou edit, conforme combinado) ou envio periódico do arquivo.
- **Email/Drive:** permissão OAuth ou pasta específica; não “todo o Drive”.
- **ERP/CRM:** idealmente API ou webhook; se não tiver, exportação combinada (ex.: CSV semanal) que você importa no DB.

Resumo: **acesso mínimo por entrega** — definido no diagnóstico e no contrato (o que você pode acessar e por quanto tempo).

#### Fluxo resumido: dados entrando na stack

```mermaid
flowchart LR
  subgraph cliente [Lado do cliente]
    P[Planilhas]
    S[Sistema ERP/CRM]
    Docs[PDFs FAQ site]
    WA[Número WhatsApp]
  end

  subgraph graventum [Graventum]
    IMP[Importação / API]
    DB[(Supabase)]
    RAG[Base RAG]
    n8n[n8n + Evolution API]
  end

  P -->|envio ou link| IMP
  S -->|API ou export| IMP
  IMP --> DB
  Docs -->|conteúdo para bot| RAG
  WA --> n8n
  DB --> n8n
  RAG --> n8n
```



- **Planilhas / sistema:** entram via importação (manual ou script) ou integração → ficam no **seu** DB (Supabase) e o n8n consome dali (ou lê direto da planilha, se for o desenho).
- **Bot WhatsApp:** usa **RAG** com o que o cliente ceder (documentos, FAQ); o cliente não precisa abrir “tudo”, só o necessário para o bot responder bem.

---

## 7. Resumo das fases (tabela)


| Fase                 | Duração     | Entregável principal                                         | Ferramentas principais                              |
| -------------------- | ----------- | ------------------------------------------------------------ | --------------------------------------------------- |
| **1. Atração**       | Contínuo    | Cliente nos encontra                                         | LinkedIn, Instagram, site, SEO, ferramenta gratuita |
| **2. Diagnóstico**   | 1–2 semanas | Roadmap com 3–5 automações priorizadas                       | Google Meet, Docs, Notion                           |
| **3. Trial**         | 14 dias     | Automação #1 em produção, cliente testando                   | n8n, Evolution API, Supabase, Claude                |
| **4. Implementação** | 15–30 dias  | Todas as automações do roadmap + treinamento                 | n8n, graventum-api, Supabase, Evolution API         |
| **5. Retainer**      | Contínuo    | Operação estável, 1–2 novas automações/mês, relatório mensal | n8n, Supabase, WhatsApp (suporte)                   |
| **6. Expansão**      | Contínuo    | Novos departamentos, automações mais complexas, upsell       | Stack ampliada conforme escopo                      |


---

## 8. Fluxograma em ASCII (alternativa sem Mermaid)

Para uso em documentos que não renderizam Mermaid:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  ATRAÇÃO    │────▶│ DIAGNÓSTICO │────▶│   TRIAL     │
│ (cliente    │     │ 1-2 sem     │     │ 14 dias     │
│  nos acha)  │     │ roadmap     │     │ automação 1 │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                    ┌─────────────┐            │
                    │ EXPANSÃO    │◀───────────┤
                    │ upsell      │            │
                    └──────▲──────┘            │
                           │                   ▼
                    ┌──────┴──────┐     ┌─────────────┐
                    │  RETAINER   │◀────│IMPLEMENTAÇÃO│
                    │ mensal      │     │ 15-30 dias  │
                    │ suporte     │     │ deploy full │
                    └─────────────┘     └─────────────┘
```

---

## 9. Referências

- Identidade e jornada: [outputs/reports/2026-02-12-identidade-graventum.md](.openclaw/workspace/outputs/reports/2026-02-12-identidade-graventum.md)
- Fluxograma processos: [outputs/diagramas/fluxograma-processos-executivos.md](.openclaw/workspace/outputs/diagramas/fluxograma-processos-executivos.md)
- Ciclo de vida: [docs-repo-https/processes/ciclo-vida-projeto-graventum.md](.openclaw/workspace/docs-repo-https/processes/ciclo-vida-projeto-graventum.md)

