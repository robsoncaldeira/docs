# Checklist: fazer o GitBook refletir o repositório

O conteúdo está no GitHub ([robsoncaldeira/docs](https://github.com/robsoncaldeira/docs), branch `main`). Para o GitBook mostrar as alterações, é preciso **acionar o sync** e conferir a configuração.

## 1. Abrir o Space no GitBook

1. Acesse [app.gitbook.com](https://app.gitbook.com).
2. Entre na organização (Graventum).
3. Abra o Space **Graventum Docs** (ou o Space que usa esse repositório).

## 2. Conferir a conexão com o GitHub

1. No Space, vá em **Settings** (ou **Integrations**).
2. Procure **Git Sync** ou **GitHub**.
3. Confirme:
   - **Repositório:** `robsoncaldeira/docs`
   - **Branch:** `main`
   - **Project directory:** em branco (ou `.`) para usar a **raiz** do repositório.  
     Se estiver preenchido (ex.: `planos`), o Space só enxerga essa pasta e não vê Business cases, Dashboards, Processes etc. **Apague** o valor para usar a raiz.

## 3. Acionar o sync (importar do GitHub)

1. Na mesma tela de Git Sync, procure o botão **Sync** ou **Import from GitHub** / **Pull from repository**.
2. Clique em **Sync** (ou equivalente).
3. Escolha a direção **GitHub → GitBook** (trazer o conteúdo do repo para o GitBook).
4. Aguarde o fim do processo (pode levar 1–2 minutos).

## 4. Aceitar as alterações vindas do Git (importante)

1. Depois do Sync, o GitBook muitas vezes **não aplica na hora**: cria um **Change request** (pedido de alteração) com o conteúdo do repositório.
2. Procure no Space por:
   - **"Change requests"** ou **"Updates from Git"** no menu ou no topo.
   - Um aviso do tipo *"You have X updates from your repository"* ou *"Accept changes"*.
3. **Abra o Change request** e clique em **Merge** / **Accept** / **Aplicar** para que a estrutura (SUMMARY) e as novas páginas (Business cases, Dashboards, Processes) passem a valer no Space.
4. Sem aceitar esse merge, a sidebar continua com a estrutura antiga mesmo com "Sync complete".

## 5. Publicar (se for o caso)

1. Se a documentação publicada for outro ambiente, dê **Publish** ou **Deploy** para atualizar o site.

## 6. Se ainda não aparecer

- **Limpe o cache do navegador** ou abra o site em aba anônima.
- Confirme no GitHub que o arquivo [SUMMARY.md](https://github.com/robsoncaldeira/docs/blob/main/SUMMARY.md) tem as seções **Business cases**, **Dashboards** e **Processes**.
- No GitBook, veja se há **erros** na tela de Git Sync (permissões, branch errado, etc.) e corrija conforme a mensagem.

---

**Resumo:** O repositório está atualizado. O que falta é, no GitBook, **conferir repo/branch/Project directory** e clicar em **Sync** (GitHub → GitBook).
