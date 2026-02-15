---
name: Formato de planos (.plan.md)
overview: "Especificação do formato dos arquivos de plano nesta pasta: frontmatter YAML, estrutura do corpo Markdown, nomenclatura e como tornar um plano acionável."
todos: []
isProject: false
---

# Formato de planos (.plan.md)

Este plano define o **formato padrão** dos arquivos `*.plan.md` em `.cursor/plans`. Todo plano novo deve seguir esta estrutura.

---

## 1. Extensão e nome do arquivo

- **Extensão:** sempre `.plan.md`.
- **Nomenclatura:** `nome_descritivo.plan.md` ou `nome_descritivo_identificador.plan.md`.
- **Nome descritivo:** snake_case ou slug (ex.: `carregar_csv_no_banco`, `workflows_n8n_dores_solucoes`).
- **Identificador:** sufixo opcional (hash ou id) para evitar colisão (ex.: `carregar_csv_no_banco_0b497ebf.plan.md`).

---

## 2. Frontmatter (YAML)

Todo plano começa com um bloco YAML entre `---`:

```yaml
---
name: Nome curto e legível do plano
overview: Uma ou duas frases descrevendo o objetivo e o que o plano entrega.
todos: []
isProject: false
---
```


| Campo         | Obrigatório | Descrição                                                                  |
| ------------- | ----------- | -------------------------------------------------------------------------- |
| **name**      | Sim         | Nome que aparece no índice; claro sem abrir o arquivo.                     |
| **overview**  | Sim         | Resumo em 1–2 frases; usado na coluna "Resumo" do README.                  |
| **todos**     | Não         | Lista de tarefas (ex.: `["Passo 1", "Passo 2"]`); pode ser `[]`.           |
| **isProject** | Não         | `true` se for projeto com várias fases; `false` (padrão) para plano único. |


---

## 3. Corpo (Markdown)

Ordem recomendada:

1. **Título H1** — igual ou muito próximo do `name`.
2. **Contexto** — por que existe o plano, fontes, premissas.
3. **Seções H2/H3** — numeradas ou nomeadas (ex.: "Parte 1 — SQLite", "Passos para enviar ao Supabase").
4. **Comandos** — em blocos de código, com caminhos absolutos ou relativos ao workspace quando fizer sentido.
5. **Tabela de artefatos** (opcional) — colunas "Artefato" e "Caminho" no final.

Exemplo de seção acionável:

```markdown
### 2.2 Script de envio CSV → Supabase

- **Arquivo:** `data/push_dores_solucoes_to_supabase.py`
- **Executar:** `cd /root/.openclaw/workspace && python3 data/push_dores_solucoes_to_supabase.py`
- **Variáveis:** `SUPABASE_URL` e `SUPABASE_SERVICE_ROLE_KEY` no `.env`.
```

---

## 4. Como tornar acionável

Um plano é **acionável** quando humano ou IA conseguem **fazer a próxima coisa** sem adivinhar.


| Prática             | Exemplo                                                                                                         |
| ------------------- | --------------------------------------------------------------------------------------------------------------- |
| Comando completo    | `python3 /root/.openclaw/workspace/data/push_dores_solucoes_to_supabase.py` em vez de "rodar o script de push". |
| Caminho explícito   | "Arquivo: `outputs/supabase/dores_solucoes_table.sql`" e "Executar no SQL Editor do Supabase".                  |
| Ordem clara         | "1. Criar tabela (uma vez). 2. Enviar dados."                                                                   |
| Tabela de artefatos | Coluna "Artefato" e "Caminho" no final do plano.                                                                |


Evite: "em algum momento rodar o script", "ver a documentação", sem indicar **onde** e **o quê**.

---

## 5. Manutenção


| Ação           | Onde atualizar                                                                                 |
| -------------- | ---------------------------------------------------------------------------------------------- |
| Novo plano     | Criar o `*.plan.md` com este formato; **adicionar linha no índice** do [README.md](README.md). |
| Plano alterado | Atualizar corpo; se o próximo passo mudar, atualizar "Como acionar" no README.                 |
| Plano obsoleto | Remover ou arquivar; **retirar do índice** no README.                                          |


---

## Referências


| O quê                                     | Onde                           |
| ----------------------------------------- | ------------------------------ |
| Índice de todos os planos                 | [README.md](README.md)         |
| Guia completo (quando criar, uso pela IA) | [GUIA_PLANS.md](GUIA_PLANS.md) |


