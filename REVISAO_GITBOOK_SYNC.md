# Revisão profunda: por que o GitBook não reflete o repositório

Mesmo com **Sync complete** e **Change requests** mergeados, a sidebar pode continuar antiga. Abaixo estão as causas mais prováveis e o que fazer em cada caso.

---

## 1. Project directory preenchido (causa muito comum)

No GitBook, cada Space pode ter um **Project directory** (pasta dentro do repo). Se estiver preenchido, o GitBook **só** usa essa pasta.

- **Onde ver:** Space → **Settings** → **Git Sync** (ou **Integrations** → GitHub).
- **O que deve estar:** Project directory **vazio** ou **`.`** (raiz).
- **Se estiver** com algo como `planos`, `docs`, `gitbook-export`, etc.: o GitBook procura `SUMMARY.md` **só dentro dessa pasta**. Na raiz temos Business cases, Dashboards, Processes; ele nunca lê isso.
- **Ação:** Apagar o valor do Project directory (deixar em branco) e salvar. Depois fazer **Sync** de novo, direção **GitHub → GitBook**.

---

## 2. Repositório ou branch errados

O Space pode estar ligado a outro repo ou a outro branch.

- **Conferir:** Settings → Git Sync: repositório deve ser **`robsoncaldeira/docs`** e branch **`main`**.
- **Ação:** Se estiver diferente, corrigir e fazer **Sync** (GitHub → GitBook).

---

## 3. Estrutura (SUMMARY) não é reaplicada no sync

Em alguns fluxos, o GitBook atualiza o **conteúdo** das páginas mas não reconstrói a **sidebar** a partir do `SUMMARY.md` do repo após o merge do change request.

- **Ação:** Forçar um **re-import completo**:
  1. Settings → Git Sync.
  2. **Desconectar** o Git (Remove / Disconnect).
  3. **Conectar de novo** ao mesmo repo (`robsoncaldeira/docs`, branch `main`), **Project directory em branco**.
  4. Na primeira sincronização, escolher **GitHub → GitBook** (importar do repositório para o GitBook).
  5. Isso recria a estrutura a partir do `SUMMARY.md` e deve trazer Business cases, Dashboards, Processes.

---

## 4. Site publicado é outro Space

O que você edita (e onde faz Sync) é o **Space**. O site público (ex.: `graventum.gitbook.io/graventum-docs`) é configurado no **Site** (collection de Spaces).

- **Conferir:** No GitBook, abrir o **Site** que serve a URL pública → **Settings** → **Structure**. Ver qual **Space** está associado à seção que aparece na doc.
- **Ação:** Garantir que o Space em que você fez Sync (e merge dos change requests) é o **mesmo** que está publicado nesse Site. Se estiver outro Space, trocar para o correto ou publicar de novo.

---

## 5. Cache ou publicação desatualizada

Às vezes a estrutura já está certa no Space, mas o site público ou o cache não atualizou.

- **Ação:** No **Site** (não no Space), ver se há botão **Publish** / **Deploy** e disparar de novo. Abrir o site em aba anônima ou outro navegador para testar sem cache.

---

## 6. Arquivos existem no repo?

Para o GitBook mostrar uma página, ela precisa:

1. Estar listada no `SUMMARY.md`.
2. O arquivo existir no caminho indicado (ex.: `business-cases/README.md`).

No repositório **robsoncaldeira/docs** (branch `main`) já existem:

- `SUMMARY.md` na raiz (com Business cases, Dashboards, Processes).
- `business-cases/` (README, caso-real, case-1 a 4, diagramas-executivos).
- `dashboards/` (README, graventum-dashboard.md, leads-dashboard.md, pipeline-dashboard.md, project-timeline.md + os .html).
- `processes/` (README, ciclo-vida-projeto-graventum.md).

Foi adicionado também um **`.gitbook.yaml`** na raiz com `structure.readme` e `structure.summary` para deixar explícito onde está a primeira página e o índice.

---

## Ordem recomendada de verificação

1. **Project directory** vazio (raiz).
2. **Repo** = `robsoncaldeira/docs`, **branch** = `main`.
3. **Desconectar e reconectar** Git Sync e fazer **Import from GitHub** (GitHub → GitBook).
4. Confirmar que o **Space** em que você faz Sync é o **mesmo** publicado no **Site**.
5. Dar **Publish** de novo no Site e testar em aba anônima.

Se depois disso ainda não refletir, o próximo passo é checar no suporte GitBook se há limite ou regra do plano (ex.: número de páginas ou de seções) que impeça a atualização da estrutura.
