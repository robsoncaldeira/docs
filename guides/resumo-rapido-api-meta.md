# Resumo Rápido: API do Meta (Facebook/WhatsApp/Instagram)

## 🎯 **O QUE VOCÊ PRECISA FAZER AGORA:**

### **PASSO 1 - Meta Business Manager (OBRIGATÓRIO)**
1. Acesse: `business.facebook.com`
2. Crie conta com nome **"Graventum"** (igual ao CNPJ)
3. Email: `contato@graventum.com`

### **PASSO 2 - Adicionar Ativos (SEM ISSO NÃO FUNCIONA)**
- Crie página Facebook "Graventum"
- Conecte Instagram @graventum (se tiver)
- Adicione um número de WhatsApp (não o pessoal ainda)

### **PASSO 3 - Verificação (A PARTE CHATA)**
**Documentos necessários (MEI):**
- RG frente/verso
- CPF
- Comprovante de residência (últimos 3 meses)
- Selfie com RG

**Processo:**
1. BM → Configurações → Segurança → Verificação do Negócio
2. Escolha "Documentos oficiais"
3. Faça upload CLARO (use scanner, não foto ruim)
4. **CRÍTICO:** Nome nos documentos DEVE ser IGUAL ao nome no BM

**Tempo:** 2 dias a 2 semanas (depende dos documentos)

## 🔑 **DEPOIS DA VERIFICAÇÃO:**

### **WhatsApp Business API:**
1. `developers.facebook.com` → Criar App "Business"
2. Adicionar produto "WhatsApp"
3. BM verificado → App → WhatsApp → API Setup
4. Copiar Access Token + Business Account ID

### **Instagram Automation:**
1. Mesmo app → Adicionar "Instagram Graph API"
2. Permissions: `instagram_basic`, `instagram_content_publish`
3. Conectar conta @graventum ao BM
4. Gerar token de acesso

## ⚡ **INTEGRAÇÃO COM n8n (RECOMENDADO):**

### Credenciais no n8n:
```
WhatsApp:
- Access Token: {token-do-passo-acima}
- Business Account ID: {id-da-conta}

Instagram:
- Access Token: {instagram-token}
- Client ID/Secret: do app
```

### Workflow exemplo (Post automático):
```
Schedule trigger → HTTP Request (OpenClaw) → Instagram Post → Telegram alert
```

## 🚨 **PROBLEMAS COMUNS:**

1. **Botão de verificação CINZA:**
   → Adicione Facebook Page + Instagram ao BM

2. **Documentos rejeitados:**
   → Use scanner, nome exato igual, selfie boa qualidade

3. **"Ineligible for verification":**
   → Delete BM antigo, crie novo com docs certos

4. **Token expirado:**
   → Tokens duram 1-60 dias, implemente refresh no n8n

## 📅 **TIMELINE REALISTA:**

**Semana 1:** Criar BM + enviar verificação  
**Semanas 2-3:** Aguardar aprovação (1-2 semanas)  
**Semana 4:** Criar app + primeiros testes  
**Semanas 5-6:** Integrar com n8n + migração gradual

## 💡 **DICA CRUCIAL:**

**NÃO use sua conta pessoal do WhatsApp para testes.**  
Crie número separado ou use sandbox primeiro.

---

**Próxima ação imediata:**  
1. Criar Meta Business Manager (15 minutos)  
2. Reunir documentos (RG, CPF, comprovante)  
3. Iniciar verificação

**Tempo total estimado:** 3-6 semanas para sistema completo funcionando.

**Alternativa enquanto isso:** Continuar com Evolution API (que já temos) e focar em outras prioridades primeiro.