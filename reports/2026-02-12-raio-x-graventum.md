# Raio-X Completo da Graventum
**Data:** 2026-02-12 | **Autor:** Helix (COO Virtual)

---

## 1. INFRAESTRUTURA — Estado Atual

### Servidor Hetzner (46.225.79.76)

**Item** | **Status** | **Nota**
--- | --- | ---
OS | Ubuntu 24.04, kernel 6.8 | [OK] Atualizado
CPU | 4 vCPUs | [OK] Ociosa (load 0.00)
RAM | 8GB (2GB usado, 5.6GB disponível) | [OK] Sobra bastante
Disco | 150GB (9.3GB usado, 93% livre) | [OK] Bem folgado
Uptime | 10 dias | [OK] Estável
Updates automáticos | [OK] Ativo | Bom

**Veredicto:** Servidor super subutilizado. Aguenta 10x mais carga tranquilo.

### Serviços Rodando

**Serviço** | **Status** | **Porta** | **Observação**
--- | --- | --- | ---
**graventum-api** (PM2) | [OK] Online | 3000 (público!) | 6 rotas, 66MB RAM
**n8n** (Docker) | [OK] Online | 5678 (via nginx) | 17MB dados, SSL ok
**nginx** | [OK] Online | 80/443 | Proxy reverso, SSL Certbot
**Evolution API** | [ERRO] NÃO EXISTE | — | **Não está instalado**

### Domínios e SSL

**Domínio** | **Destino** | **SSL**
--- | --- | ---
automation.graventum.com | n8n (Docker) | [OK] Certbot
openclaw.graventum.com | OpenClaw Gateway | [OK] Certbot
graventum.com / www | Vercel (Next.js) | [OK] Vercel

---

## 2. PROBLEMAS CRÍTICOS ENCONTRADOS

### 🔴 CRÍTICO 1: Evolution API não existe
O BOOTSTRAP.md e TOOLS.md dizem que a Evolution API está rodando em Docker. **Ela não está.** Não há container, não há imagem, não há nada. Isso significa:
- **Toda a estratégia de automação WhatsApp está parada**
- Os 3 workflows da Automação Fantasma não funcionam sem ela
- Nenhum bot WhatsApp pode ser criado

**Impacto:** Bloqueia 100% do plano de primeiro cliente.

### 🔴 CRÍTICO 2: API exposta sem autenticação na porta 3000
A porta 3000 está aberta para o mundo inteiro (`0.0.0.0:3000`). Qualquer pessoa pode:
- Chamar `/gerar-campanha` e gastar seus créditos Anthropic
- Chamar `/microsoft/send-email` e enviar emails como contato@graventum.com
- Chamar `/agent/classify-document` e gastar créditos Claude Vision

**Isso é uma vulnerabilidade séria.** Alguém pode encontrar essa porta com um scan e abusar da API.

### 🔴 CRÍTICO 3: Fail2ban inativo
Fail2ban está instalado mas **inativo**. SSH está exposto na porta 22 padrão. Sem proteção contra brute force.

### 🟡 IMPORTANTE 4: Sem backups
Não existe diretório de backups. Se o servidor morrer, perde-se:
- Todos os workflows n8n (17MB de dados)
- Config da API
- Dados do OpenClaw
- Histórico de logs

### 🟡 IMPORTANTE 5: API não tem Git
O código do graventum-api no servidor não está versionado com Git. Qualquer mudança pode ser perdida. O repo existe no GitHub mas não está sincronizado.

### 🟡 IMPORTANTE 6: Porta 5678 (n8n) aberta no firewall
O n8n escuta em 127.0.0.1:5678 (correto — só local), mas o firewall tem regra `5678/tcp ALLOW Anywhere`. Isso não causa exposição direta neste caso (nginx faz proxy), mas é uma regra desnecessária que pode virar vetor se a config mudar.

---

## 3. SITE (graventum.com) — Análise

### Técnico
| Item | Status | Nota |
|------|--------|------|
| Hospedagem | Vercel | [OK] |
| Framework | Next.js 14 | [OK] |
| TTFB | 310ms | [OK] Bom |
| SSL | [OK] | Via Vercel |
| i18n | PT-BR, EN, ES | [OK] |
| Sitemap | [OK] Existe | Apenas 3 URLs (home em 3 idiomas) |
| Cache | [ERRO] `no-cache, no-store` | 🟡 Performance poderia melhorar |

### Conteúdo — Problemas Sérios

**1. Redirecionamento errado:**
`graventum.com` redireciona para `/en` (inglês). Site de empresa brasileira que vende para PMEs brasileiras abrindo em inglês. **O público-alvo não fala inglês.**

**2. Métricas zeradas na home:**
```
0+ Horas economizadas
0+ Empresas atendidas
0% Satisfação do cliente
```
Isso **destrói credibilidade** instantaneamente. Zero horas, zero empresas, zero satisfação. Melhor remover ou colocar números projetados/internos.

**3. Cases fictícios:**
O site mostra 3 depoimentos (escritório contábil, imobiliária, clínica) que são **inventados**. Nenhum tem nome de pessoa, nome de empresa, foto, ou link. Se um prospect minimamente atento perceber, perde toda a confiança.

**4. Sem blog / sem conteúdo SEO:**
Sitemap tem apenas 3 URLs (home em 3 idiomas). Zero blog posts, zero landing pages, zero conteúdo orgânico. **SEO é inexistente.**

**5. Sem pricing:**
O modelo de engajamento é descrito (Diagnóstico → Implementação → Retainer) mas sem nenhuma indicação de preço. Para PME brasileira, preço é decisivo. A falta de referência afasta quem tem orçamento e atrai quem só quer saber quanto custa.

**6. Formulário de contato é o único CTA:**
Não há: WhatsApp button, calendário de agendamento, lead magnet, quiz, nada. Só "preencha o formulário e esperamos 24h". **Barreira alta para primeiro contato.**

---

## 4. API (graventum-api) — Análise

### Estrutura
```
src/
├── index.js           (65 linhas — entry point)
├── routes/
│   ├── campaign.js    (geração de campanha com Claude)
│   ├── contato.js     (formulário do site)
│   ├── microsoft.js   (envio de email Exchange)
│   ├── health.js      (health check)
│   └── classify-document.js  ← NOVO (deployado hoje)
├── services/
│   ├── claude.js      (wrapper Anthropic)
│   └── microsoft.js   (wrapper Graph API)
└── prompts/
    └── campaign.js    (prompts de campanha)
```

**Total:** ~330 linhas de código. API enxuta mas funcional.

**Dependências:** express, dotenv, @anthropic-ai/sdk, exceljs

**Problemas:**
- Sem rate limiting (qualquer um pode fazer milhares de requests)
- Sem autenticação (API key, JWT, nada)
- Sem logging estruturado (só console.log básico)
- Sem error handling centralizado
- Sem testes
- CORS limitado a graventum.com + localhost (ok, mas a porta 3000 pública anula isso)

---

## 5. n8n (Automação)

- **Versão:** Latest (Docker)
- **Dados:** 17MB
- **Acesso:** automation.graventum.com (SSL)
- **Status:** Online, com workflows ativos
- **Observação:** Logs do nginx mostram atividade recente (Robson acessou hoje)
- **Nota:** Encontrei referência a workflow com Microsoft Teams (`n8n-nodes-base.microsoftTeams`) nos logs — alguma integração Teams ativa

Não consigo listar workflows sem API key do n8n.

---

## 6. PANORAMA FINANCEIRO

| Item | Custo/mês estimado |
|------|-------------------|
| Hetzner Cloud (CX31) | ~€15 (~R$ 95) |
| Vercel (free tier) | R$ 0 |
| Domínio graventum.com | ~R$ 10/mês amortizado |
| Microsoft E5 (se pago) | ~R$ 100/mês |
| Anthropic API (uso atual) | ~R$ 50-100/mês (estimativa) |
| Supabase (free tier?) | R$ 0 - R$ 125 |
| **Total estimado** | **~R$ 250-430/mês** |

**Receita:** R$ 0

**Burn rate:** Baixo, mas queimando há meses sem retorno.

---

## 7. DIAGNÓSTICO GERAL

### O que está BOM
- [OK] Stack técnica sólida e moderna (n8n + Supabase + Claude + Next.js)
- [OK] Servidor estável e com muita capacidade ociosa
- [OK] API funcional com integrações Microsoft e Anthropic
- [OK] Site profissional com i18n
- [OK] SSL em tudo, firewall ativo, updates automáticos
- [OK] Burn rate baixo — pode operar por muito tempo sem receita
- [OK] Posicionamento claro no site (diagnóstico → implementação → retainer)

### O que está RUIM
- [ERRO] **Zero clientes, zero receita** — empresa existe só na infraestrutura
- [ERRO] **Evolution API não instalada** — peça central da proposta de valor (WhatsApp) não existe
- [ERRO] **API exposta sem auth** — vulnerabilidade ativa que pode gerar prejuízo
- [ERRO] **Site em inglês por padrão** — repele o público-alvo
- [ERRO] **Cases fictícios** — risco de credibilidade
- [ERRO] **Zero conteúdo SEO** — invisível no Google
- [ERRO] **Sem backups** — um rm errado ou falha de disco e perde tudo
- [ERRO] **Sem Git no servidor** — código da API pode ser perdido

### A verdade nua
A Graventum tem **infraestrutura de empresa pronta** mas **resultado de projeto pessoal**. A stack é boa, o site é bonito, a API funciona — mas nenhum cliente nunca usou nada disso. O gap não é técnico, é de **go-to-market**.

---

## 8. PLANO DE AÇÃO — O que fazer AGORA

### Prioridade 1: DESBLOQUEIOS (esta semana)

####  1A: Instalar Evolution API (2-3h)
Sem isso, toda a proposta de WhatsApp é discurso vazio.
```bash
# Helix pode fazer isso se Robson autorizar
docker pull atendai/evolution-api
# + configuração docker-compose + nginx proxy
```

####  1B: Proteger a API (30min)
Adicionar API key simples no middleware:
```javascript
// Checar header x-api-key em todas as rotas
```
**Helix pode fazer agora.**

####  1C: Ativar Fail2ban (5min)
```bash
systemctl enable fail2ban && systemctl start fail2ban
```
**Helix pode fazer agora.**

####  1D: Setup de backup automático (30min)
Cron backup diário do n8n-data + API code para Google Drive ou local.
**Helix pode fazer agora.**

### Prioridade 2: SITE (esta semana)

#### 🌐 2A: Corrigir redirect para PT-BR
O default deve ser português, não inglês.

#### 🌐 2B: Remover ou ajustar métricas zeradas
Ou tirar, ou colocar "50+ horas economizadas em projetos internos".

#### 🌐 2C: Adicionar WhatsApp button
Botão flutuante de WhatsApp no canto. CTA de menor fricção para PME brasileira.

#### 🌐 2D: Remover cases fictícios
Substituir por "Em breve: cases reais de implementação" ou simplesmente tirar a seção.

### Prioridade 3: GO-TO-MARKET (semanas 2-4)

####  3A: Completar Automação Fantasma
Com Evolution API instalada, os workflows funcionam e temos algo demonstrável.

####  3B: 1 post/semana no LinkedIn
Sobre automação contábil. Helix pode redigir todos.

####  3C: Iniciar blog no site
1 artigo/semana sobre automação para PMEs. SEO de cauda longa. Helix pode escrever todos.

####  3D: Calculadora de Tempo Perdido
Ideia 2 do relatório anterior. Gera leads sem outreach.

---

## 9. PROATIVO — Coisas que ninguém pediu mas que eu faria

### 💡 Criar um "produto" empacotado
Em vez de vender "consultoria de automação" (abstrato), vender:
- **"Contábil AI"** — Pacote pronto de automação WhatsApp para contabilidade. R$ 997/mês.
- **"Imob AI"** — Pacote pronto para imobiliárias. R$ 1.297/mês.

Nome, preço, escopo definido. PME brasileira compra produto, não projeto.

### 💡 Montar um mini-SaaS no próprio n8n
Oferecer acesso a workflows pré-construídos como serviço. O cliente não precisa saber que é n8n por baixo. Ele vê "Contábil AI powered by Graventum".

### 💡 Gravar 5 vídeos Loom de demonstração
Cada um mostrando uma automação diferente. Publicar no site e LinkedIn. Sem vender — só mostrar. Quem quiser, vem.

### 💡 Monetizar a Stack enquanto busca clientes
A Graventum tem uma API funcional com Claude. Poderia:
- Vender acesso à rota `/gerar-campanha` como micro-SaaS
- Criar uma landing page "Gere sua campanha de marketing com IA — R$ 29/uso"
- **Receita imediata enquanto constrói o negócio principal**

### 💡 Publicar templates n8n open-source
Criar 3-5 templates de workflow para o marketplace n8n Community. Cada template tem "powered by Graventum" com link. Tráfego gratuito de quem procura automação.

### 💡 Construir o "Graventum Dashboard"
Uma página em graventum.com que mostra métricas internas das automações (pra uso próprio e futuro cliente). Demonstra competência e serve como produto.

---

## 10. TIMELINE REALISTA — Primeiro Cliente

| Semana | Ação | Responsável |
|--------|------|-------------|
| **1** | Instalar Evolution API + Proteger API + Backups + Fix site | Helix (95%) + Robson (aprovar) |
| **1** | Fix redirect PT-BR, remover zeros, adicionar WhatsApp button | Helix ou Robson (código Next.js) |
| **2** | Workflows Automação Fantasma funcionando end-to-end | Helix |
| **2** | Gravar 1 vídeo demo + 2 posts LinkedIn | Robson grava / Helix escreve |
| **3** | Enviar demo para 3 escritórios contábeis | Robson |
| **3-4** | Calculadora de Tempo Perdido live no site | Helix |
| **4-6** | 1 blog post/semana (SEO) | Helix |
| **4-8** | **Primeiro cliente pagante** |  |

---

## RESUMO EXECUTIVO

A Graventum é uma **empresa de infraestrutura sem clientes**. A stack é sólida, o servidor é estável, o custo é baixo. Mas existem 3 bloqueios imediatos:

1. **Evolution API não existe** — sem ela, não há WhatsApp, não há proposta de valor tangível
2. **Site comunica errado** — inglês por padrão, zeros, cases fictícios
3. **API insegura** — vulnerabilidade real que precisa ser corrigida hoje

A boa notícia: tudo isso se resolve em 1 semana de trabalho focado. E o burn rate baixo dá tempo de executar sem pressão.

**O primeiro cliente está a ~4-8 semanas de distância, se executarmos o plano.**

---

*Relatório gerado por Helix — COO Virtual da Graventum*
*Baseado em auditoria real do servidor, site, API e infraestrutura*
