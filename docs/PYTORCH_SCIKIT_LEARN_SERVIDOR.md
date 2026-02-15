# PyTorch, scikit-learn, TensorFlow, JAX e Hugging Face no servidor

Como colocar **PyTorch**, **scikit-learn**, **TensorFlow**, **JAX** e **Hugging Face Transformers** no servidor, **quais os benefícios** e **como entram no projeto** Graventum/OpenClaw.

---

## 1. Benefícios e papel no projeto

### scikit-learn

| Benefício | Uso no projeto Graventum |
|-----------|---------------------------|
| **Classificação e scoring** | Lead scoring (priorizar quem abordar), triagem de dores/soluções, classificação de e-mails ou tickets sem chamar Claude em todo item. |
| **Clustering e segmentação** | Agrupar leads ou dores por perfil (contabilidade vs imobiliária vs clínicas), enriquecer o CRM e os workflows n8n com segmentos. |
| **Pré-processamento e features** | Pipelines de texto (TF-IDF, contagens) ou numéricos para alimentar modelos ou o próprio Claude com resumos estruturados. |
| **Modelos leves e rápidos** | Rodar no servidor (CPU), sem custo de API; útil para filtros em massa antes de enviar só os casos “quentes” ao Claude. |

**Onde entra:** scripts Python chamados pelo n8n ou pela graventum-api (ex.: “score deste lead”, “qual segmento”), análise dos datasets de dores/soluções, relatórios de segmentação.

### PyTorch

| Benefício | Uso no projeto Graventum |
|-----------|---------------------------|
| **Embeddings e modelos de texto** | Gerar embeddings locais (alternativa ou complemento ao OpenAI) para RAG, busca semântica e classificação no Supabase/pgvector. |
| **Modelos de classificação customizados** | Treinar ou ajustar um modelo leve em cima dos seus dados (dores, soluções, leads) e usar em inferência rápida no servidor. |
| **Inferência de modelos pequenos** | Rodar modelos pré-treinados (ex.: BERT pequeno, sentence-transformers) para similaridade ou categorização sem depender 100% do Claude. |

**Onde entra:** pipelines de ML no servidor (scripts ou serviços) que o n8n ou a API chamam; RAG com embeddings próprios; classificação em lote de documentos ou mensagens.

### Resumo: como fazem parte do projeto

- **Hoje:** Claude + OpenAI embeddings + n8n + Supabase fazem a maior parte (classificação, RAG, workflows).
- **Com scikit-learn:** você ganha **scoring, clustering e modelos leves** rodando no seu servidor, reduzindo chamadas à API e dando segmentação/priorização automática.
- **Com PyTorch:** você ganha **embeddings e modelos de texto locais**, RAG ou classificação que podem funcionar offline ou com custo menor, integrados aos mesmos workflows (n8n, API).

Ou seja: não substituem o Claude, **complementam** — modelos baratos/rápidos no servidor + Claude quando precisar de raciocínio ou texto rico.

### Hugging Face Transformers

| Benefício | Uso no projeto Graventum |
|-----------|---------------------------|
| **Modelos pré-treinados (BERT, etc.)** | Classificação de texto, NER, sumarização, Q&A e embeddings usando o ecossistema Hugging Face; funciona com backend PyTorch (ou TensorFlow). |
| **Pipeline de alto nível** | `pipeline("sentiment-analysis")`, `pipeline("ner")`, `pipeline("summarization")` para prototipagem rápida e integração em n8n ou API. |
| **Tokenizadores e modelos multilingues** | Mesmos modelos que o sentence-transformers pode usar; opção de modelos mais leves (distilbert, etc.) para rodar em CPU. |

**Onde entra:** classificação de mensagens/leads, extração de entidades, sumarização de textos antes de enviar ao Claude, ou como alternativa ao sentence-transformers para embeddings (modelos diferentes).

### TensorFlow

| Benefício | Uso no projeto Graventum |
|-----------|---------------------------|
| **Keras e treino de redes** | Treinar ou carregar modelos salvos em Keras/TF para classificação, regressão ou embeddings; deploy em produção com TF Serving (opcional). |
| **Modelos da comunidade** | Muitos modelos e tutoriais em TensorFlow; integração com Hugging Face via `transformers` com backend TF. |
| **TF Lite / CPU** | No servidor sem GPU, usar `tensorflow-cpu` para instalação mais leve. |

**Onde entra:** se você já tiver modelos em TensorFlow/Keras ou quiser treinar com Keras; pipelines que precisem do ecossistema TF; Hugging Face com `backend="tf"`.

### JAX

| Benefício | Uso no projeto Graventum |
|-----------|---------------------------|
| **Computação numérica e autodiff** | Pesquisa, experimentos e treino customizado com GPU/TPU ou CPU; API estilo NumPy com diferenciação automática. |
| **Flax e outros** | Flax (rede neural em JAX), Optax (otimizadores); útil para modelos customizados e publicação de artefatos em JAX. |
| **Leve em CPU** | `pip install jax jaxlib` instala versão CPU; sem necessidade de CUDA no servidor. |

**Onde entra:** experimentos e modelos customizados em JAX/Flax; quando quiser uma stack alternativa a PyTorch/TF para pesquisa ou inferência leve.

---

## 2. Onde encaixar na stack atual

| Camada atual | Como scikit-learn / PyTorch entram |
|--------------|------------------------------------|
| **n8n** | Executa script Python (scikit-learn ou PyTorch) em nós “Execute Command” ou chama um endpoint da graventum-api que usa esses modelos. |
| **graventum-api** | Nova rota ou serviço (ex.: `/score-lead`, `/segmentar`, `/embed`) que usa scikit-learn e/ou PyTorch e devolve JSON para o n8n ou front. |
| **Supabase / pgvector** | Vetores gerados por PyTorch, sentence-transformers ou Hugging Face podem ser armazenados e usados na busca RAG. |
| **Datasets (dores/soluções)** | scikit-learn para clustering; PyTorch/sentence-transformers/transformers para embeddings; Hugging Face para classificação/sumarização/NER. |

Fluxo exemplo: **Lead chega** → n8n chama script ou API que usa scikit-learn para **score + segmento** → resultado vai para o workflow (ex.: “quente” vai para Claude, “frio” para nutrição). Outro: **Documento novo** → script com PyTorch gera embedding → grava no Supabase → RAG usa esse vetor na busca.

---

## 3. Pode colocar no servidor?

**Sim.** Tudo roda em Linux (Ubuntu 24.04 no seu caso).  
- **scikit-learn:** só CPU, instalação leve.  
- **PyTorch:** no servidor **sem GPU** use a build **CPU** (menor; suficiente para modelos pequenos, embeddings, inferência).  
- **Hugging Face Transformers:** usa PyTorch (ou TensorFlow) por padrão; modelos pequenos rodam bem em CPU.  
- **TensorFlow:** no servidor sem GPU use **tensorflow-cpu** (`pip install tensorflow-cpu`) para instalação menor.  
- **JAX:** `pip install jax jaxlib` instala versão CPU; sem CUDA necessário.

---

## 4. Como instalar no servidor

### Opção A — Ambiente global (servidor compartilhado)

```bash
# scikit-learn
pip3 install scikit-learn

# PyTorch (CPU only — recomendado no servidor sem GPU)
pip3 install torch --index-url https://download.pytorch.org/whl/cpu
```

### Opção B — Virtualenv do projeto (recomendado)

```bash
cd /root/.openclaw/workspace
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

No **`requirements.txt`** (raiz do workspace) descomente:

- `scikit-learn>=1.3.0`
- Para PyTorch: use a instalação CPU abaixo (o índice CPU evita baixar CUDA no servidor).

Ou instale depois do `pip install -r requirements.txt`:

```bash
pip install torch --index-url https://download.pytorch.org/whl/cpu
```

### Verificar

```bash
python3 -c "import sklearn; print(sklearn.__version__)"
python3 -c "import torch; print(torch.__version__)"
python3 -c "import transformers; print(transformers.__version__)"
python3 -c "import tensorflow as tf; print(tf.__version__)"
python3 -c "import jax; print(jax.__version__)"
```

### Instalações opcionais mais leves (servidor sem GPU)

```bash
# PyTorch só CPU (menor download)
pip install torch --index-url https://download.pytorch.org/whl/cpu

# TensorFlow só CPU (evita pacotes CUDA)
pip install tensorflow-cpu
# Se já tiver tensorflow no requirements.txt, pode desinstalar e usar: pip uninstall tensorflow && pip install tensorflow-cpu
```

---

## 5. Como “entregar” ao seu projeto

1. **Registrar no projeto**  
   - Manter no repositório o arquivo **`requirements.txt`** na raiz do workspace (ex.: `/root/.openclaw/workspace/requirements.txt`) com as linhas de `scikit-learn` e `torch` (e, se quiser, o `--extra-index-url` para PyTorch CPU).  
   - Assim qualquer clone ou deploy do projeto sabe que precisa dessas libs.

2. **Usar nos scripts**  
   - Coloque os scripts que usam PyTorch ou scikit-learn dentro do workspace (ex.: `scripts/`, `tools/`, ou pasta do fluxo que for).  
   - No servidor, rode sempre com o mesmo ambiente (global ou o `.venv` ativado), por exemplo:  
     `source /root/.openclaw/workspace/.venv/bin/activate && python3 scripts/meu_script.py`

3. **Documentar**  
   - Este arquivo (`docs/PYTORCH_SCIKIT_LEARN_SERVIDOR.md`) já descreve onde está o `requirements.txt`, como instalar no servidor e como rodar.  
   - Se criar um script que dependa deles, vale uma linha no README ou no próprio script dizendo: “Requer: PyTorch (CPU) e scikit-learn; instalar com `pip install -r requirements.txt`”.

4. **Entrega em produção**  
   - **Servidor:** instalar dependências a partir do mesmo `requirements.txt` (com ou sem venv).  
   - **Docker:** no `Dockerfile`, fazer `COPY requirements.txt` e `RUN pip install -r requirements.txt` no mesmo ambiente em que roda o seu código.  
   - Assim PyTorch e scikit-learn ficam “entregues” ao projeto via `requirements.txt` + doc + uso nos seus scripts.

---

## 6. Resumo rápido

| O quê | Onde |
|-------|------|
| Lista de dependências | `requirements.txt` (PyTorch, scikit-learn, TensorFlow, JAX, Hugging Face) |
| Como instalar no servidor | `pip install -r requirements.txt` (use `.venv`; PyTorch/TF CPU: ver comentários no requirements e doc) |
| Scripts e uso (n8n, CLI) | [docs/ML_PYTORCH_SKLEARN_USO.md](ML_PYTORCH_SKLEARN_USO.md) |
| Como entregar ao projeto | Manter `requirements.txt` no repo, rodar scripts com o env que tem essas libs |

Se quiser, na próxima mensagem você diz se está usando **venv** ou **Docker** e eu adapto o `requirements.txt` e os comandos exatos para o seu fluxo (por exemplo só CPU no servidor).
