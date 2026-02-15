---
name: Carregar CSV no banco
overview: Criar tabela dores_solucoes que espelhe o CSV (500 registros), importar no SQLite e opcionalmente no Supabase; documentar scripts e passos.
todos: []
isProject: false
---

# Carregar planilha CSV no banco de dados

## Contexto

- **Banco SQLite:** [graventum_dores_solucoes.db](.openclaw/workspace/data/graventum_dores_solucoes.db) — tabelas `company_pains`, `graventum_solutions`, `action_plans` e views.
- **Planilha:** [500-dores-solucoes-completo.csv](.openclaw/workspace/outputs/datasets/500-dores-solucoes-completo.csv) — 501 linhas (1 cabeçalho + 500 registros).
- **Colunas do CSV:** `nicho`, `tema`, `dor_base`, `descricao_detalhada`, `impacto_financeiro`, `solucao_graventum`, `tecnologia_utilizada`, `beneficio_esperado`.

A estrutura do CSV não bate 1:1 com `company_pains`/`graventum_solutions`. Foi criada uma **tabela nova** `dores_solucoes` que espelha o CSV, no SQLite e no Supabase.

### Documentação dos scripts (reaproveitados)

Ver **[docs/SCRIPTS_DORES_SUPABASE.md](.openclaw/workspace/docs/SCRIPTS_DORES_SUPABASE.md)** — lista de scripts, fluxo consolidação → complemento → push, comandos e caminhos.

### Consolidação dos datasets (500 + 2000 e pesquisa)

Na pasta [outputs/datasets](.openclaw/workspace/outputs/datasets) foram consolidadas as planilhas e removidas duplicatas:

- **consolidated_dores_solucoes.csv** — 595 registros únicos (união de 500-dores-solucoes-completo.csv e 2000-dores-solucoes-completo.csv, chave `nicho,tema,dor_base`). O arquivo 500 já foi enviado ao Supabase; os 199 registros que só existem na planilha 2000 podem ser enviados depois.
- **consolidated_dores_pesquisa.csv** — 65 registros únicos no formato pesquisa (industry, pain_title, …), para análise.

Scripts: `consolidar_dores_solucoes.py` e `consolidar_dores_pesquisa.py`. Detalhes: [outputs/datasets/CONSOLIDACAO_DATASETS.md](.openclaw/workspace/outputs/datasets/CONSOLIDACAO_DATASETS.md).

---

## Parte 1 — SQLite (executado)

### 1.1 Nova tabela no SQLite

Tabela `dores_solucoes` em `graventum_dores_solucoes.db` com colunas alinhadas ao CSV:


| Coluna CSV           | Coluna no DB (TEXT)  |
| -------------------- | -------------------- |
| nicho                | nicho                |
| tema                 | tema                 |
| dor_base             | dor_base             |
| descricao_detalhada  | descricao_detalhada  |
| impacto_financeiro   | impacto_financeiro   |
| solucao_graventum    | solucao_graventum    |
| tecnologia_utilizada | tecnologia_utilizada |
| beneficio_esperado   | beneficio_esperado   |


Extras: `id` (INTEGER PRIMARY KEY AUTOINCREMENT), `created_at` (TIMESTAMP).

### 1.2 Script de importação SQLite

- **Arquivo:** [data/import_dores_solucoes_csv.py](.openclaw/workspace/data/import_dores_solucoes_csv.py)
- Lê o CSV em UTF-8 (módulo `csv`), conecta no SQLite, cria a tabela se não existir e insere em uma transação.
- **Executar:** `python3 /root/.openclaw/workspace/data/import_dores_solucoes_csv.py`
- **Reimportar:** apagar registros antes para evitar duplicatas.

### 1.3 Verificação SQLite

- Contagem: `sqlite3 .../graventum_dores_solucoes.db 'SELECT COUNT(*) FROM dores_solucoes;'` → 500.
- Amostra: `SELECT id, nicho, tema, substr(dor_base,1,50) FROM dores_solucoes LIMIT 3;`

---

## Parte 2 — Supabase

### 2.1 SQL para criar a tabela no Supabase

- **Arquivo:** [outputs/supabase/dores_solucoes_table.sql](.openclaw/workspace/outputs/supabase/dores_solucoes_table.sql)
- Cria a tabela `dores_solucoes` com as mesmas colunas da planilha + `id` (BIGSERIAL), `created_at` (TIMESTAMPTZ), RLS e política "Allow all", índices em `nicho` e `tema`.
- **Onde executar:** [Supabase SQL Editor](https://app.supabase.com/project/ckjiixentlgitwqoquzm/sql/new) — colar o conteúdo do arquivo e Run.

### 2.2 Script de envio CSV → Supabase

- **Arquivo:** [data/push_dores_solucoes_to_supabase.py](.openclaw/workspace/data/push_dores_solucoes_to_supabase.py)
- Usa `SUPABASE_URL` e `SUPABASE_SERVICE_ROLE_KEY` (ou `SUPABASE_ANON_KEY`) do `.env` na raiz do workspace.
- Verifica se a tabela existe; se não existir, imprime o SQL para copiar e sai.
- Lê o CSV e envia os 500 registros em lotes (80 por request) para `POST /rest/v1/dores_solucoes`.
- **Executar:** `cd /root/.openclaw/workspace && python3 data/push_dores_solucoes_to_supabase.py`

### 2.3 Script opcional: criar tabela via conexão Postgres

- **Arquivo:** [data/create_dores_solucoes_table_supabase.py](.openclaw/workspace/data/create_dores_solucoes_table_supabase.py)
- Requer `SUPABASE_DB_URL` no `.env` (connection string do Postgres).
- Executa o SQL de dores_solucoes_table.sql via `psycopg2`. Se `SUPABASE_DB_URL` não estiver definido, imprime instruções para rodar o SQL no Dashboard.

### 2.4 Script único (criar + enviar)

- **Arquivo:** [data/run_push_dores_solucoes_supabase.sh](.openclaw/workspace/data/run_push_dores_solucoes_supabase.sh)
- Tenta criar a tabela (se houver `SUPABASE_DB_URL`), depois executa o push. Uso: `bash data/run_push_dores_solucoes_supabase.sh` (a partir da raiz do workspace).

### 2.5 Passos para ter a planilha no Supabase

1. Criar a tabela (uma vez): abrir o [SQL Editor](https://app.supabase.com/project/ckjiixentlgitwqoquzm/sql/new), colar o conteúdo de `outputs/supabase/dores_solucoes_table.sql`, Run.
2. Enviar dados: `python3 data/push_dores_solucoes_to_supabase.py`.

Alternativa com senha do banco no `.env`: definir `SUPABASE_DB_URL` e rodar `bash data/run_push_dores_solucoes_supabase.sh`.

---

## Documentação criada/atualizada

- **[data/SETUP_COMPLETO.md](.openclaw/workspace/data/SETUP_COMPLETO.md)** — seções sobre a tabela `dores_solucoes` no SQLite e "Planilha no Supabase" (passos e script único).
- **[data/PLANILHA_SUPABASE.md](.openclaw/workspace/data/PLANILHA_SUPABASE.md)** — guia passo a passo para colocar a planilha no Supabase.

---

## Resumo de artefatos


| Artefato                          | Caminho                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------- |
| Planilha CSV (original 500)       | `.openclaw/workspace/outputs/datasets/500-dores-solucoes-completo.csv`           |
| Planilha consolidada (595 únicos) | `.openclaw/workspace/outputs/datasets/consolidated_dores_solucoes.csv`           |
| Banco SQLite                      | `.openclaw/workspace/data/graventum_dores_solucoes.db` (tabela `dores_solucoes`) |
| Import SQLite                     | `.openclaw/workspace/data/import_dores_solucoes_csv.py`                          |
| SQL tabela Supabase               | `.openclaw/workspace/outputs/supabase/dores_solucoes_table.sql`                  |
| Push Supabase                     | `.openclaw/workspace/data/push_dores_solucoes_to_supabase.py`                    |
| Criar tabela Supabase (opcional)  | `.openclaw/workspace/data/create_dores_solucoes_table_supabase.py`               |
| Script único Supabase             | `.openclaw/workspace/data/run_push_dores_solucoes_supabase.sh`                   |
| Guia Supabase                     | `.openclaw/workspace/data/PLANILHA_SUPABASE.md`                                  |


---

## Observação n8n

O workflow [n8n_workflow_supabase_clean.json](.openclaw/workspace/data/n8n_workflow_supabase_clean.json) lê do **Supabase** (tabela `leads`). Para usar os dados da planilha no n8n:

- Ajustar o workflow para ler a tabela `dores_solucoes` no Supabase (após o push), ou
- Usar o workflow que lê o SQLite ([n8n_sqlite_workflow.json](.openclaw/workspace/data/n8n_sqlite_workflow.json)) e apontar para a tabela `dores_solucoes` no SQLite.

