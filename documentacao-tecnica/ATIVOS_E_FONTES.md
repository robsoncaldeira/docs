# Ativos e fontes canônicas (evitar duplicatas)

Onde estão os dados e artefatos oficiais para não duplicar.

## Dores / soluções (planilhas)

**Pasta organizada:** `outputs/datasets/` — leia [outputs/datasets/README.md](../outputs/datasets/README.md).

| Uso | Fonte canônica | Observação |
|-----|----------------|------------|
| **Já no Supabase** | `500-dores-solucoes-completo.csv` | Referência; fica na raiz da pasta. |
| **Complemento para o DB** | `para_supabase_complemento.csv` (264) | Gerado por `gerar_complemento_para_supabase.py`. |
| **Consolidado único** | `consolidated_tudo.csv` (660) | Todas as fontes, sem duplicatas. |
| **Todas as linhas** | `consolidated_tudo_todas_linhas.csv` (3554) | Com duplicatas. |
| **Fontes origem** | Pasta `_fontes/` | 2000, 350 imobiliárias, dores-pesquisadas; não apagar para poder regerar. |

Scripts na mesma pasta. Relatórios detalhados em `_relatorios/`. Ver [SCRIPTS_DORES_SUPABASE.md](SCRIPTS_DORES_SUPABASE.md).

## Instagram / conteúdo

- **Metadata e captions:** `outputs/instagram/` (por data e tema). Não duplicar em docs-repo-https/instagram (cópias foram removidas).

## n8n workflows

- **Enviar dados ao Supabase:** usar script `data/push_dores_solucoes_to_supabase.py` com CSV; não usar workflow `executar-dataset-supabase.json` (removido).
- **Workflows ativos:** `data/n8n_*.json`, `outputs/n8n-workflows/*.json` (exceto o removido acima).

## Reports

- **Relatórios de identidade/raio-x:** `outputs/reports/2026-02-12-*.md`, etc.
- **Testes:** não manter test-*.md / teste-*.md em reports (foram removidos).

## Limpeza feita

Lista completa do que foi removido (scripts, CSVs legados, duplicatas): [LIMPEZA_2026-02.md](LIMPEZA_2026-02.md).
