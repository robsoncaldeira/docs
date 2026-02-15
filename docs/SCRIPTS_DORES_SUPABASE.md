# Scripts dores_solucoes — documentação e fluxo

Documentação dos **scripts reaproveitados** para planilhas dores_solucoes, consolidação e envio ao Supabase.

**Índice geral do projeto (raiz /root):** [README-PROJETO.md](/root/README-PROJETO.md)

---

## 1. Visão do fluxo

```
[Planilhas fonte]  →  consolidar_*.py  →  [CSVs consolidados]
        ↓
[500 já no Supabase] + [consolidated_tudo]  →  gerar_complemento_para_supabase.py  →  para_supabase_complemento.csv
        ↓
push_dores_solucoes_to_supabase.py  →  Supabase (tabela dores_solucoes)
```

---

## 2. Scripts reaproveitados (referência rápida)

| Script | Onde | Função | Entrada | Saída |
|--------|------|--------|---------|--------|
| **consolidar_dores_solucoes.py** | `outputs/datasets/` | Unir 500 + 2000 e remover duplicatas por (nicho, tema, dor_base) | 500-dores-solucoes-completo.csv, 2000-dores-solucoes-completo.csv | consolidated_dores_solucoes.csv (595), CONSOLIDACAO_DORES_SOLUCOES.md |
| **consolidar_dores_pesquisa.py** | `outputs/datasets/` | Unir todos os CSVs formato pesquisa e deduplicar | 350-dores-imobiliarias-completas.csv, dores-imobiliarias-*.csv, dores-pesquisadas-*.csv | consolidated_dores_pesquisa.csv (65), CONSOLIDACAO_DORES_PESQUISA.md |
| **consolidar_tudo_em_uma_planilha.py** | `outputs/datasets/` | Unir todas as fontes (500, 2000, pesquisa) em um esquema; com e sem dedup | Os 7 CSVs acima | consolidated_tudo.csv (660 únicos), consolidated_tudo_todas_linhas.csv (3554), CONSOLIDACAO_TUDO.md |
| **gerar_complemento_para_supabase.py** | `outputs/datasets/` | Filtrar registros que ainda não estão no Supabase (chaves do 500) | 500-dores-solucoes-completo.csv, consolidated_tudo.csv | para_supabase_complemento.csv (264) |
| **push_dores_solucoes_to_supabase.py** | `data/` | Enviar CSV para a tabela dores_solucoes no Supabase (lotes de 80) | Qualquer CSV com as 8 colunas dores_solucoes; ou padrão 500 | POST /rest/v1/dores_solucoes |
| **import_dores_solucoes_csv.py** | `data/` | Importar CSV para a tabela dores_solucoes no SQLite | 500-dores-solucoes-completo.csv (caminho fixo no script) | graventum_dores_solucoes.db (tabela dores_solucoes) |
| **create_dores_solucoes_table_supabase.py** | `data/` | Criar tabela dores_solucoes no Supabase via Postgres | .env SUPABASE_DB_URL, outputs/supabase/dores_solucoes_table.sql | Tabela no Supabase |
| **run_push_dores_solucoes_supabase.sh** | `data/` | Criar tabela (se DB URL) + enviar 500 | — | Chama create_* e push_* com CSV 500 |
| **push_complemento_para_supabase.sh** | `outputs/datasets/` | Atalho para enviar só o complemento | para_supabase_complemento.csv | Chama push_dores_solucoes_to_supabase.py com complemento |

---

## 3. Uso passo a passo

### 3.1 Reexecutar consolidação (todas as planilhas em uma)

```bash
cd /root/.openclaw/workspace/outputs/datasets
python3 consolidar_tudo_em_uma_planilha.py
```

Gera: `consolidated_tudo.csv`, `consolidated_tudo_todas_linhas.csv`, `CONSOLIDACAO_TUDO.md`.

### 3.2 Gerar o CSV de complemento (só o que falta no Supabase)

```bash
cd /root/.openclaw/workspace/outputs/datasets
python3 gerar_complemento_para_supabase.py
```

Requer: `500-dores-solucoes-completo.csv` e `consolidated_tudo.csv`.  
Gera: `para_supabase_complemento.csv` (264 registros: 199 dores_solucoes + 65 pesquisa).

### 3.3 Enviar o complemento ao Supabase

**Opção A — Script Python (reaproveitado):**
```bash
cd /root/.openclaw/workspace
python3 data/push_dores_solucoes_to_supabase.py outputs/datasets/para_supabase_complemento.csv
```

**Opção B — Script shell atalho:**
```bash
/root/.openclaw/workspace/outputs/datasets/push_complemento_para_supabase.sh
```

Requer: `.env` no workspace com `SUPABASE_URL` e `SUPABASE_SERVICE_ROLE_KEY` (ou `SUPABASE_ANON_KEY`).  
Tabela `dores_solucoes` já deve existir no Supabase (criada uma vez com `outputs/supabase/dores_solucoes_table.sql`).

### 3.4 Enviar os 500 originais (primeira carga)

```bash
cd /root/.openclaw/workspace
python3 data/push_dores_solucoes_to_supabase.py
# ou
bash data/run_push_dores_solucoes_supabase.sh
```

### 3.5 Importar CSV no SQLite (local)

```bash
python3 /root/.openclaw/workspace/data/import_dores_solucoes_csv.py
```

Lê `outputs/datasets/500-dores-solucoes-completo.csv` e insere em `data/graventum_dores_solucoes.db`. Para reimportar, apague os registros antes.

---

## 4. Esquema do CSV (dores_solucoes)

Colunas obrigatórias para o push e para o SQLite:

- `nicho`, `tema`, `dor_base`, `descricao_detalhada`, `impacto_financeiro`, `solucao_graventum`, `tecnologia_utilizada`, `beneficio_esperado`

O CSV de complemento e os consolidados usam esse mesmo esquema (o `consolidated_tudo` ainda tem `origem` e `arquivo_origem`; o push usa só as 8 colunas).

---

## 5. Onde está cada coisa

| Tipo | Caminho |
|------|---------|
| **Pasta datasets (organizada)** | `outputs/datasets/` — ver **README.md** dentro da pasta. |
| Planilha referência (já no Supabase) | `outputs/datasets/500-dores-solucoes-completo.csv` |
| Fontes origem (para regerar) | `outputs/datasets/_fontes/` (2000, 350 imobiliárias, dores-imobiliarias-*, dores-pesquisadas) |
| CSVs consolidados | `outputs/datasets/consolidated_*.csv`, `para_supabase_complemento.csv` |
| Scripts de consolidação e complemento | `outputs/datasets/*.py`, `outputs/datasets/push_complemento_para_supabase.sh` |
| Scripts de banco e Supabase | `data/push_dores_solucoes_to_supabase.py`, `data/import_dores_solucoes_csv.py`, `data/create_dores_solucoes_table_supabase.py`, `data/run_push_dores_solucoes_supabase.sh` |
| SQL tabela Supabase | `outputs/supabase/dores_solucoes_table.sql` |
| Banco SQLite | `data/graventum_dores_solucoes.db` |
| Documentação detalhada consolidação | `outputs/datasets/CONSOLIDACAO_DATASETS.md`, CONSOLIDACAO_TUDO.md, CONSOLIDACAO_DORES_*.md |
| Setup geral | `data/SETUP_COMPLETO.md`, `data/PLANILHA_SUPABASE.md` |
| Plano carregar CSV | `.cursor/plans/carregar_csv_no_banco_0b497ebf.plan.md` |
| Limpeza (arquivos removidos) | `docs/LIMPEZA_2026-02.md` |

---

## 6. Dependências de ambiente

- **Push Supabase:** `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY` (ou `SUPABASE_ANON_KEY`) no `.env` em `/root/.openclaw/workspace`.
- **Criar tabela via script:** `SUPABASE_DB_URL` (connection string Postgres) no `.env`.
- **Scripts Python:** só biblioteca padrão, exceto `push_*` e `create_*` Supabase (requests; create_* usa psycopg2 se disponível).
