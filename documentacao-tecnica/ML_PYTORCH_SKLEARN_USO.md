# Uso de PyTorch, scikit-learn, TensorFlow, JAX e Hugging Face no projeto

Scripts implementados e como chamar (linha de comando e n8n). O `requirements.txt` inclui ainda **TensorFlow**, **JAX** e **Hugging Face Transformers** para uso em novos scripts (classificação, sumarização, NER, modelos Keras/JAX).

---

## Ambiente

Foi criado um **virtualenv** no workspace para não poluir o Python do sistema:

```bash
cd /root/.openclaw/workspace
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
```

No servidor **sem GPU**, instale PyTorch em modo CPU depois (menor download):

```bash
.venv/bin/pip install torch --index-url https://download.pytorch.org/whl/cpu
```

Use sempre o Python do venv para rodar os scripts: **`.venv/bin/python3`**.

---

## Scripts implementados

### 1. Segmentação (clustering) — scikit-learn

**Arquivo:** `tools/ml_dores_segmentacao.py`

Adiciona uma coluna **segmento** (cluster 0..K-1) ao CSV de dores, usando TF-IDF + KMeans. Útil para agrupar dores por similaridade de texto (dor_base + solucao_graventum).

**Uso:**

```bash
cd /root/.openclaw/workspace
.venv/bin/python3 tools/ml_dores_segmentacao.py
# Saída padrão: outputs/datasets/dores_com_segmento.csv

.venv/bin/python3 tools/ml_dores_segmentacao.py --csv outputs/datasets/consolidated_dores_solucoes.csv --out outputs/datasets/dores_com_segmento.csv --clusters 8
```

**Parâmetros:** `--csv`, `--out`, `--clusters` (default 8), `--max-features` (default 500).

**Saída:** mesmo CSV com coluna extra `segmento` (0 a clusters-1). Contagem por segmento é impressa no terminal.

---

### 2. Embeddings para RAG — sentence-transformers (PyTorch)

**Arquivo:** `tools/ml_embeddings.py`

Gera vetores (embeddings) de textos para uso em busca semântica ou gravação no pgvector (Supabase). Modelo padrão: multilingue (inclui português).

**Uso:**

```bash
cd /root/.openclaw/workspace
.venv/bin/python3 tools/ml_embeddings.py --csv outputs/datasets/consolidated_dores_solucoes.csv --text-col dor_base --out outputs/embeddings_dores.json
```

Para um único texto (ex.: pipeline ou n8n):

```bash
echo "Conciliação bancária demorada" | .venv/bin/python3 tools/ml_embeddings.py --stdin
# Saída JSON no stdout: [{ "text": "...", "embedding": [ ... ] }]
```

**Parâmetros:** `--csv`, `--text-col` (default `dor_base`), `--out`, `--stdin`, `--model`, `--batch`.

**Saída:** JSON com `id`, `text`, `embedding` (lista de floats). Pode ser usado para popular uma tabela com coluna vector no Supabase.

---

## Chamada a partir do n8n

### Opção A — Executar comando (nó "Execute Command")

1. Adicione um nó **Execute Command**.
2. Comando (ajuste o caminho se o n8n rodar em outro diretório):

   **Segmentação:**
   ```text
   /root/.openclaw/workspace/.venv/bin/python3 /root/.openclaw/workspace/tools/ml_dores_segmentacao.py --clusters 6
   ```

   **Embeddings de um texto (ex.: campo do payload):**
   ```text
   /root/.openclaw/workspace/.venv/bin/python3 /root/.openclaw/workspace/tools/ml_embeddings.py --stdin
   ```
   No nó anterior, passe o texto para o stdin (ex.: redirecionar com `echo "{{ $json.texto }}" |` no shell ou usando um nó que escreva no stdin).

3. Saída do script (stdout) pode ser parseada no nó seguinte (ex.: JSON).

### Opção B — API (graventum-api)

Criar rotas que chamem os scripts por baixo, por exemplo:

- `POST /ml/segmentar` — body com CSV ou caminho; resposta com CSV + segmentos ou contagem.
- `POST /ml/embed` — body `{ "text": "..." }` ou `{ "texts": ["...", "..."] }`; resposta `{ "embedding": [...] }` ou `{ "embeddings": [...] }`.

A API rodaria no mesmo servidor e usaria `child_process` ou um módulo Python (subprocess) para chamar `.venv/bin/python3 tools/ml_*.py` e devolver o resultado em JSON.

---

## Onde estão os arquivos

| Item | Caminho |
|------|---------|
| Dependências | `requirements.txt` (raiz do workspace) |
| Venv | `.venv/` (raiz do workspace) |
| Segmentação (scikit-learn) | `tools/ml_dores_segmentacao.py` |
| Embeddings (PyTorch / sentence-transformers) | `tools/ml_embeddings.py` |
| CSV com segmentos (exemplo) | `outputs/datasets/dores_com_segmento.csv` |
| Benefícios e papel no projeto (incl. TF, JAX, Hugging Face) | `docs/PYTORCH_SCIKIT_LEARN_SERVIDOR.md` |
