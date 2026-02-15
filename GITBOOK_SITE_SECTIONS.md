# Configurar GitBook com seções separadas (site paginado)

Para que cada aba superior (Planos, API Reference, Produto e operação, etc.) seja uma **seção independente** — e não tudo sob "Índice" — o GitBook usa **Site sections**: cada seção = um **Space** diferente, cada Space apontando para uma **pasta** deste repositório (monorepo).

## Estrutura do repositório (monorepo)

Cada pasta abaixo é uma **seção** e deve ser configurada como **Project directory** de um Space no GitBook:

| Pasta no repo        | Nome sugerido no GitBook   |
|----------------------|----------------------------|
| `/` (raiz)           | Visão geral / Início       |
| `planos/`            | Planos                     |
| `api-reference/`     | API Reference              |
| `estrategia-mercado/`| Estratégia e mercado       |
| `produto-operacao/`  | Produto e operação         |
| `ml-modelos/`        | ML e modelos               |
| `openclaw-ferramentas/` | OpenClaw e ferramentas  |
| `documentacao-tecnica/` | Documentação técnica    |

Cada pasta contém:
- `README.md` — primeira página da seção
- `SUMMARY.md` — índice só dos docs dessa seção
- Os arquivos `.md` / `.plan.md` dessa seção

## Passos no GitBook (requer plano com Site sections)

1. **Criar um Space para cada seção**
   - No GitBook: Organization → Create → Space (ou duplicar o Space atual e reconfigurar).
   - Crie 8 Spaces no total (um por pasta acima).

2. **Conectar cada Space ao mesmo repositório**
   - Em cada Space: **Settings → Git Sync** (ou Integrations → GitHub).
   - Repositório: `robsoncaldeira/docs` (ou o repo que espelha este conteúdo).
   - **Project directory**: informe **apenas** a pasta da seção, por exemplo:
     - Space "Planos" → Project directory: `planos`
     - Space "API Reference" → Project directory: `api-reference`
     - Space "Estratégia e mercado" → Project directory: `estrategia-mercado`
     - E assim por diante para `produto-operacao`, `ml-modelos`, `openclaw-ferramentas`, `documentacao-tecnica`.
   - O Space "Visão geral" usa Project directory em branco (raiz do repo) ou `.`.

3. **Configurar o Site (documentação publicada)**
   - No **Site** que publica a documentação: **Settings → Structure**.
   - Adicione cada Space como **Site section** (botão "New section", escolha o Space).
   - Ordene as seções na ordem desejada (ex.: Visão geral, Planos, API Reference, …).
   - Defina qual seção é a **default** (ex.: Visão geral) em Actions → Set as default.

4. **Sincronizar**
   - Após o push neste repo, use **Sync** em cada Space (ou aguarde o sync automático) para atualizar o conteúdo.

## Observação

**Site sections** estão disponíveis no plano **Ultimate** do GitBook. Se o seu plano não incluir Site sections, as abas superiores continuarão sendo geradas a partir de um único Space; nesse caso, a estrutura em pastas ainda é útil para organização e para migrar depois.

## Referência

- [GitBook – Monorepos](https://gitbook.com/docs/getting-started/git-sync/monorepos)
- [GitBook – Site sections](https://docs.gitbook.com/publishing-documentation/site-structure/site-sections)
