# Links GitBook

- **Site público:** [https://graventum.gitbook.io/graventum-docs/](https://graventum.gitbook.io/graventum-docs/)
- **Space (edição):** [https://app.gitbook.com/o/z51rx8CM46OEv5tDy36h/s/kt1CEHZkTvzvHzNYVH7N/](https://app.gitbook.com/o/z51rx8CM46OEv5tDy36h/s/kt1CEHZkTvzvHzNYVH7N/)

## Disparar import do repo para este Space (via API)

Na raiz do projeto (onde está a pasta `scripts/`):

```bash
export GITBOOK_API_TOKEN="gb_api_..."
python3 scripts/gitbook_import_repo.py
```

O script usa por padrão o Space **kt1CEHZkTvzvHzNYVH7N** (Graventum Docs). O token você cria em [GitBook → API Tokens](https://app.gitbook.com/account/api-tokens).

**Se o import via API der erro:** conecte o Space ao GitHub na interface (Settings → Git Sync → repo `robsoncaldeira/docs`, branch `main`, direção **GitHub → GitBook**) e faça o sync manualmente.
