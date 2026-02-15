---
name: Carregar CSV no banco
overview: Criar uma tabela no SQLite existente que espelhe as colunas do CSV e importar as 500 linhas do arquivo 500-dores-solucoes-completo.csv para o banco graventum_dores_solucoes.db.
todos: []
isProject: false
---

# Carregar planilha CSV no banco de dados

## Contexto

- **Banco:** [graventum_dores_solucoes.db](.openclaw/workspace/data/graventum_dores_solucoes.db) (SQLite) — já tem tabelas `company_pains`, `graventum_solutions`, `action_plans` e views.
- **Planilha:** [500-dores-solucoes-completo.csv](.openclaw/workspace/outputs/datasets/500-dores-solucoes-completo.csv) — 501 linhas (1 cabeçalho + 500 registros).
- **Colunas do CSV:** `nicho`, `tema`, `dor_base`, `descricao_detalhada`, `impacto_financeiro`, `solucao_graventum`, `tecnologia_utilizada`, `beneficio_esperado`.

A estrutura do CSV não bate 1:1 com `company_pains`/`graventum_solutions` (que usam `pain_title`, `solution_title`, etc.). A abordagem mais direta é criar uma **tabela nova** que espelhe o CSV e importar os dados nela, sem alterar as tabelas atuais.

## Plano

### 1. Nova tabela no SQLite

Criar tabela `dores_solucoes` em `graventum_dores_solucoes.db` com colunas alinhadas ao CSV:


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


Adicionar ainda: `id` (INTEGER PRIMARY KEY ou UUID) e `created_at` (TIMESTAMP), para consistência com o resto do banco.

### 2. Script de importação

- Ler o CSV com encoding UTF-8 e tratamento de aspas/vírgulas (ex.: módulo `csv` em Python).
- Conectar em `graventum_dores_solucoes.db` e fazer INSERT de cada linha na tabela `dores_solucoes`.
- Executar em transação única para performance e rollback em caso de erro.
- Garantir que o script use o caminho absoluto do CSV: `/root/.openclaw/workspace/outputs/datasets/500-dores-solucoes-completo.csv` e do DB: `/root/.openclaw/workspace/data/graventum_dores_solucoes.db`.

### 3. Verificação

- Contar linhas em `dores_solucoes` (esperado: 500).
- Conferir amostra (ex.: `SELECT * FROM dores_solucoes LIMIT 3`).

### 4. Documentação

- Atualizar [SETUP_COMPLETO.md](.openclaw/workspace/data/SETUP_COMPLETO.md) mencionando a tabela `dores_solucoes` e que ela foi populada a partir do CSV de 500 dores-soluções.

## Resultado

- Planilha no banco: os 500 registros do CSV ficarão na tabela `dores_solucoes` do mesmo SQLite que o n8n já pode usar (desde que o DB continue copiado/montado em `/root/n8n-data/` ou equivalente no container).
- Nenhuma alteração nas tabelas `company_pains` e `graventum_solutions`; se no futuro quiser integrar dados do CSV a essas tabelas, pode ser um passo separado (mapeamento + script de migração).

## Observação

O arquivo [n8n_workflow_supabase_clean.json](.openclaw/workspace/data/n8n_workflow_supabase_clean.json) lê do **Supabase**, não do SQLite. Se a intenção for que o n8n use os dados da planilha, depois de carregar no SQLite você pode:

- usar o workflow que já lê o SQLite ([n8n_sqlite_workflow.json](.openclaw/workspace/data/n8n_sqlite_workflow.json)) e ajustá-lo para ler a view/tabela `dores_solucoes`, ou
- manter este plano só como “planilha no banco” e definir depois se quer também enviar esses dados para o Supabase.

