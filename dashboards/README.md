# 📊 Dashboard Central - Graventum

**Localização:** `/root/.openclaw/workspace/html-dashboard/`

## 🎯 OBJETIVO
Centralizar todos os dashboards, relatórios e ferramentas HTML em uma única pasta para fácil acompanhamento.

## 📁 CONTEÚDO DA PASTA

### 1. 📈 DASHBOARDS PRINCIPAIS
| Arquivo | Descrição | Status |
|---------|-----------|--------|
| `index.html` | **Página principal** com links para todos os dashboards | ✅ Ativo |
| `graventum-dashboard.html` | Dashboard principal com métricas gerais | ✅ Ativo |
| `leads-dashboard.html` | Dashboard de leads (geração + qualificação) | ✅ Ativo |
| `pipeline-dashboard.html` | Pipeline completo de automação | ✅ Ativo |
| `project-timeline.html` | Timeline do projeto + calendário de postagens | ✅ Ativo |

### 2. 🤖 AUTOMAÇÕES
| Arquivo | Descrição | Status |
|---------|-----------|--------|
| `n8n-dashboard.html` | Status das automações n8n | ✅ Ativo |
| `ready-to-post.html` | Conteúdo Instagram pronto para postagem | ✅ Automatizado |

### 3. 📱 MARKETING
| Arquivo | Descrição | Status |
|---------|-----------|--------|
| `instagram-manual-guide.html` | Guia manual para postagem no Instagram | ✅ Pronto |
| `graventum-site-preview.html` | Preview do site Graventum | ✅ Ativo |

## 🔄 FLUXO DE DADOS

### ✅ JÁ IMPLEMENTADO:
1. **Leads → Database** - Automático via scripts
2. **Instagram → Automatizado** - Conteúdo gerado automaticamente
3. **Dashboards → Atualizados** - HTMLs gerados automaticamente

### ⚠️ PRÓXIMOS PASSOS:
1. **LinkedIn → Automatizar** - Próxima prioridade
2. **Calendário → Integrar** - Já na Timeline do Projeto
3. **Database → Dashboard** - Conectar visualização

## 🚀 COMO USAR

### 1. ACESSO RÁPIDO
```bash
# Navegar para a pasta
cd /root/.openclaw/workspace/html-dashboard/

# Abrir página principal
open index.html  # ou navegador de sua preferência
```

### 2. LINKS DIRETOS
- **Dashboard Principal:** `file:///root/.openclaw/workspace/html-dashboard/graventum-dashboard.html`
- **Leads:** `file:///root/.openclaw/workspace/html-dashboard/leads-dashboard.html`
- **Timeline:** `file:///root/.openclaw/workspace/html-dashboard/project-timeline.html`

### 3. ATUALIZAÇÃO AUTOMÁTICA
Os dashboards são atualizados automaticamente por:
- Cron jobs de lead hunting
- Scripts de geração de conteúdo
- Automações n8n

## 📊 STATUS ATUAL

### ✅ CONCLUÍDO:
- [x] **Todos os HTMLs movidos** para pasta única
- [x] **Index criado** com navegação centralizada
- [x] **Leads → Database** funcionando
- [x] **Instagram automatizado** funcionando
- [x] **Dashboards organizados** por categoria

### 🚧 EM ANDAMENTO:
- [ ] **LinkedIn automation** - Próximo passo
- [ ] **Database visualization** - Conectar dashboards
- [ ] **Real-time updates** - Melhorar atualização automática

### 📅 PRÓXIMAS AÇÕES

#### 🟢 PRIORIDADE 1 (Esta Semana)
1. **Automatizar LinkedIn** - Sistema de postagem automática
2. **Integrar calendário** - Timeline com datas reais
3. **Dashboard database** - Visualização dos leads no database

#### 🟡 PRIORIDADE 2 (Próximas 2 Semanas)
4. **Notificações automáticas** - Alertas por email/WhatsApp
5. **Relatórios semanais** - Geração automática
6. **Otimização SEO** - Dashboards indexáveis

#### 🔵 PRIORIDADE 3 (Próximo Mês)
7. **API pública** - Acesso programático aos dashboards
8. **Mobile app** - Versão mobile dos dashboards
9. **Integração completa** - Todos os sistemas conectados

## 🔧 MANUTENÇÃO

### ATUALIZAÇÃO MANUAL (se necessário)
```bash
# Atualizar todos os dashboards
cd /root/.openclaw/workspace
./tools/update-all-dashboards.sh

# Verificar status
cd html-dashboard
ls -la *.html
```

### BACKUP AUTOMÁTICO
```bash
# Backup diário automático
./tools/backup-dashboards.sh
```

## 📞 SUPORTE

### PROBLEMAS COMUNS:
1. **Arquivo não encontrado** - Verificar se script de atualização rodou
2. **Links quebrados** - Executar `./tools/fix-links.sh`
3. **Dados desatualizados** - Verificar cron jobs

### SOLUÇÕES:
- **Reexecutar scripts:** `./tools/regenerate-dashboards.sh`
- **Verificar logs:** `tail -f /var/log/graventum-dashboards.log`
- **Reiniciar serviços:** `systemctl restart graventum-dashboards`

---

**Última atualização:** 14 Fevereiro 2026 - 01:36 UTC  
**Próxima revisão:** 15 Fevereiro 2026 - 09:00 UTC  
**Status:** ✅ **Organizado e operacional**

---
*Esta documentação é mantida automaticamente pelo sistema Graventum.*