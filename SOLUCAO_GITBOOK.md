# Solução definitiva: GitBook refletir o repositório

O repositório **robsoncaldeira/docs** (branch `main`) está correto: `SUMMARY.md` na raiz, `.gitbook.yaml`, e todas as pastas (Planos, API Reference, Estratégia e mercado, Produto e operação, ML e modelos, OpenClaw, Business cases, Dashboards, Processes, Documentação técnica). O que falha é o **Space atual** do GitBook não reconstruir a sidebar a partir do repo após sync/merge.

**Solução que funciona:** criar um **novo Space** e fazer **Import from GitHub** (importação completa). Assim o GitBook lê o `SUMMARY.md` e os arquivos do zero.

---

## Passo a passo

### 1. Criar um novo Space

1. Em [app.gitbook.com](https://app.gitbook.com), abra a organização (Graventum).
2. Clique em **Create** → **Space** (ou **New space**).
3. Nome sugerido: **Graventum Docs** (ou "Graventum Docs 2" se quiser manter o antigo por enquanto).

### 2. Conectar ao repositório e importar

1. No **novo** Space, vá em **Settings** → **Git Sync** (ou **Integrations** → **GitHub**).
2. Clique em **Set up Git Sync** / **Connect to GitHub**.
3. Selecione o repositório **robsoncaldeira/docs** e o branch **main**.
4. **Project directory:** deixe **vazio** (raiz do repositório).
5. Na primeira sincronização, escolha **GitHub → GitBook** (importar do repositório para o GitBook). Não escolha GitBook → GitHub.
6. Confirme e aguarde o import terminar.

Depois do import, a sidebar do **novo** Space deve mostrar todas as seções: Planos, API Reference, Estratégia e mercado, Produto e operação, ML e modelos, OpenClaw e ferramentas, **Business cases**, **Dashboards**, **Processes**, Documentação técnica.

### 3. Usar o novo Space no site público

1. Abra o **Site** que serve a URL da documentação (ex.: graventum.gitbook.io/...).
2. Vá em **Settings** → **Structure**.
3. **Ou** adicione o novo Space como seção e defina como padrão, **ou** remova o Space antigo da estrutura e deixe só o novo.
4. Faça **Publish** no Site para atualizar o site público.

### 4. (Opcional) Arquivar o Space antigo

Se tudo estiver certo no novo Space e no site, pode arquivar ou excluir o Space antigo para evitar confusão.

---

## Conferências rápidas

- **Repositório:** `robsoncaldeira/docs`
- **Branch:** `main`
- **Project directory:** vazio (raiz)
- **Direção no primeiro sync:** **GitHub → GitBook**
- Na raiz do repo existem: `README.md`, `SUMMARY.md`, `.gitbook.yaml`, e as pastas referenciadas no `SUMMARY`.

Se seguir estes passos no **novo** Space, a documentação no GitBook passa a refletir o repositório.
