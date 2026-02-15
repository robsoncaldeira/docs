---
name: Teste Ralph Wiggum Ollama DeepSeek
overview: Plano de teste para validar o fluxo Open Ralph Wiggum + OpenCode + Ollama + DeepSeek, com critérios de sucesso e fallbacks.
todos: []
isProject: false
---

# Plano de Teste: Ralph Wiggum + OpenCode + Ollama + DeepSeek

## Objetivo

Validar se o loop autônomo de código funciona com a stack local (Ollama + DeepSeek) antes de considerar uso em produção ou para desenvolvimento interno da Graventum.

**Garantia:** Todos os testes rodam em `/tmp/` — **nenhum arquivo do projeto atual é alterado.**

---

## Pré-requisitos (Checklist)

- **Ollama** instalado e rodando (`ollama serve`)
- **Node.js** v20+ (para OpenCode)
- **Bun** instalado (requisito do Ralph Wiggum)
- **Modelo DeepSeek** no Ollama: `ollama pull deepseek-coder` (ou deepseek-r1 se tiver VRAM)

---

## Fase 1: Setup (30 min)

### 1.1 Instalar dependências

```bash
# Open Ralph Wiggum
npm install -g @th0rgal/ralph-wiggum

# OpenCode (agente padrão)
npm install -g opencode-ai
```

### 1.2 Configurar OpenCode para Ollama

Criar/editar `~/.config/opencode/opencode.json`:

```json
{
  "model": "ollama/deepseek-coder",
  "providers": {
    "ollama": {
      "baseURL": "http://localhost:11434/v1"
    }
  }
}
```

### 1.3 Validar Ollama

```bash
ollama list                    # Ver modelos instalados
ollama run deepseek-coder "Hi" # Teste rápido
curl http://localhost:11434/v1/models  # API respondendo
```

---

## Ambiente Isolado (Mock) — Zero impacto no projeto

**Todos os testes rodam em `/tmp/**` — nenhum arquivo do workspace é alterado.


| Diretório         | Uso                                 | Pode deletar? |
| ----------------- | ----------------------------------- | ------------- |
| `/tmp/ralph-test` | Teste trivial (Fase 2)              | Sim           |
| `/tmp/ralph-mock` | Mock de projeto com testes (Fase 3) | Sim           |


---

## Fase 2: Teste Mínimo (15 min)

### 2.1 Tarefa trivial — sandbox isolado

```bash
mkdir -p /tmp/ralph-test && cd /tmp/ralph-test
git init

ralph "Crie um arquivo hello.js que exporta uma função hello() retornando 'Hello World'. Output COMPLETE quando concluído." \
  --agent opencode \
  --model ollama/deepseek-coder \
  --max-iterations 5
```

**Critério de sucesso:** Arquivo criado, loop termina com COMPLETE em ≤5 iterações.

### 2.2 Se falhar

- Verificar logs do OpenCode
- Testar `opencode` direto (sem ralph): `opencode "crie hello.js"`
- Se Ollama lento: tentar `ollama/deepseek-coder:6.7b` (versão menor)

---

## Fase 3: Teste com Verificação (30 min)

### 3.1 Projeto com testes

Usar repositório existente da Graventum (ex.: `graventum-api`) ou criar um mini-projeto:

```bash
cd /root/graventum-api  # ou projeto de teste
ralph "Adicione um teste unitário para o endpoint /health. Rode npm test após cada alteração. Output COMPLETE quando todos os testes passarem." \
  --agent opencode \
  --model ollama/deepseek-coder \
  --max-iterations 10
```

**Critério de sucesso:** Teste criado, `npm test` passa, loop termina.

### 3.2 Monitoramento

Em outro terminal: `ralph --status` (mostra iteração atual, histórico, struggle indicators).

---

## Fase 4: Teste com DeepSeek API (Opcional)

Se Ollama for lento ou instável, validar com DeepSeek via API:

```bash
# Configurar OpenCode com DeepSeek API (não Ollama)
# Em opencode.json: model: "deepseek/deepseek-chat"
ralph "Refatore a função X para usar async/await. Output COMPLETE quando concluído." \
  --agent opencode \
  --model deepseek/deepseek-chat \
  --max-iterations 5
```

---

## Critérios de Sucesso do Plano


| Critério                                                     | Passa? |
| ------------------------------------------------------------ | ------ |
| OpenCode conecta ao Ollama                                   |        |
| deepseek-coder responde em tempo aceitável (<2 min/iteração) |        |
| Loop completa tarefa trivial em ≤5 iterações                 |        |
| Loop completa tarefa com testes em ≤10 iterações             |        |
| `ralph --status` funciona durante execução                   |        |


---

## Riscos e Mitigações


| Risco                        | Mitigação                                                    |
| ---------------------------- | ------------------------------------------------------------ |
| VRAM insuficiente            | Usar deepseek-coder:6.7b ou phi3:mini                        |
| OpenCode não encontra Ollama | Verificar baseURL, porta 11434                               |
| Loop não detecta COMPLETE    | Usar `--completion-promise "DONE"` se necessário             |
| Bun não instalado            | `curl -fsSL [https://bun.sh/install](https://bun.sh/install) |


---

## Decisão Pós-Teste

- **Se passar:** Documentar em `docs-repo-https/tech/` e avaliar uso para desenvolvimento interno (graventum-api, scripts).
- **Se falhar:** Manter foco em tração (primeiro cliente); Ralph Wiggum permanece como "avaliar no futuro" conforme plano de análise.

---

## Relatório de Execução (14/02/2026)

### Processo executado


| Item             | Status                                                          |
| ---------------- | --------------------------------------------------------------- |
| Ollama           | ✅ Instalado, rodando                                            |
| Modelos          | phi3:mini (deepseek-coder não disponível no teste)              |
| Bun              | ✅ Instalado                                                     |
| Ralph + OpenCode | Instalados em /tmp/ralph-test                                   |
| Loop             | Iniciou (Iteration 1/5)                                         |
| Resultado        | phi3:mini lento (>3 min); usar deepseek-coder para testes reais |


### Custos de API


| Abordagem    | Custo                            | Uso                      |
| ------------ | -------------------------------- | ------------------------ |
| Ollama local | R$0 em API                       | Volume alto, privacidade |
| DeepSeek API | ~$0.28/1M input, $0.42/1M output | Custo baixo              |
| Claude       | ~$3-15/1M tokens                 | Máxima qualidade         |


### Necessidade Graventum: **Não agora**

- Core business: automação PMEs, não dev de software
- Fase: pré-receita, foco em primeiro cliente
- Reavaliar quando: 2+ clientes, demanda por dev acelerado

### Onde empregar (se precisar)

- graventum-api, scripts, dashboards
- Stack recomendada: Ollama + deepseek-coder (local)

**Relatório completo:** [outputs/reports/2026-02-14-ralph-wiggum-teste-e-analise-custos.md](/root/.openclaw/workspace/outputs/reports/2026-02-14-ralph-wiggum-teste-e-analise-custos.md)

---

## Arquivos de Referência

- [Open Ralph Wiggum](https://github.com/Th0rgal/open-ralph-wiggum)
- [OpenCode + Ollama](https://docs.ollama.com/integrations/opencode)
- Plano anterior: [análise_graventum_vs_ralph_clawdbot_n8n_534ceff3.plan.md](/root/.cursor/plans/análise_graventum_vs_ralph_clawdbot_n8n_534ceff3.plan.md)

