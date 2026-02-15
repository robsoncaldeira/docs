# Processo do zero — GitBook com todo o conteúdo

Você apagou tudo e criou um Space novo. Siga estes passos para o GitBook ficar com **tudo** que está no repositório: Planos, API Reference, Estratégia e mercado, Produto e operação, ML e modelos, OpenClaw, **Business cases** (caso real + casos por nicho + diagramas), **Dashboards** (Graventum, Leads, Pipeline, Project Timeline), **Processes** e Documentação técnica.

---

## O que já está no repositório

- **robsoncaldeira/docs**, branch **main**
- Raiz: `README.md`, `SUMMARY.md`, `.gitbook.yaml`
- Pastas: `planos/`, `api-reference/`, `estrategia-mercado/`, `produto-operacao/`, `ml-modelos/`, `openclaw-ferramentas/`, `business-cases/`, `dashboards/`, `processes/`, `documentacao-tecnica/`
- **Dashboards:** `graventum-dashboard.md` (+ .html), `leads-dashboard.md`, `pipeline-dashboard.md`, `project-timeline.md` (cada um com iframe para o HTML)
- **Business cases:** caso real (primeiro cliente), casos 1–4, diagramas executivos
- **Processes:** ciclo de vida do projeto Graventum

Nada disso precisa ser criado de novo no GitBook: ao importar do GitHub, tudo vem junto.

---

## Passo a passo (Space novo vazio)

### 1. Abrir o Space novo

1. Acesse [app.gitbook.com](https://app.gitbook.com).
2. Abra a organização (Graventum).
3. Entre no **Space** que você criou do zero (vazio).

### 2. Conectar ao GitHub

1. No Space, vá em **Settings** (engrenagem ou menu).
2. Clique em **Git Sync** (ou **Integrations** → **GitHub**).
3. **Set up Git Sync** / **Connect to GitHub**.
4. Autorize o GitBook no GitHub se aparecer a tela de permissões.

### 3. Escolher repositório e importar

1. **Repository:** selecione **robsoncaldeira/docs**.
2. **Branch:** **main**.
3. **Project directory:** deixe **vazio** (não preencha nenhuma pasta).
4. Quando o GitBook perguntar a direção da primeira sincronização, escolha **GitHub → GitBook** (importar do repositório para o GitBook). Não use GitBook → GitHub.
5. Confirme e aguarde o import terminar.

### 4. Conferir a sidebar

Depois do import, a sidebar deve mostrar:

- Visão geral  
- Planos  
- API Reference  
- Estratégia e mercado  
- Produto e operação  
- ML e modelos  
- OpenClaw e ferramentas  
- **Business cases** (visão geral, caso real, casos 1–4, diagramas)  
- **Dashboards** (visão geral, Graventum Dashboard, Leads Dashboard, Pipeline Dashboard, Project Timeline)  
- **Processes** (visão geral, ciclo de vida)  
- Documentação técnica  

### 5. Publicar no site (se usar um Site)

1. Abra o **Site** da documentação (ex.: graventum.gitbook.io/...).
2. **Settings** → **Structure**.
3. Adicione este Space como seção (ou defina como padrão).
4. **Publish** para atualizar o site público.

---

## Resumo

| Onde | O quê |
|------|--------|
| Repo | **robsoncaldeira/docs**, branch **main**, Project directory **vazio** |
| 1ª sync | **GitHub → GitBook** |
| Resultado | Dashboards, Business cases, Processes e todo o resto no GitBook |

Se em algum passo aparecer erro (repo, branch ou permissão), descreva a mensagem e o passo em que parou para ajustarmos.
