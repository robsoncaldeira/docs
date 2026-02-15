# Fazer o GitBook refletir as alterações do GitHub

Quando você altera o repositório **robsoncaldeira/docs** no GitHub (ou dá push do `docs-merge`), o **site do GitBook** (graventum.gitbook.io) pode continuar mostrando a versão antiga. Siga um dos caminhos abaixo.

---

## Opção 1: Sync manual no GitBook (recomendado)

1. Acesse **[app.gitbook.com](https://app.gitbook.com)** e abra o Space **Graventum Docs**.
2. Vá em **Settings** (engrenagem no menu do Space).
3. Encontre **Git Sync** ou **Integrations → GitHub**.
4. Clique em **Sync** / **Pull from GitHub** / **Sync now** (o botão que puxa o conteúdo do repo).
5. Aguarde terminar. O conteúdo do Space deve atualizar (ex.: "Estrutura" sem "(SUMMARY)", dashboards com links novos).
6. Se o seu site público estiver em **Docs sites** (ex.: graventum.gitbook.io):
   - Abra o **Site**, não só o Space.
   - Vá em **Publish** ou **Sync** no Site, se existir, para publicar a versão mais recente do Space.

---

## Opção 2: Reimportar o repo via API (força atualização)

Se o Sync manual não aparecer ou não atualizar, você pode disparar um **import** de novo do GitHub:

```bash
export GITBOOK_API_TOKEN="gb_api_..."   # token da API do GitBook
python3 scripts/gitbook_import_repo.py
```

(O script está em `/root/scripts/gitbook_import_repo.py` e usa o Space **kt1CEHZkTvzvHzNYVH7N**.)

Depois do import, aguarde alguns minutos e confira o Space. Se o Site estiver separado, publique o Site de novo.

---

## Por que não reflete sozinho?

- O GitBook **não** atualiza o conteúdo do Space a cada push no GitHub de forma garantida; em muitos setups é preciso rodar **Sync** (ou import) manualmente.
- O **Space** é o “livro”; o **Site** é o site público. Às vezes o Space atualiza mas o Site só mostra a nova versão depois de **Publish** no Site.

Resumo: **Settings do Space → Git Sync → Sync** e, se usar Site, **Publish no Site**.
