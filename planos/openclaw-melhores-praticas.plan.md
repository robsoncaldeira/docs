---
name: OpenClaw — Melhores práticas, boas perguntas e autonomia segura
overview: "Guia para usar o OpenClaw como parceiro na jornada empreendedora: boas práticas de uso, como fazer perguntas que geram resultados, e como aumentar a autonomia em tarefas complexas sem correr riscos."
todos: []
isProject: false
---

# OpenClaw: Melhores práticas, boas perguntas e autonomia segura

Este plano reúne dicas, tutoriais e regras para tirar o máximo do OpenClaw como **parceiro** na sua jornada empreendedora e, aos poucos, dar a ele **mais autonomia** em tarefas complexas, **sem correr riscos** desnecessários.

---

## 1. O que é o OpenClaw no seu contexto

- **Workspace:** `.openclaw/workspace` — seu “escritório” digital (dados, scripts, planilhas, documentação, memórias).
- **Quem executa:** Um agente de IA (ex.: Cursor/Helix) que lê e segue arquivos como `AGENTS.md`, `MEMORY.md`, `SOUL.md`, `BOOTSTRAP.md` e os planos em `.cursor/plans/`.
- **Papel:** Copilot empresarial — ajuda a tomar decisões, executar tarefas, manter contexto e integrar com n8n, Supabase e outras ferramentas.

Para que ele seja um **parceiro** de verdade, é preciso: **boas práticas de uso**, **perguntas bem feitas** e **autonomia progressiva com segurança**.

---

## 2. Melhores práticas de uso do OpenClaw

### 2.1 Dar contexto antes da tarefa

- **Início de sessão:** O agente já lê `memory/YYYY-MM-DD.md`, `MEMORY.md` (em sessão main) e `AGENTS.md`. Você pode dizer em uma frase o foco do dia: *“Hoje quero fechar a integração do CSV no Supabase e testar um workflow no n8n.”*
- **Ao pedir algo complexo:** Diga o **objetivo** e o **limite**: *“Quero um script que envie os 500 registros da planilha para o Supabase, mas sem alterar tabelas existentes.”*
- **Quando há restrições:** Deixe explícito: *“Não mexer no container do n8n. Só criar um novo workflow e importar.”*

Assim o OpenClaw evita suposições erradas e respeita suas regras (como as de n8n e Supabase em `MEMORY.md`).

### 2.2 Usar a memória a seu favor

- **Pendências:** O que está em `MEMORY.md` (Pendências / Ações) deve ser considerado pelo agente. Atualize quando concluir algo ou quando surgir uma nova prioridade.
- **Lições aprendidas:** Use `memory/lessons.md` para erros que não quer repetir (ex.: “Não rodar DDL no Supabase via REST; usar SQL Editor.”).
- **Estado das plataformas:** Arquivos como `memory/platform-status.json` e `memory/heartbeat-state.json` ajudam o agente a não perguntar de novo “o n8n está no ar?” — ele pode checar antes.

Quanto mais a memória estiver atualizada, mais o OpenClaw age como alguém que “já conhece o projeto”.

### 2.3 Dividir tarefas grandes em etapas

- Em vez de: *“Implementa o pipeline completo de leads com IA, Supabase e Telegram.”*
- Prefira: *“Primeiro: lista os passos que precisamos (dados, triggers, nós n8n). Depois implementamos um passo por vez e testamos.”*

Tarefas em etapas reduzidas permitem **revisar** e **corrigir** antes de seguir, e o agente pode propor um plano (ex.: em `.cursor/plans/`) e você aprova antes da execução.

### 2.4 Especificar onde e como salvar resultados

- Diga onde quer artefatos: *“Gera o SQL da tabela e salva em `outputs/supabase/dores_solucoes_table.sql`.”*
- Para documentação: *“Atualiza o SETUP_COMPLETO.md com a seção da planilha no Supabase.”*

Isso mantém o workspace organizado e evita arquivos soltos.

### 2.5 Usar Plan mode quando a decisão for importante

- Em Cursor, use **Plan mode** para: mudanças de arquitetura, novos workflows, alterações em produção. O agente propõe um plano; você revisa e só então aprova a execução.
- Para coisas pequenas (corrigir typo, um comando, uma query), o modo normal já resolve.

Assim você **aumenta a autonomia** em tarefas rotineiras e **mantém o controle** nas decisões de maior impacto.

---

## 3. Tutorial: como fazer boas perguntas

Boas perguntas geram respostas úteis e evitam idas e voltas.

### 3.1 Perguntas que funcionam bem


| Tipo                 | Exemplo                                                                                                                                   | Por que funciona                                       |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| Objetivo + restrição | “Quero colocar a planilha no Supabase. Não tenho senha do Postgres; só posso usar o SQL Editor e a REST API.”                             | O agente sabe o que fazer e o que não usar.            |
| Contexto + formato   | “Com base na tabela dores_solucoes, me dá um plano com 5 ideias de workflow n8n. Quero em markdown, em um novo arquivo em .cursor/plans.” | Define base de dados, tipo de resultado e onde salvar. |
| Etapa explícita      | “Primeiro me explica o que você entendeu do fluxo. Só depois sugere o código.”                                                            | Garante alinhamento antes de implementar.              |
| Verificação          | “Antes de rodar esse script em produção, lista o que ele vai alterar (arquivos, tabelas, APIs).”                                          | Reduz risco de efeitos colaterais.                     |


### 3.2 Perguntas que atrapalham

- **Muito vagas:** *“Melhora isso.”* (não fica claro o “isso” nem o critério de “melhor”.)
- **Sem contexto:** *“Cria um workflow.”* (qual trigger? qual objetivo? qual sistema?).
- **Tudo de uma vez:** *“Configura n8n, Supabase, Telegram, IA e o site.”* (muitas decisões e dependências; melhor quebrar em passos.)

### 3.3 Fórmula prática para pedidos complexos

Use mentalmente:

1. **O quê** — resultado desejado (ex.: “planilha no Supabase”, “workflow que qualifica leads”).
2. **Com o quê** — dados/ferramentas (ex.: “CSV em outputs/datasets”, “tabela dores_solucoes”, “n8n e Supabase”).
3. **Limites** — o que não fazer (ex.: “não alterar tabelas existentes”, “não parar o container n8n”).
4. **Formato / destino** — onde e como quer o resultado (ex.: “um novo plano em .cursor/plans”, “SQL em outputs/supabase”).

Exemplo completo: *“Quero um plano com ideias de workflows n8n com base na planilha dores_solucoes (CSV e tabela no Supabase). Inclui objetivo de cada workflow, trigger e nós principais. Salva em .cursor/plans com um nome descritivo.”*

### 3.4 Pedir explicação antes de código

- *“Explica em 3 passos como você faria para enviar os 500 registros do CSV para o Supabase.”*
- *“Quais nós do n8n usaríamos e em que ordem?”*

Isso treina o “parceiro” a **raciocinar em voz alta** e você valida o raciocínio antes de implementar.

---

## 4. OpenClaw como parceiro na jornada empreendedora

### 4.1 Parceiro, não “ferramenta”

- Trate pedidos como **briefings**: objetivo, contexto, restrições. Quanto mais você compartilha o “porquê”, mais o OpenClaw pode sugerir alternativas e antecipar problemas.
- Use a **memória** como “histórico do projeto”: decisões em `MEMORY.md`, lições em `memory/lessons.md`, relatórios em `daily-reports/` ou `memory/YYYY-MM-DD.md`. O agente que lê isso age com mais coerência ao longo do tempo.
- Peça **resumos e próximos passos**: ao fim de uma tarefa, *“Resume o que fizemos e sugere 3 próximos passos prioritários.”* Isso mantém a jornada orientada a resultados.

### 4.2 Rotinas que fortalecem a parceria

- **Início do dia (opcional):** *“Lê MEMORY.md e memory/hoje.md. Lista as pendências e sugere por onde começar.”*
- **Após uma decisão importante:** *“Registra em MEMORY.md que decidimos X; atualiza a tabela de pendências.”*
- **Quando algo der errado:** *“Documenta o que aconteceu em memory/lessons.md para não repetirmos.”*

Assim o OpenClaw passa a “lembrar” do projeto e da sua forma de trabalhar.

### 4.3 Integração com o resto da operação

- **n8n:** OpenClaw propõe e documenta workflows; a execução e as credenciais ficam no n8n (boa prática já descrita em `resumo-openclaw-copilot-empresarial.md`).
- **Supabase:** Para CRUD (leads, dores_solucoes) o agente pode usar a API; para DDL (criar tabelas) você usa o SQL Editor — deixe isso explícito em MEMORY ou no plano.
- **Planos em .cursor/plans:** Use planos para “contratos” de trabalho: o que será feito, em que ordem e com que cuidado. Depois de executar, atualize o plano ou marque concluído.

---

## 5. Aumentar autonomia sem correr riscos

A ideia é dar **mais autonomia em tarefas complexas** de forma **progressiva** e **controlada**.

### 5.1 Princípio: autonomia em camadas


| Camada               | O que o OpenClaw pode fazer sozinho                                                  | O que exige sua confirmação                                                |
| -------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| Leitura e análise    | Ler arquivos, planilhas, DB; resumir; listar opções                                  | —                                                                          |
| Proposta             | Gerar planos, esboços de workflow, SQL, scripts                                      | Executar em produção, alterar credenciais ou infra                         |
| Edição em “rascunho” | Criar/editar arquivos em pastas seguras (ex.: `outputs/`, `data/`, `.cursor/plans/`) | Editar `.env`, parar containers, mudar configuração de produção            |
| Execução sensível    | —                                                                                    | Rodar migrações em produção, publicar workflows, enviar mensagens em massa |


Você pode dizer explicitamente: *“Pode criar e editar arquivos em outputs/ e .cursor/plans. Para rodar scripts que alterem dados em produção ou toquem no n8n/Supabase, me mostre o plano antes.”*

### 5.2 Regras que reduzem risco

- **Sempre que for executar algo destrutivo ou irreversível:** o agente deve **listar** o que será alterado (arquivos, tabelas, endpoints) e você **aprova** antes (ou usa Plan mode).
- **Credenciais e segredos:** nunca no código; usar variáveis de ambiente (ex.: `.env`) e lembrar no MEMORY/AGENTS que o agente não deve expor chaves.
- **n8n / Docker:** Manter as regras atuais: não parar/remover o container de produção; só criar workflows e importar; em caso de dúvida, checar antes.
- **Supabase:** DDL (CREATE/ALTER) via SQL Editor por você; o agente pode gerar o SQL e dizer onde colar.

### 5.3 Autonomia “por tipo de tarefa”

- **Alta autonomia (pode fazer e salvar):**  
Gerar documentação, planos, SQL em arquivos, scripts em `data/` ou `tools/`, análises em markdown, atualizar MEMORY e memory/daily.
- **Autonomia com confirmação:**  
Rodar scripts que escrevem em banco (ex.: import CSV para Supabase), criar/atualizar workflows n8n (mostrar JSON ou passos antes), alterar arquivos de configuração que afetem produção.
- **Sempre com sua aprovação:**  
Executar DDL em produção, mudar regras de firewall/rede, enviar campanhas (email/WhatsApp/Telegram), excluir dados em massa, alterar ou recriar containers.

### 5.4 Frases que você pode usar

- *“Faz isso em modo plano: propõe os passos e só executa depois que eu aprovar.”*
- *“Pode implementar tudo em outputs/ e data/; não rode nada que altere produção sem me avisar.”*
- *“Lista exatamente o que esse comando vai mudar; só vou rodar se eu confirmar.”*
- *“Da próxima vez que pedir algo assim, sugira um plano em .cursor/plans antes de codar.”*

Isso “treina” o uso: mais autonomia onde é seguro, mais checagem onde o impacto é alto.

---

## 6. Resumo rápido

- **Melhores práticas:** Dar contexto, usar memória (MEMORY.md, lessons, platform-status), quebrar tarefas grandes, dizer onde salvar, usar Plan mode em decisões importantes.
- **Boas perguntas:** Incluir objetivo, restrições, formato/destino; pedir explicação antes de código quando for complexo; evitar perguntas vagas ou gigantes.
- **Parceiro:** Briefings claros, registrar decisões e lições, pedir resumos e próximos passos, alinhar OpenClaw com n8n e Supabase via documentação e planos.
- **Autonomia segura:** Autonomia em camadas (leitura → proposta → edição em rascunho); confirmação para execução sensível; regras fixas para n8n, Supabase e credenciais; usar frases explícitas para o que pode ou não fazer sozinho.

---

## 7. Referências no workspace

- Contexto de negócio e regras do agente: [AGENTS.md](.openclaw/workspace/AGENTS.md), [MEMORY.md](.openclaw/workspace/MEMORY.md)
- OpenClaw como copilot + n8n: [outputs/resumo-openclaw-copilot-empresarial.md](.openclaw/workspace/outputs/resumo-openclaw-copilot-empresarial.md), [outputs/openclaw-advanced-use-cases-2026-02-14.md](.openclaw/workspace/outputs/openclaw-advanced-use-cases-2026-02-14.md)
- Planos relacionados: [carregar_csv_no_banco_0b497ebf.plan.md](.cursor/plans/carregar_csv_no_banco_0b497ebf.plan.md), [workflows_n8n_dores_solucoes.plan.md](.cursor/plans/workflows_n8n_dores_solucoes.plan.md)

