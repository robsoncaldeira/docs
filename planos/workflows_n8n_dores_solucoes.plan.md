---
name: Workflows n8n baseados na planilha Dores-Soluções
overview: "Ideias de workflows no n8n que usam a tabela dores_solucoes (500 registros: nicho, tema, dor_base, solucao_graventum, beneficio_esperado, etc.) para qualificação de leads, geração de conteúdo, propostas e atendimento."
todos: []
isProject: false
---

# Ideias de Workflows n8n com a planilha Dores-Soluções

## Base de dados

- **Fonte:** Tabela `dores_solucoes` (500 registros), espelho da planilha [500-dores-solucoes-completo.csv](.openclaw/workspace/outputs/datasets/500-dores-solucoes-completo.csv).
- **Colunas úteis para workflows:** `nicho`, `tema`, `dor_base`, `descricao_detalhada`, `impacto_financeiro`, `solucao_graventum`, `tecnologia_utilizada`, `beneficio_esperado`.
- **Onde está:** SQLite (`.openclaw/workspace/data/graventum_dores_solucoes.db`) e Supabase (tabela `dores_solucoes` após rodar o push). No n8n, usar **Supabase** (HTTP Request ou nó Supabase) ou **Read/Write File** + SQLite se o DB estiver montado no container.

---

## 1. Resposta personalizada ao lead (dor → solução)

**Objetivo:** Quando um lead informa sua “dor” ou nicho (formulário, webhook, chat), o workflow consulta `dores_solucoes`, escolhe as melhores soluções e envia uma resposta personalizada (email, WhatsApp ou Telegram).

**Ideia de fluxo:**

- **Trigger:** Webhook (formulário/landing) ou novo lead no Supabase com campo `nicho` / `dor_texto`.
- **Nó 1:** Supabase ou HTTP Request — `GET /rest/v1/dores_solucoes?nicho=eq.{nicho}&limit=3` (ou filtro por `tema`).
- **Nó 2:** Code ou Template — monta mensagem usando `solucao_graventum`, `beneficio_esperado`, `impacto_financeiro`.
- **Nó 3:** Email (SMTP/SendGrid) ou WhatsApp/Telegram — envia a mensagem ao lead.

**Dados da planilha usados:** `nicho`, `tema`, `solucao_graventum`, `beneficio_esperado`, `impacto_financeiro`.

---

## 2. Geração de conteúdo por nicho/tema

**Objetivo:** Gerar posts (LinkedIn, blog, e-mail) usando dores e soluções reais da base, agrupados por `nicho` ou `tema`.

**Ideia de fluxo:**

- **Trigger:** Schedule (ex.: toda segunda às 9h).
- **Nó 1:** Supabase — `SELECT DISTINCT nicho FROM dores_solucoes` (ou por `tema`).
- **Nó 2:** Loop (Split In Batches) — para cada nicho.
- **Nó 3:** Supabase — `SELECT * FROM dores_solucoes WHERE nicho = {nicho} LIMIT 5`.
- **Nó 4:** IA (OpenAI/Ollama) — prompt: “Com base nestas dores e soluções: {dor_base, solucao_graventum, beneficio_esperado}, gere um post para LinkedIn em 2 parágrafos.”
- **Nó 5:** Salvar em tabela `conteudo_gerado` ou enviar para buffer de redes / email.

**Dados da planilha usados:** `nicho`, `tema`, `dor_base`, `solucao_graventum`, `beneficio_esperado`.

---

## 3. Proposta comercial automática

**Objetivo:** Lead qualificado (ex.: em Supabase `leads`) recebe uma proposta em PDF ou documento com até 3 dores-soluções do seu nicho, com impacto e benefício.

**Ideia de fluxo:**

- **Trigger:** Novo item em `leads` com `nicho` preenchido ou webhook “gerar proposta” com `lead_id` e `nicho`.
- **Nó 1:** Supabase — buscar `dores_solucoes WHERE nicho = {nicho} LIMIT 3` (ou ordenar por tema).
- **Nó 2:** Code ou Template — montar texto/HTML da proposta (dor_base, solucao_graventum, impacto_financeiro, beneficio_esperado).
- **Nó 3:** Gerar PDF (nó dedicado ou serviço externo) e anexar a email ou gravar em Storage.

**Dados da planilha usados:** `nicho`, `tema`, `dor_base`, `descricao_detalhada`, `impacto_financeiro`, `solucao_graventum`, `beneficio_esperado`.

---

## 4. Bot “Qual é a sua dor?” (Telegram / WhatsApp)

**Objetivo:** Usuário envia uma frase; o workflow identifica a dor (por palavra-chave ou IA) e devolve soluções da base.

**Ideia de fluxo:**

- **Trigger:** Webhook do Telegram ou WhatsApp (mensagem recebida).
- **Nó 1:** Code ou IA — classificar a mensagem em um `nicho` e opcionalmente um `tema` (ex.: Contabilidade, Compliance).
- **Nó 2:** Supabase — `SELECT * FROM dores_solucoes WHERE nicho = {nicho} LIMIT 3` (ou busca por similaridade em `dor_base` se houver extensão/IA).
- **Nó 3:** Formatar resposta com `solucao_graventum` e `beneficio_esperado` e enviar de volta pelo mesmo canal.

**Dados da planilha usados:** `nicho`, `tema`, `dor_base`, `solucao_graventum`, `beneficio_esperado`.

---

## 5. Relatório semanal por nicho

**Objetivo:** Resumo quantitativo e qualitativo das dores/soluções por nicho para uso interno ou gestão.

**Ideia de fluxo:**

- **Trigger:** Schedule (ex.: sexta às 18h).
- **Nó 1:** Supabase — `SELECT nicho, COUNT(*), array_agg(DISTINCT tema) FROM dores_solucoes GROUP BY nicho`.
- **Nó 2:** Code — montar texto ou tabela (ex.: “Contabilidade: X dores, temas: Declarações fiscais, SPED, …”).
- **Nó 3:** Email ou Google Sheets — enviar relatório ou preencher planilha.

**Dados da planilha usados:** `nicho`, `tema` (agregação).

---

## 6. Enriquecimento de lead com dores sugeridas

**Objetivo:** Ao criar ou atualizar um lead (Supabase `leads`) com `nicho` ou `industry`, preencher automaticamente sugestões de dores/soluções para esse lead (para o time comercial usar).

**Ideia de fluxo:**

- **Trigger:** Supabase trigger (novo/atualização em `leads`) ou Schedule que busca leads sem “dores_sugeridas”.
- **Nó 1:** Para cada lead, ler `nicho` (ou mapear `industry` → nicho).
- **Nó 2:** Supabase — `SELECT id, dor_base, solucao_graventum, beneficio_esperado FROM dores_solucoes WHERE nicho = {nicho} LIMIT 5`.
- **Nó 3:** Supabase Update — gravar em `leads` (campo JSON ou tabela relacionada) as dores_sugeridas para o lead.

**Dados da planilha usados:** `nicho`, `dor_base`, `solucao_graventum`, `beneficio_esperado`.

---

## 7. API / Landing dinâmica (dores por nicho ou tema)

**Objetivo:** Landing page ou site chama um webhook com `nicho` ou `tema` e recebe JSON com dores e soluções para preencher blocos “Problemas que resolvemos” / “Soluções”.

**Ideia de fluxo:**

- **Trigger:** Webhook GET com query `?nicho=Contabilidade` ou `?tema=SPED`.
- **Nó 1:** Supabase ou HTTP Request — `GET /rest/v1/dores_solucoes?nicho=eq.Contabilidade&select=dor_base,solucao_graventum,beneficio_esperado,tema&limit=10`.
- **Nó 2:** Respond to Webhook — devolver JSON com a lista para o front consumir.

**Dados da planilha usados:** `nicho`, `tema`, `dor_base`, `solucao_graventum`, `beneficio_esperado`.

---

## 8. Sincronização dores_solucoes → outro sistema

**Objetivo:** Manter um CRM, planilha (Google Sheets) ou outro sistema atualizado com a lista de dores-soluções.

**Ideia de fluxo:**

- **Trigger:** Schedule (diário) ou Webhook após atualização no Supabase.
- **Nó 1:** Supabase — `SELECT * FROM dores_solucoes` (ou com filtros).
- **Nó 2:** Google Sheets / Airtable / HTTP Request — inserir ou atualizar linhas no destino.

**Dados da planilha usados:** Todas as colunas, conforme necessidade do destino.

---

## Priorização sugerida


| Prioridade | Workflow                          | Motivo                                    |
| ---------- | --------------------------------- | ----------------------------------------- |
| Alta       | 1. Resposta personalizada ao lead | Uso direto em captação e primeiro contato |
| Alta       | 7. API / Landing dinâmica         | Poucos nós, alto impacto em conversão     |
| Média      | 2. Geração de conteúdo            | Alimenta marketing com dados reais        |
| Média      | 6. Enriquecimento de lead         | Melhora qualificação comercial            |
| Média      | 3. Proposta comercial             | Acelera fechamento                        |
| Baixa      | 4. Bot Telegram/WhatsApp          | Requer integração de canal                |
| Baixa      | 5. Relatório semanal              | Uso interno / gestão                      |
| Baixa      | 8. Sincronização                  | Útil se já existir outro sistema          |


---

## Referências

- Planilha e carga no banco: [carregar_csv_no_banco_0b497ebf.plan.md](.cursor/plans/carregar_csv_no_banco_0b497ebf.plan.md)
- SQL tabela Supabase: [outputs/supabase/dores_solucoes_table.sql](.openclaw/workspace/outputs/supabase/dores_solucoes_table.sql)
- Workflows existentes (leads, conteúdo): [outputs/n8n-workflows/](.openclaw/workspace/outputs/n8n-workflows/) (workflow-1-qualificacao-leads, workflow-2-geracao-conteudo, dores-solucoes-automation)
- n8n Graventum: [https://automation.graventum.com](https://automation.graventum.com)

