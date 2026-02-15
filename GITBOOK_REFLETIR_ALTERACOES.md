# Fazer o GitBook refletir as alterações do GitHub

Documentação do **caminho que funciona** para a documentação aparecer atualizada no GitBook (graventum.gitbook.io/graventum-docs).

---

## Fluxo rápido (usar na próxima vez)

### Passo 1: Subir as alterações para o GitHub

No clone do repositório de documentação (`docs-merge`):

```bash
cd /root/docs-merge
git add .
git commit -m "sua mensagem"
git push origin main
```

(Opcional: manter a branch `gitbook-export` igual à `main` para o caso de o GitBook estar nela:  
`git push origin main:gitbook-export`)

### Passo 2: Atualizar o GitBook (reimport via API)

O GitBook **não** puxa sozinho a cada push. É preciso disparar um **import** do repositório. Use o script:

```bash
export GITBOOK_API_TOKEN="gb_api_..."   # token da API do GitBook
python3 /root/scripts/gitbook_import_repo.py
```

- **Onde pegar o token:** [app.gitbook.com](https://app.gitbook.com) → organização → **Settings** → **Integrations** ou **API** → criar/copiar token.
- **O que o script faz:** chama a API do GitBook para reimportar o repo `robsoncaldeira/docs` (branch `main`) no Space **Graventum Docs** (id `kt1CEHZkTvzvHzNYVH7N`).
- **Depois:** aguarde 2–5 minutos e recarregue a página no GitBook. O conteúdo do Space passa a ser o do GitHub.

**Um comando só (substitua o token):**

```bash
export GITBOOK_API_TOKEN="gb_api_SEU_TOKEN"; python3 /root/scripts/gitbook_import_repo.py
```

---

## Resumo do caminho

| Onde | O quê |
|------|--------|
| **Código** | Você edita em `docs-merge` (ou no GitHub). |
| **GitHub** | `git push origin main` → repo `robsoncaldeira/docs` atualizado. |
| **GitBook** | Rodar o script com `GITBOOK_API_TOKEN` → reimport do repo → Space atualizado em poucos minutos. |

Sem o Passo 2, o GitBook continua mostrando a versão antiga.

---

## Referência do script

- **Arquivo:** `/root/scripts/gitbook_import_repo.py`
- **Uso:** `GITBOOK_API_TOKEN` no ambiente + `python3 /root/scripts/gitbook_import_repo.py`
- **API:** `POST https://api.gitbook.com/v1/spaces/{spaceId}/git/import` com `url` do repo e `ref` (branch `main`).
- **Space:** Graventum Docs, id `kt1CEHZkTvzvHzNYVH7N`.

---

## Se o Sync na interface do GitBook funcionar

Em alguns setups existe **Settings** do Space → **Git Sync** → botão **Sync** / **Pull from GitHub**. Se aparecer e funcionar, pode usar em vez do script. Se não aparecer ou não atualizar, o **reimport via script** (Passo 2 acima) é o caminho garantido.

---

## Branch: main vs gitbook-export

As alterações estão na **`main`**. O script importa sempre da `main`. Se no GitBook estiver configurada a branch `gitbook-export`, pode trocar para `main` em **Settings → Git Sync**, ou manter as duas iguais com `git push origin main:gitbook-export` depois do push.

---

## Segurança

Não commitar nem colar o `GITBOOK_API_TOKEN` em arquivos ou chats. Depois de usar, revogar o token no GitBook e criar outro se necessário.
