---
name: Guia de planos — quando criar, estrutura, acionável, IA
overview: Documentação completa sobre o que é um plano, quando criar, como estruturar, como torná-lo acionável e como a Cursor/IA usa a pasta de planos.
todos: []
isProject: false
---

# Guia de planos (`.cursor/plans`)

Documentação robusta sobre o que é um **plano**, quando criar, como estruturar e como torná-lo **acionável** para humanos e para o Cursor/IA.

**Formato em .plan.md:** a especificação do formato (frontmatter, corpo, nomenclatura) está em [formato_plan.plan.md](formato_plan.plan.md). Este plano complementa com "quando criar", "como acionar" e uso pela IA.

---

## 1. O que é um plano

Um **plano** é um documento Markdown (`.plan.md`) nesta pasta que:

- Define um **objetivo** ou entrega (projeto, relatório, especificação).
- Fornece **contexto** (por que, para quem, dependências).
- Lista **passos, comandos ou artefatos** que podem ser executados ou consultados.
- Permite que alguém (ou a IA) **implemente ou siga** o plano sem precisar redescobrir tudo.

**Não é:** apenas uma lista de ideias soltas. **É:** um documento que, lido do início ao fim, permite **acionar** trabalho (rodar script, abrir doc, implementar fluxo).

---

## 2. Quando criar um plano

| Situação | Criar plano? |
|----------|---------------|
| Nova feature ou entrega com vários passos (scripts, DB, docs) | **Sim** — um plano reúne contexto e passos. |
| Relatório ou análise que vai ser consultada de novo | **Sim** — o plano é o artefato de referência. |
| Decisão ou estudo (mercado, viabilidade, comparação) | **Sim** — conclusões e "como acionar" ficam no plano. |
| Tarefa única e óbvia (ex.: "corrigir typo") | **Opcional** — só se quiser rastrear. |
| Configuração pontual (ex.: um comando no servidor) | **Opcional** — pode ir em doc de projeto ou em outro doc. |

**Regra prática:** se você (ou a IA) precisar **retomar** o trabalho depois ou **seguir passos** definidos, vale ter um plano.

---

## 3. Estrutura recomendada

### 3.1 Frontmatter (YAML no topo)

Todo plano deve começar com um bloco YAML entre `---`:

```yaml
---
name: Nome curto e legível do plano
overview: Uma ou duas frases descrevendo o objetivo e o que o plano entrega.
todos: []
isProject: false
---
```

- **name:** aparece no índice; deve ser claro sem abrir o arquivo.
- **overview:** usado na coluna "Resumo"; deve permitir decidir se o plano é o certo.
- **todos:** pode ser `[]` ou uma lista; ferramentas ou a IA podem usar para marcar progresso.
- **isProject:** distingue plano único de "projeto" com várias entregas.

### 3.2 Corpo (Markdown)

Ordem sugerida: título H1, contexto, seções H2/H3, comandos em blocos de código, tabela de artefatos. Ver exemplo em [formato_plan.plan.md](formato_plan.plan.md).

---

## 4. Como tornar um plano acionável

Um plano é **acionável** quando um humano ou a IA consegue **fazer a próxima coisa** sem adivinhar.

| Prática | Exemplo |
|--------|---------|
| Comando completo | `python3 /root/.openclaw/workspace/data/push_dores_solucoes_to_supabase.py` em vez de "rodar o script de push". |
| Caminho explícito | "Arquivo: `outputs/supabase/dores_solucoes_table.sql`" e "Executar no SQL Editor do Supabase". |
| Um "Como acionar" por plano | No índice ([indice_e_uso_plans.plan.md](indice_e_uso_plans.plan.md)), uma linha que diz o próximo passo. |
| Ordem clara | "1. Criar tabela (uma vez). 2. Enviar dados." |
| Tabela de artefatos | Coluna "Artefato" e "Caminho" no final do plano. |

Evite: "em algum momento rodar o script", "ver a documentação", sem indicar **onde** e **o quê**.

---

## 5. Nomenclatura de arquivos

- **Formato:** `nome_descritivo.plan.md` ou `nome_descritivo_identificador.plan.md`.
- **Nome descritivo:** snake_case ou slug. **Extensão:** sempre `.plan.md`.

---

## 6. Uso pelo Cursor / IA

- **Referência direta:** "Implementa conforme `.cursor/plans/carregar_csv_no_banco_0b497ebf.plan.md`".
- **Índice:** "Quais planos existem?" → [indice_e_uso_plans.plan.md](indice_e_uso_plans.plan.md) ou listar `*.plan.md`.
- **Próximo passo:** coluna "Como acionar" no índice.

---

## 7. Manutenção

| Ação | Onde atualizar |
|------|----------------|
| Novo plano | Criar o `*.plan.md`; **adicionar linha no índice** em [indice_e_uso_plans.plan.md](indice_e_uso_plans.plan.md). |
| Plano alterado | Atualizar corpo; se o próximo passo mudar, atualizar "Como acionar" no índice. |
| Plano obsoleto | Remover ou arquivar; **retirar do índice**. |

---

## 8. Resumo rápido

| O quê | Onde / como |
|-------|-------------|
| Listar todos os planos | [indice_e_uso_plans.plan.md](indice_e_uso_plans.plan.md) |
| Formato (.plan.md) | [formato_plan.plan.md](formato_plan.plan.md) |
