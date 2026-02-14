# Guia Prático: Como Usar as APIs do Meta (Facebook/Instagram/WhatsApp) em 2026

## 🚨 O Problema (Por que é tão chato)

A Meta mudou tudo nos últimos anos e o processo é **deliberadamente complexo** para:
1. **Prevenir spam/abuso** - filtragem rigorosa
2. **Garantir compliance** - LGPD, GDPR, etc.
3. **Monetizar acesso** - empresas grandes pagam, pequenas sofrem
4. **Centralizar controle** - tudo passa pelo Business Manager

**Resultado:** Processo confuso, documentação dispersa, aprovações demoradas.

## 📋 Visão Geral do Ecossistema

### 1. **Meta Business Manager (BM)**
- **O que é:** Painel central que gerencia TUDO
- **Para que serve:** Verificação de negócio, gestão de ativos, controle de acesso
- **Obrigatório para:** WhatsApp Business API, Instagram Professional, Facebook Pages

### 2. **APIs Disponíveis:**
- **WhatsApp Business API:** Mensagens automatizadas (Cloud API vs On-Premise)
- **Instagram Graph API:** Postagem automática, analytics, gestão de comentários
- **Facebook Graph API:** Gestão de páginas, ads, insights
- **Messenger API:** Chatbots no Messenger

### 3. **Níveis de Acesso:**
```
Nível 1: Basic Access (gratuito) → Leitura básica
Nível 2: Standard Access (verificação) → Postagem limitada  
Nível 3: Advanced Access (business verified) → Tudo liberado
```

## 🛠️ Passo a Passo Prático

### **FASE 1: Configurar Meta Business Manager**

#### 1.1 Criar Business Manager
1. Acesse: `business.facebook.com`
2. Clique em "Criar Conta"
3. Preencha:
   - Nome do negócio: **"Graventum"** (exatamente como CNPJ)
   - Seu nome: Robson Caldeira
   - Email de negócio: `contato@graventum.com`

#### 1.2 Adicionar Ativos (CRÍTICO)
Sem ativos, o botão de verificação fica **cinza**:
1. **Facebook Page:** Criar página "Graventum" (mesmo nome do BM)
2. **Instagram Account:** Conectar @graventum (se existir)
3. **WhatsApp:** Adicionar número (11) XXXXX-XXXX (não usar o pessoal ainda)

#### 1.3 Configurar Administradores
- **Admin principal:** Você (Robson)
- **Admin backup:** Outro email (ou criar email alternativo)
- **Desenvolvedor:** Sua conta de desenvolvedor

### **FASE 2: Verificação do Negócio (A Parte Mais Chata)**

#### 2.1 Documentos Necessários
**Para MEI/Individual:**
- [ ] RG frente/verso
- [ ] CPF  
- [ ] Comprovante de residência (últimos 3 meses)
- [ ] Selfie com RG (às vezes pedem)

**Para CNPJ:**
- [ ] Contrato social
- [ ] CNPJ (cartão ou extrato)
- [ ] Documento do responsável (RG/CPF)
- [ ] Comprovante de endereço da empresa

#### 2.2 Processo de Submissão
1. No BM: `Configurações → Segurança → Verificação do Negócio`
2. Se o botão estiver **cinza**: Adicione mais ativos (página, instagram)
3. Escolha método: **Documentos oficiais** (mais rápido que domínio)
4. Faça upload **CLARO E LEGÍVEL** (fotos boas, sem reflexo)
5. **Dica crucial:** Nome nos documentos DEVE bater exatamente com nome no BM

#### 2.3 Tempo de Aprovação
- **Rápido:** 2-3 dias (se documentos perfeitos)
- **Normal:** 1-2 semanas  
- **Problemas:** 3-4 semanas (rejeições, pedem mais docs)

#### 2.4 Problemas Comuns e Soluções

| Problema | Solução |
|----------|---------|
| Botão "Iniciar verificação" cinza | Adicione Facebook Page + Instagram |
| Documentos rejeitados | Use scanner, não foto; nome exato igual |
| Selfie não aceita | Fundo branco, boa iluminação, rosto visível |
| "Ineligible for verification" | Delete BM antigo, crie novo com docs certos |
| Pedem mais documentos | Envie tudo de uma vez (RG, CPF, comprovante) |

### **FASE 3: Criar App de Desenvolvedor**

#### 3.1 Acessar Developers Facebook
1. `developers.facebook.com`
2. Login com sua conta pessoal (não do BM ainda)
3. "My Apps" → "Create App"

#### 3.2 Tipo de App
- **Para WhatsApp:** Escolha "Business"
- **Para Instagram/Facebook:** Escolha "Other" → "Business"

#### 3.3 Configurações Básicas
```
App Name: Graventum Automation
Contact Email: contato@graventum.com
Business Manager: Selecionar "Graventum" (após verificação)
```

#### 3.4 Adicionar Produtos
1. **WhatsApp** → Cloud API (recomendado para começar)
2. **Instagram** → Graph API
3. **Facebook** → Graph API
4. **Messenger** (opcional)

### **FASE 4: Obter Access Tokens**

#### 4.1 Tokens para WhatsApp
1. No app: `WhatsApp → API Setup`
2. Gera token automaticamente (válido por algumas horas)
3. Para token permanente:
   ```bash
   # Exchange token
   curl -X GET "https://graph.facebook.com/v18.0/oauth/access_token?  
     grant_type=fb_exchange_token&
     client_id={app-id}&
     client_secret={app-secret}&
     fb_exchange_token={short-lived-token}"
   ```

#### 4.2 Tokens para Instagram
1. Conectar Instagram Professional Account ao BM
2. No app: `Instagram Basic Display` → `Add or Remove Instagram Testers`
3. Adicionar sua conta do Instagram
4. Permissions necessárias:
   - `instagram_basic`
   - `instagram_content_publish` 
   - `pages_read_engagement`

#### 4.3 Tokens para Facebook Pages
1. Conectar Facebook Page ao BM
2. No app: `Facebook Login` → `Settings`
3. Adicionar `pages_manage_posts`, `pages_read_engagement`
4. Gerar Page Access Token:
   ```bash
   curl -i -X GET "https://graph.facebook.com/{page-id}?
     fields=access_token&
     access_token={user-access-token}"
   ```

### **FASE 5: Configurar Webhooks (Para Receber Mensagens)**

#### 5.1 WhatsApp Webhooks
```javascript
// Endpoint que você precisa expor
POST https://sua-api.com/webhook/whatsapp
Headers: 
  X-Hub-Signature-256: sha256={signature}
Body:
{
  "object": "whatsapp_business_account",
  "entry": [...]
}

// Verificação inicial (Meta envia GET)
GET https://sua-api.com/webhook/whatsapp?
  hub.mode=subscribe&
  hub.challenge=123456&
  hub.verify_token=SEU_TOKEN
```

#### 5.2 Configurar no Dashboard
1. App → WhatsApp → Configuration
2. Webhook URL: `https://sua-api.com/webhook/whatsapp`
3. Verify Token: `graventum-2026-secreto`
4. Subscription Fields: `messages, message_template_status_update`

### **FASE 6: Primeiros Testes**

#### 6.1 Testar WhatsApp
```bash
# Enviar mensagem de teste
curl -X POST "https://graph.facebook.com/v18.0/{phone-number-id}/messages" \
  -H "Authorization: Bearer {access-token}" \
  -H "Content-Type: application/json" \
  -d '{
    "messaging_product": "whatsapp",
    "to": "5511999999999",
    "type": "text",
    "text": {"body": "Teste da Graventum!"}
  }'
```

#### 6.2 Testar Instagram Post
```bash
# Criar container de mídia
curl -X POST "https://graph.facebook.com/v18.0/{ig-user-id}/media" \
  -H "Authorization: Bearer {access-token}" \
  -F "image_url=https://exemplo.com/imagem.jpg" \
  -F "caption=Primeiro post automático da Graventum!"

# Publicar
curl -X POST "https://graph.facebook.com/v18.0/{ig-user-id}/media_publish" \
  -H "Authorization: Bearer {access-token}" \
  -F "creation_id={media-id}"
```

## 🔧 Integração com n8n (Recomendado)

### Por que usar n8n em vez de código direto:
1. **Gestão de credenciais segura** (nunca no código)
2. **Retry automático** para falhas da API
3. **Logging completo** de todas as interações
4. **Fácil manutenção** (workflows visuais)

### Configuração no n8n:

#### 1. Credenciais do WhatsApp:
```
Tipo: WhatsApp
Access Token: {seu-token-aqui}
Business Account ID: {id-da-conta}
```

#### 2. Credenciais do Instagram:
```
Tipo: Instagram Graph API
Access Token: {instagram-token}
Client ID: {app-id}
Client Secret: {app-secret}
```

#### 3. Workflow Exemplo (Post Automático):
```
Trigger: Schedule (9:00 AM daily)
→ HTTP Request: Busca conteúdo do OpenClaw
→ Instagram Node: Upload imagem
→ Delay: 2 minutos (processamento)
→ Instagram Node: Publica com legenda
→ Telegram Node: Notifica sucesso/erro
```

## 🚫 Erros Mais Comuns e Como Evitar

### 1. **Rate Limits**
```
Erro: "Rate limit exceeded"
Solução: 
- WhatsApp: 80 mensagens/segundo por número
- Instagram: 200 requests/hora por token
- Implementar queue + delays no n8n
```

### 2. **Token Expired**
```
Erro: "Invalid OAuth access token"
Solução:
- Tokens duram 1-60 dias (depende do tipo)
- Implementar refresh automático no n8n
- Monitorar com cron job diário
```

### 3. **Permission Errors**
```
Erro: "Insufficient permission"
Solução:
- Verificar scopes no Token Debugger
- Facebook: pages_manage_posts, pages_read_engagement
- Instagram: instagram_content_publish, instagram_basic
- WhatsApp: whatsapp_business_messaging, whatsapp_business_management
```

### 4. **Business Verification Required**
```
Erro: "Business verification required"
Solução:
- Não tem jeito: tem que verificar o BM
- Use documentos CORRETOS na primeira tentativa
- Se rejeitar, espere 7 dias antes de nova tentativa
```

## 📊 Monitoramento e Manutenção

### Checklist Diário:
- [ ] Tokens válidos (usar Token Debugger)
- [ ] Rate limits (monitorar 429 errors)
- [ ] Webhooks ativos (testar com mensagem)
- [ ] Templates aprovados (WhatsApp)

### Ferramentas Úteis:
1. **Token Debugger:** `developers.facebook.com/tools/debug/`
2. **Graph API Explorer:** `developers.facebook.com/tools/explorer/`
3. **Webhook Tester:** `webhook.site` (para testes)
4. **n8n Logs:** Monitorar execuções falhas

## 🎯 Estratégia para Graventum

### Prioridade 1: WhatsApp Business API
**Por que:** Já temos Evolution API, mas a oficial é mais confiável
**Passos:**
1. Verificar BM com documentos Graventum
2. Criar app para WhatsApp
3. Migrar gradualmente do Evolution
4. Manter ambos durante transição

### Prioridade 2: Instagram Automation
**Por que:** Postagem automática de conteúdo
**Passos:**
1. Conectar @graventum ao BM verificado
2. Criar app com permissões de postagem
3. Implementar via n8n + OpenClaw
4. Testar com 1 post/dia

### Prioridade 3: Facebook Pages
**Por que:** Cross-posting LinkedIn → Facebook
**Passos:**
1. Conectar página Facebook ao BM
2. Configurar auto-posting
3. Analytics automáticos

## ⏱️ Timeline Realista

### Semana 1:
- [ ] Criar/verificar Meta Business Manager
- [ ] Reunir documentos corretamente
- [ ] Submeter verificação

### Semana 2-3:
- [ ] Aguardar aprovação (1-2 semanas)
- [ ] Criar app de desenvolvedor
- [ ] Configurar tokens básicos

### Semana 4:
- [ ] Testes iniciais WhatsApp
- [ ] Configurar n8n integration
- [ ] Primeiro post automático Instagram

### Semana 5-6:
- [ ] Migração gradual do Evolution API
- [ ] Sistema completo de postagem
- [ ] Monitoramento e otimização

## 💡 Dicas Finais

### 1. **Documentação Oficial é Sua Amiga**
- WhatsApp: `developers.facebook.com/docs/whatsapp/`
- Instagram: `developers.facebook.com/docs/instagram/`
- Graph API: `developers.facebook.com/docs/graph-api/`

### 2. **Use Contas de Teste Primeiro**
- Não use sua conta pessoal do WhatsApp
- Crie Instagram/Facebook de teste
- Teste TUDO no sandbox antes de produção

### 3. **Tenha Paciência com a Meta**
- Suporte é lento (2-5 dias resposta)
- Aprovações podem levar semanas
- Bugs são comuns, workarounds necessários

### 4. **Backup é Fundamental**
- Mantenha Evolution API como fallback
- Tenha estratégia manual como plano B
- Monitor 24/7 com alertas

### 5. **Compliance é Inegociável**
- Termos de uso do WhatsApp são rígidos
- Double opt-in para marketing
- Opção de unsubscribe obrigatória
- Logs de consentimento por 2 anos

## 🆘 Suporte e Recursos

### Comunidades Úteis:
- **Reddit:** r/facebook, r/whatsapp, r/webdev
- **Stack Overflow:** Tags: facebook-graph-api, whatsapp-business
- **GitHub:** Meta SDKs oficial issues

### Ferramentas Recomendadas:
- **Postman:** Para testar APIs
- **ngrok:** Expor webhooks localmente
- **Charles Proxy:** Debug de requests
- **Sentry:** Error tracking

### Canais de Suporte Meta:
1. **Developer Support:** Via dashboard do app
2. **Business Support:** No BM (após verificação)
3. **Community Forums:** `developers.facebook.com/community`

---

**Status deste guia:** Atualizado para 2026  
**Baseado em:** Pesquisa atual + experiência prática  
**Próxima ação:** Iniciar verificação do Meta Business Manager  
**Tempo estimado total:** 3-6 semanas para sistema completo

> **Nota:** Este processo é intencionalmente complexo. A Meta quer garantir que apenas negócios sérios tenham acesso automatizado. A paciência e atenção aos detalhes são essenciais.