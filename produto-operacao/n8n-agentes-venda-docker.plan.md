---
name: n8n — Criar agentes, vender ao cliente e deploy (Docker)
overview: "Respostas diretas: sim, no n8n se criam agentes; como vender isso ao cliente (modelos de entrega); e o papel do container Docker na oferta."
todos: []
isProject: false
---

# n8n: agentes, como vender ao cliente e Docker

## 1. No n8n se criam agentes?

**Sim.** No n8n você monta **workflows** que funcionam como agentes de IA:

- **Nós de LLM** (OpenAI, Anthropic/Claude, etc.): o workflow recebe input (pergunta, lead, documento), chama a IA e devolve resposta ou dispara ações.
- **LangChain / Agent:** em instalações **self-hosted** existe suporte a fluxos tipo agente (LangChain Code node, templates “AI Agent”). A imagem Docker para isso pode ser `n8nio/n8n` (padrão) ou, quando disponível, variantes com AI/LangChain.
- **Na prática:** um “agente” no n8n é um **workflow com trigger** (webhook, schedule, formulário) que usa nós de IA, condições e integrações (Supabase, WhatsApp, email). Ex.: “Webhook recebe pergunta → nó Claude gera resposta → resposta vai para WhatsApp”. Isso já é um agente conversacional.

Ou seja: você **não** cria um “agente” como entidade separada; você cria um **workflow** que se comporta como agente (recebe, processa com IA, age). O cliente pode chamar de “agente” na comunicação comercial.

---

## 2. Como vender isso ao cliente?

Depende de **quem hospeda o n8n** e **quem “vê” a interface**.

### Opção A: Tudo na sua infra (Graventum)

- **Seu n8n** (já em Docker, ex.: graventum-n8n) roda os workflows.
- Você **cria e mantém** os workflows do cliente (agente de atendimento, qualificação de lead, resumo de relatório, etc.).
- O cliente **não** acessa o n8n; ele só usa o resultado (WhatsApp, email, painel, API).
- **Como vender:** “Agente de IA sob medida para o seu negócio: atendimento, qualificação de leads, relatório diário. Nós configuramos e operamos; você usa no dia a dia.” Incluído no **retainer** ou como **pacote** (ex.: “Pacote Agente WhatsApp”).

**Prós:** Controle total, um único n8n, menor custo de infra. **Contras:** Cliente não “vê” o agente por dentro; depende de você para mudanças.

### Opção B: n8n dedicado por cliente (Docker por cliente)

- Cada cliente tem **um n8n só dele**, em um **container Docker** (no seu servidor ou no deles).
- Você entrega o **projeto** (workflows exportados em JSON) e o **container** (imagem n8n + volume de dados).
- O cliente (ou você em nome dele) acessa a interface do n8n para ver/ajustar workflows, ou você mantém e o cliente só consome.
- **Como vender:** “Seu próprio ambiente de automação com agentes de IA. Isolado, seus dados não misturam com outros clientes. Inclui X agentes prontos + suporte para novos.”

**Prós:** Isolamento, “ambiente do cliente”, possível white-label. **Contras:** Mais custo (um container/VM por cliente), mais manutenção (atualizar vários n8n).

### Opção C: Híbrido

- **Produção** na sua infra (Opção A): workflows rodam no seu n8n; cliente só usa os canais (WhatsApp, email, etc.).
- **Opcional:** Para cliente que quiser “ver” ou evoluir: você oferece **acesso restrito** a um espaço (ex.: pastas por cliente no mesmo n8n) ou um **n8n separado** (Docker) só para ele.

**Como vender:** “Agentes operando no dia a dia; se quiser, podemos liberar um painel de automação só seu (ambiente dedicado).”

---

## 3. Seria em um container Docker?

**Pode ser.** O n8n já roda em Docker no seu contexto (container graventum-n8n). A decisão é **quantos n8n** você quer rodar:


| Modelo                            | Docker                           | Comentário                                                                                                                                                                   |
| --------------------------------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Um n8n para todos os clientes** | 1 container (o que você já tem)  | Workflows de todos no mesmo n8n; separação por pasta/nome. Entrega = “agente como serviço”, cliente não vê o n8n.                                                            |
| **Um n8n por cliente**            | 1 container por cliente          | Cada cliente tem seu próprio container n8n (ex.: `n8n-cliente-contabilidade-x`). Dados e workflows isolados. Você pode vender como “ambiente dedicado” ou “n8n white-label”. |
| **n8n na infra do cliente**       | Container no servidor do cliente | Você entrega imagem Docker + volumes + workflows; o cliente (ou o TI dele) sobe o container. Venda: “Solução self-hosted; dados ficam na sua rede.”                          |


**Resumo:**  

- **Não é obrigatório** dar um container por cliente. A oferta pode ser “agentes rodando no nosso n8n” (um só container).  
- **Se quiser** oferecer “n8n dedicado” ou “self-hosted”, aí sim: **um container Docker (n8n) por cliente** ou um container no ambiente do cliente.

---

## 4. Tem como criar painel no n8n?

**Não.** O n8n **não tem recurso nativo** para criar painel (dashboard) de negócio. A interface dele serve para desenhar e rodar workflows, ver execuções e configurar nós — não para montar telas com gráficos, KPIs e filtros para o cliente final.

**O que dá para fazer:**


| Abordagem                      | O que é                                                                                                               | Papel do n8n                                                                                                                  |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **n8n alimenta outro sistema** | O painel fica em outra ferramenta (Grafana, Metabase, Retool, Google Data Studio, etc.).                              | n8n busca/processa dados e grava no banco (ex.: Supabase) ou envia para a ferramenta. O **painel** é criado nessa ferramenta. |
| **Frontend próprio + n8n**     | Você faz um site/painel (ex.: Next.js) que lê dados do Supabase (ou API).                                             | n8n popula o banco/API em tempo real ou agendado. O **painel** é a sua aplicação que consome esses dados.                     |
| **n8nDash**                    | Projeto open source (PHP + MySQL + HTML/Bootstrap) que funciona como dashboard para n8n.                              | Integra com n8n (webhooks, dados); o “painel” é o n8nDash, não o n8n.                                                         |
| **UI dentro do workflow**      | Nós com elementos de interface (botões, inputs) nos **formulários/triggers** do n8n (ex.: Form Trigger, Wait + form). | São telas para **preencher formulário ou aprovar passo**, não um dashboard de métricas.                                       |


**Resumo:** Para ter **painel** (gráficos, KPIs, visão do negócio), use **n8n na parte de dados/automação** e o **painel** em outra ferramenta (Grafana, Metabase, Retool) ou no seu próprio front (Next.js, etc.) lendo do Supabase que o n8n alimenta — ou n8nDash, se fizer sentido. Ou seja: não dá para “criar painel” dentro do n8n; dá para usar o n8n para **alimentar** um painel que fica fora dele.

---

## 5. Recomendações práticas (Graventum)

1. **Começar com Opção A:** Agentes como **workflows no seu n8n**; cliente não acessa o n8n. Venda: “Agente de IA para [atendimento / leads / relatório diário] — nós configuramos e mantemos.” Inclui no retainer. Sem novo container por cliente.
2. **Docker:** Manter **um** container n8n (graventum-n8n) como hoje. Só considerar **um container por cliente** se houver demanda explícita por isolamento ou white-label.
3. **Comunicação:** Chamar de “agente” ou “agentes de IA” na proposta; na técnica, são workflows n8n com nós de IA. Não precisa falar “workflow” para o cliente.
4. **Se o cliente pedir “meu próprio n8n”:** Aí sim oferecer n8n em **Docker dedicado** (você hospeda) ou **entrega do Docker** para o cliente subir no servidor dele; precificar como add-on ou pacote superior.

---

## 5. Referências

- n8n self-hosted + agentes: [n8n.io/ai-agents](https://n8n.io/ai-agents), templates “AI Agent” e “LangChain” na comunidade n8n.
- No projeto: n8n em Docker (container graventum-n8n), [data/COMO-AGENTE-INTEGRA-N8N.md](.openclaw/workspace/data/COMO-AGENTE-INTEGRA-N8N.md), [MEMORY.md](.openclaw/workspace/MEMORY.md) (regras do container).

