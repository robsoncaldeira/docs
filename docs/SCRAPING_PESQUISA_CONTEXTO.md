# Scraping para pesquisa e contexto (BeautifulSoup)

Uso de **requests + BeautifulSoup** para investigar, pesquisar e entender contexto — sem substituir APIs onde já funcionam bem.

---

## Por que usar

- **Contexto real:** Reclame Aqui, sites de concorrentes e landing pages trazem linguagem e dores reais (não só dados sintéticos).
- **Pesquisa de mercado:** Entender como players se posicionam, que termos usam, que reclamações aparecem por nicho.
- **Enriquecer dores/soluções:** Validar e complementar o dataset gerado (5000 linhas) com amostras reais raspadas.
- **Investigar leads:** Antes de abordar, ver o que a empresa faz, que problemas públicos existem (reclamações, notícias).

---

## Casos de uso na Graventum

| Objetivo | Fonte exemplo | O que extrair |
|----------|----------------|----------------|
| Dores reais por nicho | Reclame Aqui (páginas de empresa/setor) | Título, texto da reclamação, data, empresa |
| Posicionamento concorrente | Sites de consultorias/automação | Headlines, CTAs, casos de sucesso, glossário |
| Contexto de um lead | Site da empresa, página "Sobre" | Segmento, tamanho, foco (fiscal, operacional, etc.) |
| Tendências por setor | Notícias, releases | Palavras-chave, temas recorrentes |

---

## Scripts disponíveis

### 1. `tools/research_scraper.py` (base)
**Uso:** `python3 tools/research_scraper.py <URL> [--out arquivo.txt]`
- Script mínimo para baixar uma URL e extrair texto principal
- Remove menus, rodapés, scripts automaticamente
- Bom para testes rápidos e extração simples

### 2. `tools/reclameaqui_scraper.py` (especializado)
- **Foco:** Reclame Aqui - pesquisa de dores reais por nicho
- **Extrai:** Título, texto, data, empresa, nicho, gravidade, URL
- **Saída:** CSV/JSON em `outputs/datasets/scraped_reclameaqui_YYYY-MM-DD.csv`

**Duas formas de obter dados (o Reclame Aqui retorna 403 em muitos servidores):**

| Modo | Quando usar | Comando |
|------|-------------|---------|
| **--from-csv** | No servidor ou quando o site bloquear (403) | `python3 tools/reclameaqui_scraper.py --from-csv outputs/datasets/dados-para-analises.csv` |
| **--browser** | No seu PC (IP residencial) para dados ao vivo | `python3 tools/reclameaqui_scraper.py --niche contabilidade --max-pages 5 --browser` |

**Exemplos:**
```bash
# Coletar a partir do CSV existente (sempre funciona; gera scraped_reclameaqui_*.csv)
python3 tools/reclameaqui_scraper.py --from-csv outputs/datasets/dados-para-analises.csv

# Raspar ao vivo no Reclame Aqui (use no seu computador se --from-csv for só backup)
python3 tools/reclameaqui_scraper.py --niche contabilidade --max-pages 3 --browser

# Raspar de arquivo de URLs (com --browser se houver 403)
python3 tools/reclameaqui_scraper.py --urls urls.txt --output-format json --browser

python3 tools/reclameaqui_scraper.py --help
```

### 3. `tools/batch_scraper.py` (genérico em lote)
**Uso:** `python3 tools/batch_scraper.py --urls urls.txt --output dados.csv`
- **Foco:** Múltiplas URLs com extração configurável
- **Configuração:** JSON com seletores CSS por domínio
- **Campos comuns:** Título, description, H1, texto principal, contagem de palavras
- **Saída:** CSV/JSON em `outputs/research/`

**Exemplos:**
```bash
# Extração básica
python3 tools/batch_scraper.py --urls urls.txt --fields title,text,date --output pesquisa.csv

# Com configuração personalizada
python3 tools/batch_scraper.py --urls urls.txt --config selectors.json --output dados.json
```

---

## Como começar

### Dependência

```bash
pip install beautifulsoup4 requests
```

(O `requests` já é usado no workspace.)

### Estrutura de arquivos gerados

```
outputs/
├── datasets/                    # Dados raspados estruturados
│   ├── scraped_reclameaqui_2026-02-15.csv
│   ├── scraped_reclameaqui_2026-02-15.json
│   └── scraping_failed_2026-02-15.log
└── research/                   # Pesquisas diversas
    ├── batch_scraping_2026-02-15.csv
    └── site_analysis_2026-02-15.json
```

### Boas práticas

1. **Respeitar o site:** `robots.txt`, rate limit (ex.: 1 req/segundo), User-Agent identificável.
2. **Termos de uso:** Reclame Aqui e outros têm ToS; usar para pesquisa interna e não republicar em massa.
3. **Dados pessoais:** Não extrair ou armazenar dados pessoais além do necessário; preferir dados agregados ou anônimos.
4. **Fallback:** Se o site mudar o HTML, os seletores quebram; tratar erros e logar falhas.
5. **Rate limiting:** Sempre usar `--rate-limit` ou o padrão de 1 segundo entre requisições.

---

## Onde isso se encaixa no fluxo

- **Lead hunting:** APIs (Brave etc.) continuam como fonte principal; scraping como **complemento** para aprofundar contexto de um lead ou nicho.
- **Dores/soluções:** Manter o gerador sintético; usar scraping para **validar** e **enriquecer** com amostras reais (ex.: 100–500 reclamações por nicho).
- **Conteúdo e vendas:** Pesquisar linguagem e objeções reais para templates, pitches e artigos.
- **Pesquisa de concorrentes:** Analisar posicionamento, ofertas e casos de sucesso.

---

## Referência rápida BeautifulSoup

```python
from bs4 import BeautifulSoup
import requests

r = requests.get(url, headers={"User-Agent": "Graventum-Research/1.0"})
soup = BeautifulSoup(r.text, "html.parser")

# Por tag
soup.find_all("p")
soup.find("h1").get_text(strip=True)

# Por classe
soup.find_all(class_="reclamacao-texto")

# Por seletor CSS
soup.select("article .content")
```

---

## Configuração de seletores (exemplo JSON)

```json
{
  "selectors": {
    "*": {
      "title": "title",
      "main_text": "article",
      "date": "time"
    },
    "www.reclameaqui.com.br": {
      "title": ".sc-1pe7b5t-1",
      "text": ".sc-1pe7b5t-2",
      "date": "attr:datetime",
      "company": ".company-name"
    }
  }
}
```

---

## Busca por novos dados

Para que **novos dados** de reclamações entrem no sistema **sem duplicar** o que já está no Supabase:

1. **Gerar CSV novo**
   - **No seu PC (Reclame Aqui ao vivo):**  
     `python3 tools/reclameaqui_scraper.py --niche contabilidade --max-pages 5 --browser`  
     Gera algo como `scraped_reclameaqui_YYYY-MM-DD.csv`.
   - **No servidor (quando o site retorna 403):** use `--from-csv` ou outro CSV no formato scraped.  
   - **Outras fontes:** `batch_scraper.py` / `research_scraper.py` — se o CSV tiver coluna `url` (ou `url_fonte`) e colunas compatíveis, o mesmo fluxo vale.

2. **Copiar para o workspace (se o CSV veio do PC)**  
   Coloque o arquivo em `outputs/datasets/` (ou em outra pasta combinada no servidor).

3. **Rodar a carga incremental**  
   No servidor (ou onde estiver o `.env` com Supabase):
   ```bash
   python3 tools/carregar-novos-dados-supabase.py outputs/datasets/scraped_reclameaqui_2026-02-15.csv
   ```
   O script consulta as `url_fonte` já existentes no Supabase, filtra as linhas do CSV cuja URL ainda não está no banco e **insere apenas essas** na tabela `dados_para_analises`. Assim não há duplicatas.

**Prompt para o Helix (ou assistente):**  
*“Quando eu colocar um CSV novo de reclamações em `outputs/datasets/` (ex.: `scraped_reclameaqui_*.csv`), roda o script de carga incremental para enviar só os registros novos para a tabela `dados_para_analises` no Supabase.”*

---

## Limitações: 403 no Reclame Aqui (servidor/datacenter)

O Reclame Aqui retorna **403** em muitos IPs de datacenter, mesmo com Playwright. Para **coletar dados funcionando**:

1. **No servidor (ou quando der 403):** use **--from-csv** com o CSV que você já tem. O script gera `scraped_reclameaqui_YYYY-MM-DD.csv` no mesmo formato, pronto para análise:
   ```bash
   python3 tools/reclameaqui_scraper.py --from-csv outputs/datasets/dados-para-analises.csv
   ```
2. **No seu PC (IP residencial):** use **--browser** para raspar o site ao vivo:
   ```bash
   pip install playwright && playwright install chromium
   python3 tools/reclameaqui_scraper.py --niche contabilidade --max-pages 5 --browser
   ```
3. **Outros sites:** `batch_scraper.py` e `research_scraper.py` funcionam para sites que não bloqueiam (concorrentes, blogs).

---

*Documento criado para apoiar pesquisa e contexto; revisar periodicamente conforme novos casos de uso.*
