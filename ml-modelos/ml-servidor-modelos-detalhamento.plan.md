---
name: ML no servidor — Modelos, scripts e documentação (detalhamento completo)
overview: "Tudo que foi feito em relação a modelos e ML no servidor: bibliotecas (PyTorch, scikit-learn, TensorFlow, JAX, Hugging Face), scripts (segmentação e embeddings), ambiente, benefícios, instalação, uso (CLI e n8n) e artefatos."
todos: []
isProject: false
---

# ML no servidor — Modelos, scripts e documentação (detalhamento completo)

Este plano consolida **tudo que foi feito** em termos de modelos e ML no servidor Graventum/OpenClaw e detalha o conteúdo dos docs de uso e instalação.

---

## 1. O que foi feito (resumo executivo)


| Item                                      | Onde está                                    | Descrição                                                                                                                                                                                                |
| ----------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dependências (requirements.txt)**       | `/root/.openclaw/workspace/requirements.txt` | requests, scikit-learn, torch, sentence-transformers, transformers, accelerate, tensorflow, jax, jaxlib. Comentários para PyTorch/TF CPU.                                                                |
| **Ambiente virtual**                      | `/root/.openclaw/workspace/.venv`            | Venv completo. **Instalado:** scikit-learn, torch (CPU), sentence-transformers, transformers, accelerate, tensorflow, jax, jaxlib, requests. Scripts de segmentação e embeddings testados e funcionando. |
| **Script segmentação**                    | `tools/ml_dores_segmentacao.py`              | TF-IDF + KMeans (scikit-learn) sobre dor_base + solucao_graventum; adiciona coluna `segmento` ao CSV. Testado.                                                                                           |
| **Script embeddings**                     | `tools/ml_embeddings.py`                     | sentence-transformers (PyTorch); modelo padrão `paraphrase-multilingual-MiniLM-L12-v2` (multilingue, PT). Saída JSON para pgvector/RAG.                                                                  |
| **Saída segmentação (exemplo)**           | `outputs/datasets/dores_com_segmento.csv`    | CSV com coluna `segmento` (0..K-1).                                                                                                                                                                      |
| **Doc uso (CLI, n8n)**                    | `docs/ML_PYTORCH_SKLEARN_USO.md`             | Ambiente, scripts, parâmetros, chamada n8n (Execute Command), ideia de rotas API.                                                                                                                        |
| **Doc servidor (benefícios, instalação)** | `docs/PYTORCH_SCIKIT_LEARN_SERVIDOR.md`      | Benefícios por lib, stack, instalação (global/venv), verificação, entrega ao projeto.                                                                                                                    |
| **Índice do projeto**                     | `/root/README-PROJETO.md`                    | Referências a ML (docs e scripts).                                                                                                                                                                       |


**Modelos/algoritmos efetivamente usados nos scripts:**

- **Segmentação:** `TfidfVectorizer` (max_features=500, min_df=2, max_df=0.95) + `KMeans` (n_clusters configurável, default 8). Sem modelo externo; só scikit-learn.
- **Embeddings:** modelo Hugging Face `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2` (baixado na primeira execução; roda em CPU).

---

## 2. Benefícios e papel no projeto (por biblioteca)

### scikit-learn


| Benefício                    | Uso no projeto Graventum                                                                                          |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Classificação e scoring      | Lead scoring, triagem de dores/soluções, classificação de e-mails ou tickets sem chamar Claude em todo item.      |
| Clustering e segmentação     | Agrupar leads ou dores por perfil (contabilidade vs imobiliária vs clínicas), enriquecer CRM e n8n com segmentos. |
| Pré-processamento e features | TF-IDF, contagens; alimentar modelos ou Claude com resumos estruturados.                                          |
| Modelos leves e rápidos      | Rodar no servidor (CPU), sem custo de API; filtros em massa antes de enviar só casos “quentes” ao Claude.         |


**Onde entra:** scripts chamados pelo n8n ou graventum-api (“score deste lead”, “qual segmento”), análise dos datasets dores/soluções.

### PyTorch


| Benefício                             | Uso no projeto Graventum                                                                            |
| ------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Embeddings e modelos de texto         | Embeddings locais para RAG, busca semântica e classificação no Supabase/pgvector.                   |
| Modelos de classificação customizados | Treinar ou ajustar modelo leve em cima de dores, soluções, leads; inferência no servidor.           |
| Inferência de modelos pequenos        | sentence-transformers, BERT pequeno para similaridade ou categorização sem depender 100% do Claude. |


**Onde entra:** pipelines ML no servidor (scripts ou serviços) chamados pelo n8n ou API; RAG com embeddings próprios.

### Hugging Face Transformers


| Benefício                          | Uso no projeto Graventum                                                                                      |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Modelos pré-treinados (BERT, etc.) | Classificação, NER, sumarização, Q&A e embeddings; backend PyTorch ou TensorFlow.                             |
| Pipeline de alto nível             | `pipeline("sentiment-analysis")`, `pipeline("ner")`, `pipeline("summarization")` para prototipagem e n8n/API. |
| Tokenizadores e multilingues       | Modelos leves (distilbert, etc.) para rodar em CPU.                                                           |


**Onde entra:** classificação de mensagens/leads, extração de entidades, sumarização antes de enviar ao Claude.

### TensorFlow


| Benefício               | Uso no projeto Graventum                                                |
| ----------------------- | ----------------------------------------------------------------------- |
| Keras e treino          | Treinar ou carregar modelos Keras/TF; deploy com TF Serving (opcional). |
| Integração Hugging Face | `transformers` com backend TF.                                          |
| TF Lite / CPU           | No servidor sem GPU: `tensorflow-cpu` para instalação menor.            |


**Onde entra:** modelos existentes em TF/Keras; Hugging Face com backend TF.

### JAX


| Benefício                      | Uso no projeto Graventum                                  |
| ------------------------------ | --------------------------------------------------------- |
| Computação numérica e autodiff | Pesquisa, experimentos, treino customizado (GPU/TPU/CPU). |
| Flax, Optax                    | Redes neurais e otimizadores em JAX.                      |
| Leve em CPU                    | `pip install jax jaxlib` sem CUDA.                        |


**Onde entra:** experimentos e modelos customizados em JAX/Flax.

### Resumo: como fazem parte do projeto

- **Hoje:** Claude + OpenAI embeddings + n8n + Supabase fazem a maior parte.
- **Com scikit-learn:** scoring, clustering e modelos leves no servidor; menos chamadas à API; segmentação/priorização automática.
- **Com PyTorch:** embeddings e modelos de texto locais; RAG ou classificação offline ou mais barata.
- Não substituem o Claude; **complementam** — modelos baratos/rápidos no servidor + Claude quando precisar de raciocínio ou texto rico.

---

## 3. Onde encaixar na stack atual


| Camada                    | Como ML entra                                                                                        |
| ------------------------- | ---------------------------------------------------------------------------------------------------- |
| n8n                       | Execute Command (script Python) ou chamada a endpoint da graventum-api que usa scikit-learn/PyTorch. |
| graventum-api             | Rotas como `/score-lead`, `/segmentar`, `/embed` que chamam os scripts e devolvem JSON.              |
| Supabase / pgvector       | Vetores gerados por PyTorch/sentence-transformers/Hugging Face armazenados e usados na busca RAG.    |
| Datasets (dores/soluções) | scikit-learn para clustering; PyTorch/sentence-transformers para embeddings.                         |


Fluxo exemplo: **Lead chega** → n8n chama script ou API (scikit-learn score + segmento) → “quente” vai para Claude, “frio” para nutrição. **Documento novo** → script gera embedding → grava no Supabase → RAG usa o vetor.

---

## 4. Ambiente e instalação no servidor

### 4.1 Virtualenv (recomendado)

```bash
cd /root/.openclaw/workspace
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
```

No servidor **sem GPU**, PyTorch em modo CPU (menor download):

```bash
.venv/bin/pip install torch --index-url https://download.pytorch.org/whl/cpu
```

Usar sempre: `**.venv/bin/python3**`.

### 4.2 Opção ambiente global

```bash
pip3 install scikit-learn
pip3 install torch --index-url https://download.pytorch.org/whl/cpu
```

### 4.3 Verificar instalação

```bash
python3 -c "import sklearn; print(sklearn.__version__)"
python3 -c "import torch; print(torch.__version__)"
python3 -c "import transformers; print(transformers.__version__)"
python3 -c "import tensorflow as tf; print(tf.__version__)"
python3 -c "import jax; print(jax.__version__)"
```

### 4.4 Instalações mais leves (servidor sem GPU)

```bash
# PyTorch só CPU
pip install torch --index-url https://download.pytorch.org/whl/cpu

# TensorFlow só CPU
pip install tensorflow-cpu
# Ou: pip uninstall tensorflow && pip install tensorflow-cpu
```

### 4.5 Pode colocar no servidor?

**Sim.** Tudo roda em Linux (ex.: Ubuntu 24.04). scikit-learn: só CPU, leve. PyTorch: build CPU. Hugging Face Transformers: usa PyTorch (ou TF); modelos pequenos em CPU. TensorFlow: `tensorflow-cpu`. JAX: `jax` + `jaxlib` em CPU.

---

## 5. Scripts implementados — uso detalhado

### 5.1 Segmentação (clustering) — scikit-learn

**Arquivo:** `/root/.openclaw/workspace/tools/ml_dores_segmentacao.py`

Adiciona coluna **segmento** (cluster 0..K-1) ao CSV de dores usando **TF-IDF + KMeans** sobre `dor_base` + `solucao_graventum`.

**Parâmetros:** `--csv` (entrada), `--out` (saída), `--clusters` (default 8), `--max-features` (default 500).

**Uso:**

```bash
cd /root/.openclaw/workspace
.venv/bin/python3 tools/ml_dores_segmentacao.py
# Saída padrão: outputs/datasets/dores_com_segmento.csv

.venv/bin/python3 tools/ml_dores_segmentacao.py --csv outputs/datasets/consolidated_dores_solucoes.csv --out outputs/datasets/dores_com_segmento.csv --clusters 8
```

**Saída:** mesmo CSV com coluna `segmento`; contagem por segmento no terminal.

### 5.2 Embeddings para RAG — sentence-transformers (PyTorch)

**Arquivo:** `/root/.openclaw/workspace/tools/ml_embeddings.py`

Gera vetores de texto para busca semântica ou pgvector (Supabase). **Modelo padrão:** `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2` (multilingue, inclui português).

**Parâmetros:** `--csv`, `--text-col` (default `dor_base`), `--out`, `--stdin`, `--model`, `--batch` (default 32).

**Uso com CSV:**

```bash
cd /root/.openclaw/workspace
.venv/bin/python3 tools/ml_embeddings.py --csv outputs/datasets/consolidated_dores_solucoes.csv --text-col dor_base --out outputs/embeddings_dores.json
```

**Uso com um texto (stdin), ex.: n8n:**

```bash
echo "Conciliação bancária demorada" | .venv/bin/python3 tools/ml_embeddings.py --stdin
# Saída JSON: [{ "text": "...", "embedding": [ ... ] }]
```

**Saída:** JSON com `id`, `text`, `embedding` (lista de floats); compatível com tabela vector no Supabase.

---

## 6. Chamada a partir do n8n

### Opção A — Execute Command

1. Nó **Execute Command**.
2. **Segmentação:**
  ```text
   /root/.openclaw/workspace/.venv/bin/python3 /root/.openclaw/workspace/tools/ml_dores_segmentacao.py --clusters 6
  ```
3. **Embeddings (texto do payload):**
  ```text
   /root/.openclaw/workspace/.venv/bin/python3 /root/.openclaw/workspace/tools/ml_embeddings.py --stdin
  ```
   Passar o texto para o stdin (ex.: `echo "{{ $json.texto }}" |` no shell ou nó que escreve no stdin).
4. Saída (stdout) pode ser parseada no nó seguinte (ex.: JSON).

### Opção B — API (graventum-api)

Rotas sugeridas que chamem os scripts por baixo:

- `POST /ml/segmentar` — body com CSV ou caminho; resposta com CSV + segmentos ou contagem.
- `POST /ml/embed` — body `{ "text": "..." }` ou `{ "texts": ["...", "..."] }`; resposta `{ "embedding": [...] }` ou `{ "embeddings": [...] }`.

A API usaria subprocess para `.venv/bin/python3 tools/ml_*.py` e devolver JSON.

---

## 7. Como entregar ao projeto

1. **Registrar:** manter `requirements.txt` no repo (workspace); qualquer clone/deploy sabe das libs.
2. **Scripts:** rodar sempre com o mesmo ambiente (`.venv` ou global), ex.: `source .venv/bin/activate && python3 tools/ml_dores_segmentacao.py`.
3. **Documentar:** docs em `docs/ML_PYTORCH_SKLEARN_USO.md` e `docs/PYTORCH_SCIKIT_LEARN_SERVIDOR.md`; referências em README-PROJETO.md.
4. **Produção:** servidor — `pip install -r requirements.txt` (e venv ou PyTorch CPU conforme doc). Docker — `COPY requirements.txt` e `RUN pip install -r requirements.txt`.

---

## 8. Tabela de artefatos (onde está cada coisa)


| Artefato                              | Caminho                                                             |
| ------------------------------------- | ------------------------------------------------------------------- |
| Dependências                          | `/root/.openclaw/workspace/requirements.txt`                        |
| Venv                                  | `/root/.openclaw/workspace/.venv/`                                  |
| Segmentação (scikit-learn)            | `/root/.openclaw/workspace/tools/ml_dores_segmentacao.py`           |
| Embeddings (sentence-transformers)    | `/root/.openclaw/workspace/tools/ml_embeddings.py`                  |
| CSV com segmentos (exemplo)           | `/root/.openclaw/workspace/outputs/datasets/dores_com_segmento.csv` |
| Doc uso (CLI, n8n)                    | `/root/.openclaw/workspace/docs/ML_PYTORCH_SKLEARN_USO.md`          |
| Doc servidor (benefícios, instalação) | `/root/.openclaw/workspace/docs/PYTORCH_SCIKIT_LEARN_SERVIDOR.md`   |
| Índice do projeto                     | `/root/README-PROJETO.md`                                           |


---

## 9. Como acionar

- **Rodar segmentação:** `cd /root/.openclaw/workspace && .venv/bin/python3 tools/ml_dores_segmentacao.py --clusters 8`
- **Rodar embeddings (um texto):** `echo "Seu texto" | .venv/bin/python3 tools/ml_embeddings.py --stdin`
- **Consultar uso e n8n:** abrir `docs/ML_PYTORCH_SKLEARN_USO.md`
- **Consultar benefícios e instalação:** abrir `docs/PYTORCH_SCIKIT_LEARN_SERVIDOR.md`

