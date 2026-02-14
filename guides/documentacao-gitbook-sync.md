# Documentação: Sincronização OpenClaw → GitBook

## 📋 Visão Geral

Solução completa para sincronizar conteúdo do workspace OpenClaw com GitBook usando n8n como intermediário, **sem necessidade de compartilhar tokens**.

### 🎯 Objetivo
1. Criar endpoint na Graventum API para receber markdown
2. Criar workflow n8n que consome endpoint e cria conteúdo no GitBook
3. Implementar sincronização inicial do workspace
4. Criar space "Playbook Operacional" com estrutura básica

### 🏗️ Arquitetura
```
OpenClaw Workspace → Graventum API → n8n → GitBook
```

## 1. Endpoint API Graventum

### 📍 Rota: POST `/gitbook/sync`
**URL**: `http://46.225.79.76:3000/gitbook/sync`

### 🔐 Autenticação
Header: `x-api-key: graventum-gitbook-sync-2025`

### 📦 Payload
```json
{
  "spaceId": "playbook-operacional",
  "title": "Título da Página",
  "content": "# Conteúdo Markdown\n\nTexto formatado...",
  "slug": "slug-opcional",
  "parentPageId": "id-da-pagina-pai",
  "visibility": "all",
  "metadata": {
    "source": "openclaw-workspace",
    "filePath": "caminho/do/arquivo.md",
    "category": "core"
  }
}
```

### 🚀 Implementação
1. **Arquivo**: `gitbook-endpoint.js`
2. **Localização**: `/routes/gitbook/` na API Graventum
3. **Registro**: Adicionar em `app.js`:
   ```javascript
   app.use('/gitbook', require('./routes/gitbook/gitbook-endpoint'));
   ```

## 2. Workflow n8n

### 🔄 Workflow Principal: Sincronização
**Nome**: "GitBook Sync - OpenClaw Workspace"
**Arquivo**: `n8n-gitbook-workflow.json`

#### Nodos:
1. **Webhook** - Recebe payload da API
2. **Validate Payload** - Valida dados recebidos
3. **GitBook OAuth** - Cria/atualiza página no GitBook
4. **Log Success** - Registra sucesso
5. **Success Response** - Retorna resposta

#### Credenciais:
- Usa OAuth já configurado no n8n (Robson autenticado)
- Organização: Graventum (ID: LPzDENVU2jEpQQeuKgSr)

### 🏗️ Workflow Secundário: Criar Space
**Nome**: "GitBook - Criar Space Playbook Operacional"
**Arquivo**: `n8n-create-space-workflow.json`

#### Funcionalidades:
1. Cria space "Playbook Operacional"
2. Cria página Home
3. Prepara estrutura de páginas
4. Envia notificação no Telegram

## 3. Sincronização Inicial

### 📁 Script: `sync-initial-workspace.js`
```bash
node sync-initial-workspace.js
```

#### Arquivos sincronizados:
1. `AGENTS.md` → Documentação do workspace
2. `BOOTSTRAP.md` → Contexto de negócio
3. `SOUL.md` → Identidade do assistente
4. `TOOLS.md` → Integrações e infraestrutura
5. `USER.md` → Perfil do Robson
6. `HEARTBEAT.md` → Sistema de verificações

### 🗂️ Estrutura no GitBook:
```
Playbook Operacional/
├── 🏠 Home
├── 📚 Documentação Core
│   ├── AGENTS - Graventum Workspace
│   ├── BOOTSTRAP - Contexto de Negócio
│   └── SOUL - Identidade do Assistente
├── 🛠️ Infraestrutura
│   ├── TOOLS - Integrações e Infraestrutura
│   └── USER - Perfil do Robson
├── 🧠 Sistema de Memória
│   └── HEARTBEAT - Sistema de Verificações
└── 📁 Outputs e Tools
```

## 4. Configuração Passo a Passo

### 🔧 Passo 1: Configurar Endpoint na API
```bash
# No servidor 46.225.79.76
cd /root/graventum-api
mkdir -p routes/gitbook
cp gitbook-endpoint.js routes/gitbook/
# Editar app.js para incluir rota
# Adicionar ao .env:
# GITBOOK_SYNC_API_KEY=graventum-gitbook-sync-2025
pm2 restart graventum-api
```

### 🔧 Passo 2: Importar Workflows no n8n
1. Acessar `automation.graventum.com`
2. Criar novo workflow
3. Importar `n8n-gitbook-workflow.json`
4. Importar `n8n-create-space-workflow.json`
5. Ativar workflows

### 🔧 Passo 3: Criar Space no GitBook
1. Executar workflow "GitBook - Criar Space Playbook Operacional"
2. Verificar notificação no Telegram
3. Confirmar space criado em `https://app.gitbook.com`

### 🔧 Passo 4: Sincronizar Workspace
```bash
# No workspace OpenClaw
node sync-initial-workspace.js
```

### 🔧 Passo 5: Testar Endpoint
```bash
curl -X POST http://46.225.79.76:3000/gitbook/sync \
  -H "x-api-key: graventum-gitbook-sync-2025" \
  -H "Content-Type: application/json" \
  -d '{
    "spaceId": "playbook-operacional",
    "title": "Teste de Sincronização",
    "content": "# Teste\n\nConteúdo de teste."
  }'
```

## 5. Automação e Manutenção

### 🔄 Sincronização Automática
**Opção 1: Webhook no OpenClaw**
- Configurar OpenClaw para enviar webhook quando arquivos mudam
- Endpoint: `http://46.225.79.76:3000/gitbook/sync`

**Opção 2: Cron Job**
```bash
# Executar diariamente
0 2 * * * cd /root/.openclaw/workspace && node sync-initial-workspace.js
```

**Opção 3: Monitoramento de Arquivos**
- Usar `inotifywait` para monitorar mudanças
- Disparar sincronização automática

### 📊 Monitoramento
1. **Logs**: `gitbook-sync-log.json`
2. **Notificações**: Telegram para sucesso/erro
3. **Health Check**: `GET /gitbook/health`

### 🔧 Manutenção
1. **Atualizar API Key**: Alterar no `.env` e scripts
2. **Adicionar Arquivos**: Atualizar `FILES_TO_SYNC` no script
3. **Modificar Estrutura**: Atualizar workflows n8n
4. **Backup**: Exportar workflows periodicamente

## 6. Segurança

### 🔐 Medidas Implementadas
1. **API Key obrigatória** para todos os endpoints
2. **Validação de payload** no endpoint e n8n
3. **OAuth do GitBook** via n8n (sem token sharing)
4. **Logging** de todas as operações
5. **Respostas padronizadas** com status claro

### ⚠️ Considerações
- API key armazenada em `.env` e scripts
- Webhook n8n público (proteger com autenticação adicional se necessário)
- Rate limiting recomendado para produção

## 7. Solução de Problemas

### ❌ Erro: API Key inválida
```json
{"success":false,"error":"API key inválida"}
```
**Solução**: Verificar header `x-api-key` ou variável de ambiente.

### ❌ Erro: Payload inválido
```json
{"success":false,"error":"Payload inválido","message":"Campo obrigatório ausente: title"}
```
**Solução**: Verificar campos obrigatórios no payload.

### ❌ Erro: GitBook OAuth falhou
**Solução**:
1. Verificar credenciais OAuth no n8n
2. Confirmar acesso à organização Graventum
3. Verificar permissões do usuário

### ❌ Erro: Space não encontrado
**Solução**:
1. Executar workflow de criação do space
2. Verificar ID do space no payload
3. Confirmar permissões na organização

## 8. Melhorias Futuras

### 🚀 Planejadas
1. **Sincronização bidirecional** (GitBook → OpenClaw)
2. **Versionamento** de conteúdo
3. **Diff visual** entre versões
4. **Webhook do GitBook** para notificar mudanças
5. **Dashboard** de status da sincronização

### 🔧 Técnicas
1. **Cache** de IDs de páginas
2. **Retry automático** para falhas temporárias
3. **Batch processing** para múltiplos arquivos
4. **Compressão** de conteúdo para grandes documentos

## 9. Arquivos Entregues

### 📁 Estrutura de Arquivos
```
workspace/
├── gitbook-sync-solution.md          # Visão geral da solução
├── gitbook-endpoint.js               # Endpoint API completo
├── gitbook-integration-guide.md      # Guia de integração
├── n8n-gitbook-workflow.json         # Workflow principal n8n
├── n8n-create-space-workflow.json    # Workflow criar space
├── sync-initial-workspace.js         # Script sincronização inicial
├── documentacao-gitbook-sync.md      # Esta documentação
└── gitbook-sync-log.json             # Log da sincronização (gerado)
```

## 10. Status Atual

### ✅ Concluído
- [x] Design da arquitetura
- [x] Endpoint API implementado
- [x] Workflows n8n criados
- [x] Script de sincronização inicial
- [x] Documentação completa

### 🔄 Pendente de Implementação
- [ ] Integração endpoint na API Graventum
- [ ] Importação workflows no n8n
- [ ] Criação do space no GitBook
- [ ] Teste de sincronização completa

### 🎯 Próximos Passos Imediatos
1. **Integrar endpoint** na API Graventum (Passo 1 do guia)
2. **Importar workflows** no n8n (Passo 2)
3. **Criar space** "Playbook Operacional" (Passo 3)
4. **Testar sincronização** completa (Passo 4-5)

---

**📞 Suporte**: Em caso de problemas, verificar logs e consultar esta documentação. A solução foi projetada para ser autônoma e não requerer intervenção após configuração inicial.