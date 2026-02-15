# Limpeza do projeto (fev/2026)

Arquivos removidos por serem redundantes ou scripts one-off já com resultado consolidado. Escopo: `/root` (projeto todo).

## Removidos

### /root (raiz)
- **Arquivo corrompido** (nome com trecho de código `.join(fallbacks)...`) — removido.

### .openclaw/workspace (raiz do workspace)
- **gerar_dores.py**, **gerar_dores2.py** — geradores one-off de dores; resultados já estão nos CSVs consolidados.
- **move-docs-simple.py**, **move-docs-to-github.py** — scripts pontuais de migração de docs.
- **test-gitbook-sync.sh** — teste pontual.
- **executar-dataset-supabase.sh** — substituído por `data/push_dores_solucoes_to_supabase.py` + `para_supabase_complemento.csv`; continha token em texto.

### graventum-api-push/
- **FIX-WORKFLOW-AGORA.md** — fix one-off; workflow documentado em docs/.
- **WORKFLOW-PRONTO.md** (raiz) — duplicata de `docs/WORKFLOW-PRONTO.md`; mantida só a versão em docs/.

### outputs/datasets/
- **expansao-2000-linhas.py** — script one-off que gerou 2000 a partir de 500; o CSV 2000-dores-solucoes-completo.csv já existe.
- **gerar-mais-dores.py**, **gerar-dores-imobiliarias.py** — geradores pontuais; os CSVs de saída já estão nas planilhas consolidadas.
- **unification-process.md**, **dataset-expansion-plan.md** — planejamentos substituídos por CONSOLIDACAO_*.md.
- **resumo-dores-imobiliarias.md**, **resumo_dataset.md**, **relatorio-final-pesquisa.md** — resumos substituídos por CONSOLIDACAO_DATASETS.md e CONSOLIDACAO_TUDO.md.

### outputs/scripts/
- **insert-2000-to-supabase.py** — lógica duplicada; uso: `push_dores_solucoes_to_supabase.py` com `para_supabase_complemento.csv`.

### scripts/
- **insert_500_records.py** — inseria 500 no Supabase via cliente supabase; substituído por `data/push_dores_solucoes_to_supabase.py`.
- **upload_dataset_to_supabase.py** — upload do 500 para Supabase; substituído por `data/push_dores_solucoes_to_supabase.py`.
- **create_tables_via_api.py** — criação de tabelas via API; substituído por `outputs/supabase/dores_solucoes_table.sql` + `data/create_dores_solucoes_table_supabase.py`.

### Duplicatas e ativos legados (workspace)

**CSVs na raiz do workspace (formato antigo; substituídos por outputs/datasets):**
- **dores_contabilidade_dataset.csv**, **_2**, **_3**, **_4**, **dores_contabilidade_completo.csv** — contabilidade; fonte canônica: 500/2000 e consolidated em outputs/datasets.
- **dores_turismo_*** (completo, dataset, final, parte3, adicional), **dataset_turismo_limpo.csv**, **dataset_turismo_350.csv**, **dataset_turismo_completo.csv** — turismo; consolidado em outputs/datasets.
- **dores_ajuste.csv**, **dores_adicionais.csv**, **dores_finais.csv**, **dores_geradas2.csv**, **dor_extra.csv**, **dataset-clinicas-dores.csv** — intermediários/legados.

**Cópias duplicadas:**
- **docs-repo-https/instagram/2026-02-14/crm-inteligente-metadata.json** — idêntico a `outputs/instagram/2026-02-14/crm-inteligente-metadata.json`; mantida só a versão em outputs.

**Reports de teste:**
- **outputs/reports/test-exec-1770908710.md**, **test-2026-02-12-093002.md**, **test-2026-02-12-115053.md**, **teste-com-link-124414.md**, **teste-workflow-122230.md** — saídas de teste; removidas.

**n8n workflows obsoletos:**
- **outputs/n8n-workflows/executar-dataset-supabase.json** — envio de dataset via workflow; substituído por script `push_dores_solucoes_to_supabase.py` + CSV de complemento.

## Mantidos (referência)

- **/root:** README-PROJETO.md (índice geral), helix-*.json (configs agente), docker-compose.yml, backups/, graventum-api/, graventum-api-push/, n8n-data/, .pm2/.
- **.openclaw/workspace:** configurar-n8n-workflow.sh, executar-agora.sh, executar-via-n8n.sh (n8n/orquestração); AGENTS.md, BOOTSTRAP.md, MEMORY.md, TOOLS.md, etc.; data/, docs/, outputs/.
- **outputs/datasets:** 500 na raiz; consolidated_*.csv, para_supabase_complemento.csv; scripts (consolidar_*, gerar_complemento, push_*.sh); README.md, CONSOLIDACAO_DATASETS.md; _fontes/ (planilhas origem), _relatorios/ (relatórios dos scripts).
- **data/:** push_dores_solucoes_to_supabase.py, import_dores_solucoes_csv.py, create_dores_solucoes_table_supabase.py, run_push_dores_solucoes_supabase.sh, SETUP_COMPLETO.md, PLANILHA_SUPABASE.md, workflows n8n, DB.
- **docs/:** SCRIPTS_DORES_SUPABASE.md (documentação dos scripts reaproveitados).
