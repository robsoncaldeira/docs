---
name: Índice e uso da pasta de planos
overview: Documentação da pasta .cursor/plans — para que serve, índice acionável de todos os planos, como usar (humano e IA) e convenções.
todos: []
isProject: false
---

# Índice e uso da pasta de planos

Pasta **`.cursor/plans`**: planos, relatórios e especificações acionáveis do projeto. Cada arquivo `*.plan.md` descreve um objetivo, contexto, passos e artefatos; este plano é o ponto de entrada para **encontrar** e **acionar** qualquer plano.

---

## 1. Para que serve esta pasta

| Uso | Descrição |
|-----|-----------|
| **Índice** | Localizar rapidamente o plano certo (ver tabela abaixo). |
| **Acionar** | Seguir "Como acionar" para executar passos, scripts ou abrir documentação ligada ao plano. |
| **Contexto para IA** | Dar ao Cursor/assistente referência a um plano dizendo: "segue o plano X" ou "implementa o que está em .cursor/plans/nome.plan.md". |
| **Rastreabilidade** | Saber o que foi planejado, o que foi feito e onde estão scripts/docs gerados. |

---

## 2. Índice de planos (acionável)

Cada linha indica: **arquivo**, **nome**, **resumo** e **como acionar** (próximo passo ou artefato principal).

| Arquivo | Nome | Resumo | Como acionar |
|---------|------|--------|--------------|
| [formato_plan.plan.md](formato_plan.plan.md) | Formato de planos (.plan.md) | Especificação do formato: frontmatter YAML, corpo Markdown, nomenclatura e plano acionável. | Ao criar um plano novo, seguir a estrutura definida neste arquivo; ver também [guia_plans.plan.md](guia_plans.plan.md). |
| [ml_servidor_modelos_detalhamento_completo.plan.md](ml_servidor_modelos_detalhamento_completo.plan.md) | ML no servidor — Modelos e scripts (detalhamento completo) | Tudo que foi feito em ML: libs, scripts (segmentação, embeddings), ambiente, benefícios, instalação, n8n. | Rodar segmentação ou embeddings conforme seção 9 do plano; ver docs/ML_PYTORCH_SKLEARN_USO.md e PYTORCH_SCIKIT_LEARN_SERVIDOR.md. |
| [ml_casos_uso_fluxogramas_beneficios.plan.md](ml_casos_uso_fluxogramas_beneficios.plan.md) | ML — Casos de uso, fluxogramas e benefícios (guia visual) | Casos de uso concretos: fluxogramas Mermaid, integração n8n/Supabase, poder computacional, benefícios para clientes. | Abrir o plano para diagramas e cenários; base para propostas e para implementar workflows (formulário → embedding → pgvector → resposta). |
| [carregar_csv_no_banco_0b497ebf.plan.md](carregar_csv_no_banco_0b497ebf.plan.md) | Carregar CSV no banco | Tabela `dores_solucoes` no SQLite e Supabase; scripts de import e push. | Executar: `python3 /root/.openclaw/workspace/data/push_dores_solucoes_to_supabase.py` (aceita CSV como arg). Ver docs/SCRIPTS_DORES_SUPABASE.md. Complemento: `outputs/datasets/push_complemento_para_supabase.sh`. |
| [jornada_cliente_graventum_fluxogramas.plan.md](jornada_cliente_graventum_fluxogramas.plan.md) | Jornada do cliente Graventum | Fluxogramas e diagramas Mermaid (visão geral, fases, funnel, implementação). | Consultar para apresentações e onboarding. Diagramas em Mermaid; seção 6.1 descreve implementação (planilhas, DB, RAG WhatsApp). |
| [workflows_n8n_dores_solucoes.plan.md](workflows_n8n_dores_solucoes.plan.md) | Workflows n8n Dores-Soluções | Ideias de workflows n8n usando tabela `dores_solucoes`: lead, conteúdo, proposta, atendimento. | Implementar no n8n um dos fluxos (ex.: webhook → Supabase dores_solucoes → resposta personalizada). Base: Supabase tabela `dores_solucoes` ou CSV consolidado. |
| [agentes_ia_bi_relatorio_diario.plan.md](agentes_ia_bi_relatorio_diario.plan.md) | Agentes de IA e BI com relatório diário | Viabilidade de agentes customizados e BI com resumo por gráfico + relatório diário para PME. | Usar como base para proposta de produto/serviço; implementar piloto (ex.: n8n + IA para resumo de um gráfico). |
| [n8n_agentes_como_vender_docker.plan.md](n8n_agentes_como_vender_docker.plan.md) | n8n — Agentes, vender ao cliente e Docker | Como criar agentes no n8n, modelos de venda e papel do Docker. | Consultar para desenho de oferta e deploy; usar Docker para entregar n8n ao cliente. |
| [estudo_mercado_automatizacao_pme_br.plan.md](estudo_mercado_automatizacao_pme_br.plan.md) | Estudo de mercado — Automação/IA PME Brasil | Tamanho de mercado, segmentos, concorrência, tendências e riscos. | Usar em pitch, planejamento e conteúdo; atualizar com dados novos quando necessário. |
| [graventum_identidade_entregas_relatorio.plan.md](graventum_identidade_entregas_relatorio.plan.md) | Graventum — Identidade e entregas | Identidade, modelo de negócio e entregas com base na doc OpenClaw. | Base para site, pitch e alinhamento interno; cruzar com jornada do cliente (fluxogramas). |
| [primeira_impressao_graventum_30h.plan.md](primeira_impressao_graventum_30h.plan.md) | Primeira impressão — 30h de leitura | Visão superficial da pasta após folhear a documentação. | Referência de "primeira impressão"; útil para onboarding e revisão de clareza da doc. |
| [openclaw_melhores_praticas_parceiro_autonomia.plan.md](openclaw_melhores_praticas_parceiro_autonomia.plan.md) | OpenClaw — Melhores práticas e autonomia | Boas práticas, perguntas que geram resultado e autonomia segura. | Seguir ao usar o OpenClaw; compartilhar com parceiros/usuários do assistente. |
| [openclaw_coo_capacidade_máxima_7e95f7b3.plan.md](openclaw_coo_capacidade_máxima_7e95f7b3.plan.md) | OpenClaw COO — Capacidade máxima | Maximizar Helix: estrutura no contexto, indexar docs-repo no RAG, n8n webhooks, AGENTS.md. | Executar itens do plano: atualizar contexto, RAG, webhooks e AGENTS.md conforme descrito. |
| [ollama_openclaw_redução_custos_68eaf32f.plan.md](ollama_openclaw_redução_custos_68eaf32f.plan.md) | Ollama OpenClaw — Redução de custos | Ollama no servidor; OpenClaw usa como provider com fallback para APIs. | Instalar Ollama no servidor; configurar OpenClaw para usar Ollama e manter fallback para APIs. |
| [análise_graventum_vs_ralph_clawdbot_n8n_534ceff3.plan.md](análise_graventum_vs_ralph_clawdbot_n8n_534ceff3.plan.md) | Análise Graventum vs Ralph/Clawdbot/n8n | Comparação ecossistema Ralph/Clawdbot/n8n com processos Graventum; curva de amadurecimento. | Consultar para decisão de adotar ou não Ralph Wiggum; alinhar com roadmap de agentes. |
| [teste_ralph_wiggum_ollama_deepseek_41863b7a.plan.md](teste_ralph_wiggum_ollama_deepseek_41863b7a.plan.md) | Teste Ralph Wiggum + Ollama + DeepSeek | Plano de teste do fluxo Open Ralph Wiggum + OpenCode + Ollama + DeepSeek. | Executar cenários de teste do plano; validar critérios de sucesso e fallbacks. |

---

## 3. Como usar (humano)

1. **Encontrar um plano:** use a tabela acima ou busque por nome/arquivo nesta pasta.
2. **Acionar:** na coluna "Como acionar" há comando, script ou doc; abra o `.plan.md` para detalhes.
3. **Seguir passos:** dentro do plano há seções numeradas, comandos e links para artefatos (scripts, SQL, docs).
4. **Atualizar:** se um plano ganhar novos passos ou artefatos, atualize o plano e, se necessário, a linha correspondente neste plano (índice).

---

## 4. Como usar (Cursor / IA)

- **Referenciar um plano:** "Implementa o que está em `.cursor/plans/carregar_csv_no_banco_0b497ebf.plan.md`" ou "Segue o plano de workflows n8n dores-soluções".
- **Índice:** "Lista os planos em `.cursor/plans`" → usar este plano (indice_e_uso_plans.plan.md) ou listar `*.plan.md`.
- **Convenções e estrutura:** ver [guia_plans.plan.md](guia_plans.plan.md) para quando criar, formato e como escrever planos acionáveis.

---

## 5. Convenções

- **Formato dos planos:** estrutura, frontmatter e nomenclatura em [formato_plan.plan.md](formato_plan.plan.md).
- **Guia completo:** quando criar, como acionar, uso pela IA em [guia_plans.plan.md](guia_plans.plan.md).
- **Manutenção:** ao criar ou alterar um plano de forma relevante, atualize a tabela do índice neste plano.
