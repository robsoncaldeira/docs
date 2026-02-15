# GitBook: apagar e começar do zero

Para o GitBook refletir o repositório **robsoncaldeira/docs**, o caminho é **apagar (ou arquivar) o Space atual** e **criar um novo** com importação completa do GitHub. O repo está pronto: `SUMMARY.md`, `.gitbook.yaml` e todas as pastas na raiz.

---

## 1. Apagar ou arquivar o Space atual

1. Acesse [app.gitbook.com](https://app.gitbook.com) e abra a organização (Graventum).
2. Entre no **Space** que hoje usa para a documentação (ex.: Graventum Docs).
3. Vá em **Settings** (ícone de engrenagem ou menu do Space).
4. Role até **Danger zone** / **Space settings** e use:
   - **Archive space** — o Space some da lista mas pode ser restaurado; ou  
   - **Delete space** — remove de vez (só faça se não precisar do conteúdo atual).
5. Confirme. O Space antigo deixa de existir (ou fica arquivado).

---

## 2. Criar um Space novo

1. Na mesma organização, clique em **Create** → **Space** (ou **New space**).
2. Dê um nome, por exemplo **Graventum Docs**.
3. Crie o Space (pode começar vazio).

---

## 3. Conectar ao repo e importar (GitHub → GitBook)

1. No **novo** Space, vá em **Settings** → **Git Sync** (ou **Integrations** → **GitHub**).
2. Clique em **Set up Git Sync** / **Connect to GitHub**.
3. Autorize o GitBook no GitHub se pedir.
4. Selecione:
   - **Repositório:** `robsoncaldeira/docs`
   - **Branch:** `main`
   - **Project directory:** deixe **vazio** (raiz).
5. Na primeira sincronização, escolha **GitHub → GitBook** (trazer conteúdo do repositório para o GitBook). **Não** escolha GitBook → GitHub.
6. Confirme e aguarde o import terminar.

A sidebar deve mostrar: Visão geral, Planos, API Reference, Estratégia e mercado, Produto e operação, ML e modelos, OpenClaw e ferramentas, Business cases, Dashboards, Processes, Documentação técnica.

---

## 4. Publicar no site

1. Abra o **Site** que usa a URL da documentação (ex.: graventum.gitbook.io/...).
2. Vá em **Settings** → **Structure**.
3. Adicione o **novo** Space como seção (se ainda não estiver) e defina como padrão, ou substitua o Space antigo por este.
4. Faça **Publish** no Site para atualizar o site público.

---

## Resumo

| Passo | Ação |
|-------|------|
| 1 | Apagar ou arquivar o Space atual |
| 2 | Criar um Space novo |
| 3 | Git Sync: repo `robsoncaldeira/docs`, branch `main`, Project directory **vazio**, direção **GitHub → GitBook** |
| 4 | No Site, usar o novo Space e publicar |

Começando do zero assim, o GitBook lê o `SUMMARY.md` e todos os arquivos do repositório numa única importação.
