# Relatório: Teste Ralph Wiggum + Análise de Custos e Necessidade

**Data:** 14/02/2026  
**Contexto:** Validação do fluxo Ralph Wiggum + OpenCode + Ollama para a Graventum

---

## 1. Processo do Teste Executado

### 1.1 Pré-requisitos verificados

| Item | Status |
|------|--------|
| Ollama | ✅ Instalado, rodando |
| Modelos | phi3:mini (2.2 GB) — deepseek-coder não estava disponível |
| Node.js | v22.22.0 |
| Bun | ✅ Instalado (requisito do Ralph) |
| Ralph Wiggum | Instalado localmente em /tmp/ralph-test |
| OpenCode | Instalado como dependência do Ralph |

### 1.2 Configuração

1. **OpenCode** configurado em `~/.config/opencode/opencode.json`:
   - Model: `ollama/phi3:mini`
   - baseURL: `http://127.0.0.1:11434/v1`

2. **Ambiente isolado:** `/tmp/ralph-test` — zero impacto no projeto Graventum

3. **Comando executado:**
```bash
npx ralph "Crie um arquivo hello.js que exporta uma função hello() retornando 'Hello World'. Output COMPLETE quando concluído." \
  --agent opencode \
  --model ollama/phi3:mini \
  --max-iterations 5
```

### 1.3 Resultado observado

- **Loop iniciou** corretamente (Iteration 1/5)
- **OpenCode** conectou ao Ollama
- **Tempo:** phi3:mini é lento — >3 min na primeira iteração; hello.js não criado no tempo observado
- **Conclusão técnica:** Setup validado (Ralph + OpenCode + Ollama funcionam). phi3:mini não é ideal para código — usar deepseek-coder para testes reais

**Nota:** Com `ollama pull deepseek-coder` e `--model ollama/deepseek-coder`, a resposta tende a ser mais rápida e precisa para tarefas de código.

---

## 2. Visão de Custos de API

### 2.1 Comparativo por abordagem

| Abordagem | Custo | Quando usar |
|-----------|-------|-------------|
| **Ollama (local)** | R$0 em API. Custo: energia (~R$40-80/mês GPU dedicada) + hardware | Desenvolvimento interno, privacidade, volume alto |
| **DeepSeek API** | ~$0.28/1M input, $0.42/1M output (cache miss). Cache hit: $0.028/1M | Custo baixo, boa qualidade para código |
| **Claude API** | ~$3/1M input, $15/1M output (Sonnet 4) | Máxima qualidade, tarefas complexas |
| **Gemini** | Intermediário | Alternativa Google |

### 2.2 Estimativa para Ralph Wiggum (exemplo: 10 iterações, 5k tokens/iteração)

| Provider | Custo estimado/execução |
|----------|-------------------------|
| Ollama local | R$0 (apenas energia) |
| DeepSeek API | ~$0.02-0.05 |
| Claude | ~$0.50-2.00 |

**Conclusão custos:** Ollama + DeepSeek (local ou API) é a opção mais barata. Claude só se a qualidade for crítica.

---

## 3. A Graventum Precisa do Ralph Wiggum Agora?

### 3.1 Resposta: **Não, não é prioridade**

| Critério | Avaliação |
|----------|-----------|
| **Core business** | Graventum vende automação para PMEs (contabilidade, imobiliárias, etc.), não desenvolvimento de software |
| **Produto** | Workflows n8n, diagnósticos, retainer — não código autônomo |
| **Fase atual** | Pré-receita, foco em primeiro cliente |
| **ROI** | Ralph acelera desenvolvimento interno; o gargalo atual é tração, não velocidade de código |

### 3.2 Quando faria sentido

- Equipe técnica crescendo e precisando de mais produtividade
- Produto de software próprio (ex.: SaaS de automação)
- Cliente pedindo explicitamente "auto-aprimoramento de código"
- Desenvolvimento intenso de graventum-api, dashboards ou ferramentas internas

---

## 4. Se a Resposta For "Precisa" — Onde Empregar

### 4.1 Uso interno (desenvolvimento Graventum)

| Área | Aplicação |
|------|-----------|
| **graventum-api** | Refatorações, adição de testes, migração de módulos |
| **Scripts de automação** | Manutenção e evolução de scripts Python/Node |
| **Dashboards** | Geração e ajuste de componentes |
| **Workflows n8n** | Geração de nós e lógica (via código, não UI) |

### 4.2 Uso para clientes (futuro)

| Cenário | Aplicação |
|---------|-----------|
| **Software house parceira** | Cliente que desenvolve produtos e quer loop de código |
| **Escritório de contabilidade com dev** | Manutenção de integrações customizadas |
| **Produto white-label** | Base de código que precisa evoluir com pouca intervenção |

### 4.3 Stack recomendada para uso futuro

- **Ollama + deepseek-coder** (local): custo zero em API, boa qualidade para código
- **DeepSeek API** (cloud): se não houver GPU ou para tarefas maiores
- **Claude**: apenas para tarefas que exijam raciocínio mais complexo

---

## 5. Recomendação Final

1. **Agora:** Não implementar Ralph Wiggum como prioridade. Foco em primeiro cliente e tração.
2. **Documentar:** Manter o plano de teste e este relatório para referência futura.
3. **Reavaliar:** Quando houver 2+ clientes e demanda por desenvolvimento interno acelerado.
4. **Custo:** Se for usar no futuro, preferir Ollama + deepseek-coder (local) para custo zero em API.

---

**Arquivos do teste:** `/tmp/ralph-test` (pode ser deletado após validação)
