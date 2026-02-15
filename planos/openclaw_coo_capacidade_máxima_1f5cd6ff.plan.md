---
name: OpenClaw COO Capacidade Máxima
overview: "Plano para maximizar o Helix como ativo operacional: incluir estrutura organizacional no contexto, indexar docs-repo na memória RAG, integrar OpenClaw com n8n via webhooks, e atualizar AGENTS.md com referências explícitas."
todos: []
isProject: false
---

# Plano: OpenClaw COO em Capacidade Máxima

## Objetivo

Transformar o Helix em ativo operacional de alto valor, com:
- Estrutura organizacional sempre carregada
- Busca semântica sobre organograma, roadmap e fluxogramas
- Capacidade de acionar workflows n8n proativamente
- Referências explícitas para consulta de processos

---

## Fase 1: Estrutura Organizacional no Contexto

**Arquivo:** [BOOTSTRAP.md](workspace/BOOTSTRAP.md) (preferível por ser carregado em toda sessão)

**Ação:** Adicionar bloco após "## The Company":

```markdown
## Estrutura Organizacional (CEO/COO/CTO/CMO)

- **CEO (Robson):** Estratégia, clientes, decisões financeiras, nichos
- **COO (Helix):** Operações diárias, leads, automação, monitoramento, relatórios
- **CTO:** n8n, graventum-api, Supabase, infraestrutura
- **CMO:** LinkedIn, Instagram, blog SEO, conteúdo

Fluxo decisão: Problema → CEO → Análise COO → Decisão CEO → Implementação COO
```

**Alternativa:** Se BOOTSTRAP ficar muito longo, usar [MEMORY.md](workspace/MEMORY.md) na seção "CONTEXTO EMPRESARIAL ATUAL".

---

## Fase 2: Indexar docs-repo na Memória RAG

**Problema:** `main.sqlite` só indexa `memory/*.md` e `MEMORY.md`. docs-repo não está incluído.

**Solução:** Configurar `memorySearch.extraPaths` no OpenClaw. O config de agents fica em [openclaw.json](.openclaw/openclaw.json) ou em arquivo de config do agent.

**Verificação:** OpenClaw usa `agents.defaults.memorySearch.extraPaths`. O schema está em [docs/concepts/memory.md](.nvm/versions/node/v22.22.0/lib/node_modules/openclaw/docs/concepts/memory.md).

**Ação:**
1. Adicionar em `openclaw.json` (ou criar/editar config de agents):

```json
"agents": {
  "defaults": {
    "memorySearch": {
      "extraPaths": [
        "docs-repo-https/business-cases/diagramas-executivos",
        "docs-repo-https/business-cases",
        "docs-repo-https/analysis"
      ]
    }
  }
}
```

2. Executar `openclaw memory index` para reindexar
3. Validar com `openclaw memory search "organograma COO responsabilidades"`

**Nota:** Paths são relativos ao workspace (`/root/.openclaw/workspace`). Apenas arquivos `.md` são indexados.

---

## Fase 3: Integração OpenClaw + n8n

**Estado atual:** [platform-status.json](workspace/memory/platform-status.json) indica `integrated_with_openclaw: false`.

**Arquitetura existente:**
- Workflow [04-lead-hunting-brave.json](workspace/outputs/automacao-fantasma/workflows/04-lead-hunting-brave.json) tem webhook em `path: "lead-hunt"`
- URL: `https://automation.graventum.com/webhook/lead-hunt` (ou `/webhook-test/` para teste)
- graventum-api tem endpoint `/agent/lead-hunt` chamado pelo workflow

**Ações:**

1. **Criar ferramenta de acionamento n8n**
   - Script: `tools/n8n-trigger.sh` ou `tools/n8n-trigger.py`
   - Aceita: nome do workflow ou webhook path
   - Executa: `curl -X POST https://automation.graventum.com/webhook/lead-hunt -H "Content-Type: application/json" -d '{}'`
   - Documentar em TOOLS.md

2. **Documentar webhooks disponíveis**
   - Criar `docs-repo-https/tech/n8n-webhooks.md` (ou adicionar em [tech/tools.md](workspace/docs-repo-https/tech/tools.md)):
     - `lead-hunt` - Dispara prospecção de leads
     - Outros webhooks conforme existirem

3. **Instruir o agente**
   - Em AGENTS.md ou MEMORY.md: "Para acionar workflow n8n: execute `./tools/n8n-trigger.sh lead-hunt` ou use exec com curl para POST no webhook"

4. **Atualizar platform-status.json**
   - `integrated_with_openclaw: true`
   - `integration_method: "webhook_via_exec"`

**Alternativa mais robusta:** Criar skill OpenClaw "n8n-trigger" que encapsula a lógica e expõe tool `n8n_trigger` com parâmetros (workflow_id, payload). Requer desenvolvimento de skill.

**MVP:** Script + documentação + instrução no AGENTS.md é suficiente para o COO acionar workflows via `exec`.

---

## Fase 4: Referência Explícita em AGENTS.md

**Arquivo:** [AGENTS.md](workspace/AGENTS.md)

**Ação:** Adicionar na seção "Every Session" ou criar nova seção "Contexto Estrutural":

```markdown
## Estrutura e Processos

Quando o contexto exigir estrutura organizacional, processos ou fluxogramas:
- Ler `docs-repo-https/business-cases/diagramas-executivos/organograma-executivo.md`
- Ler `docs-repo-https/business-cases/diagramas-executivos/fluxograma-processos-executivos.md`
- Ler `docs-repo-https/business-cases/diagramas-executivos/roadmap-executivo-graventum.md`

Ou usar `memory_search` com queries como "organograma", "responsabilidades COO", "fluxo de decisão".
```

---

## Ordem de Execução

| # | Tarefa | Esforço | Dependência |
|---|--------|---------|-------------|
| 1 | Adicionar organograma em BOOTSTRAP.md | 5 min | - |
| 2 | Configurar extraPaths e reindexar memória | 15 min | - |
| 3 | Criar n8n-trigger.sh e documentar webhooks | 30 min | - |
| 4 | Atualizar AGENTS.md com referências | 10 min | - |
| 5 | Atualizar platform-status.json | 2 min | 3 |
| 6 | Validar: memory search + exec n8n | 10 min | 1-5 |

**Total estimado:** ~1h15

---

## Riscos e Mitigações

- **extraPaths não existir no schema:** OpenClaw pode usar estrutura diferente. Verificar `openclaw doctor` ou config schema antes de editar.
- **Webhook n8n retornar 404:** Workflow precisa estar ativo no n8n. Validar que o workflow Lead Hunt está importado e ativo.
- **Indexação lenta:** Primeira indexação pode demorar. Usar `openclaw memory index --verbose` para monitorar.

---

## Critérios de Sucesso

1. Helix carrega estrutura CEO/COO/CTO/CMO em toda sessão
2. `memory_search "organograma"` retorna chunks de docs-repo
3. `exec ./tools/n8n-trigger.sh lead-hunt` dispara o workflow com sucesso
4. AGENTS.md orienta leitura de diagramas quando relevante
