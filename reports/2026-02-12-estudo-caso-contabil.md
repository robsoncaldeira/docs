# Estudo de Caso — Automação Fantasma para Escritórios Contábeis

**Data:** 2026-02-12  
**Objetivo:** Identificar escritórios contábeis brasileiros ideais para a estratégia "Automação Fantasma" da Graventum

---

## 1. Candidatos Identificados

### 🏆 #1 — Servir Contabilidade (TOP PICK)
- **Cidade:** Belo Horizonte, MG
- **Site:** https://servircontabilidade.com.br
- **Decisor:** **Hilder** (contador responsável e sócio único)
- **Perfil:** Escritório pequeno, especializado em prestadores de serviços da saúde (dentistas, psicólogos, fisioterapeutas, nutricionistas, médicos)
- **Sinais de dor:**
  - ✅ Atendimento 100% via WhatsApp ("direto comigo, Hilder")
  - ✅ Processos manuais evidentes — troca de documentos por software genérico
  - ✅ Uma pessoa centralizando tudo (gargalo operacional)
  - ✅ Depoimentos de clientes mencionam "pontualidade" e "atendimento" — não tecnologia
  - ✅ Sem menção a automação, chatbot, ou portal do cliente sofisticado
  - ✅ Emissão de NF "pelo nosso aplicativo" (provavelmente manual)
  - ✅ Boleto enviado mensalmente (processo manual de cobrança)
- **Por que é ideal:** Escritório one-man-show com gargalo claro no atendimento. Hilder faz tudo. Qualquer automação entregue de graça vai liberar horas do dia dele. Impacto imediato e visível.

---

### #2 — Scalabrini & Associados
- **Cidade:** Belo Horizonte, MG (Centro)
- **Site:** https://www.contabilidadescalabrini.com.br
- **Perfil:** +50 anos de mercado, escritório tradicional grande
- **Serviços:** Contabilidade, Fiscal/Tributário, DP/RH, Pessoa Física, Legalização, Auditoria, Laudos, BPO, Certificado Digital
- **Sinais de dor:**
  - ✅ Enorme lista de serviços manuais (8+ departamentos)
  - ✅ Publica informativos manualmente para clientes ("Prezado Cliente...")
  - ✅ Calculadoras no site (salário líquido, rescisão, etc.) — conteúdo SEO mas sem automação interna
  - ✅ Área do cliente, Holerite eletrônico, Intranet — sistemas legados separados
  - ✅ Blog com atualizações de legislação enviadas manualmente
- **Decisor:** Não identificado diretamente (empresa familiar, buscar no LinkedIn "Scalabrini contabilidade BH")
- **Risco:** Pode ser grande demais (>200 clientes). Mais complexo para abordagem rápida.

---

### #3 — Contabilidade Conexus
- **Cidade:** Belo Horizonte, MG
- **Site:** https://www.contabilidadeconexus.com.br
- **Perfil:** Desde 1982, sede própria, atendimento nacional
- **Serviços:** Fiscal, contábil, pessoal, gestão eletrônica de documentos, legalização societária, assessoria tributária
- **Sinais de dor:**
  - ✅ "Vasta gama de serviços" sem menção a automação
  - ✅ Site extremamente básico (pouco conteúdo extraído = site simples/estático)
  - ✅ 40+ anos = provável resistência mas também muita dor acumulada
- **Decisor:** Buscar no LinkedIn "Conexus contabilidade BH"

---

### #4 — Papyrus Contabilidade
- **Cidade:** Belo Horizonte, MG
- **Site:** https://contabilidadepapyrus.com.br
- **Perfil:** Desde 1983, 4+ décadas, atendimento nacional
- **Sinais de dor:**
  - ✅ Menciona "softwares licenciados" e "backup modernos" — linguagem de quem está tentando parecer moderno mas não é
  - ✅ "Gestão Eletrônica de Documentos" = provavelmente pasta compartilhada ou sistema legado
  - ✅ Site minimalista = baixo investimento digital
- **Nota:** Muito similar ao perfil da Conexus. Provável que Conexus e Papyrus sejam escritórios irmãos ou rebrand (textos quase idênticos).

---

### #5 — Atvis Contabilidade
- **Cidade:** Não identificada claramente (provavelmente SP ou RJ)
- **Site:** https://www.atvis.com.br
- **Perfil:** Escritório tradicional, assessoria completa
- **Sinais de dor:**
  - ✅ Conteúdo genérico sobre MEI, Simples Nacional, eSocial
  - ✅ "Entre em contato pelo WhatsApp" — dependência total
  - ✅ Linguagem educativa no site = tentando atrair leads mas sem funil automatizado
  - ✅ Sem menção a portal do cliente ou automação

---

## 2. Análise TOP 1: Servir Contabilidade

### Por que a Servir é o caso perfeito

| Fator | Situação Atual | Oportunidade |
|-------|---------------|-------------|
| Atendimento | 1 pessoa (Hilder) via WhatsApp | Chatbot + triagem automática |
| Cobrança | Boleto enviado manualmente | Cobrança automática + lembrete |
| Onboarding | "Nós cuidamos de tudo" (manual) | Workflow guiado com checklist |
| Documentos | Software genérico de troca | Upload automatizado + OCR |
| Comunicação | Não há newsletter/updates | Disparo automático de alertas fiscais |
| NF de Serviço | "Pelo nosso aplicativo" | Automação de emissão por trigger |
| Nicho | Saúde (dentistas, médicos, psicólogos) | Comunicação segmentada por profissão |

### Mini-Diagnóstico

**Hilder opera no modelo "eu faço tudo":**
- Ele É o atendimento, o comercial, o operacional
- Cada cliente novo = mais horas do dia dele
- Sem automação, ele escala linearmente (mais clientes = proporcionalmente mais trabalho)
- Risco: burnout, erros, atrasos, clientes insatisfeitos

**O que a Graventum entregaria como "presente":**

Um pacote de 3 automações que economizam ~10h/semana do Hilder.

---

## 3. Workflows Específicos (n8n + WhatsApp Evolution API + Supabase)

### 🤖 Workflow 1: Chatbot de Triagem WhatsApp
**Stack:** Evolution API → n8n → Supabase → Evolution API

```
Trigger: Nova mensagem WhatsApp (webhook Evolution API)
→ n8n: Classifica intenção (AI node ou keyword match)
  - "Quero abrir empresa" → Envia checklist de documentos automático
  - "Dúvida sobre imposto" → Envia FAQ ou agenda horário
  - "Enviar documento" → Instrui upload pelo portal
  - Outro → Encaminha para Hilder com contexto
→ Supabase: Loga conversa + classifica lead/cliente
→ Evolution API: Responde automaticamente
```

**Impacto:** Elimina ~70% das mensagens repetitivas. Hilder só recebe o que precisa de atenção humana.

### 📋 Workflow 2: Onboarding Automático de Novo Cliente
**Stack:** Supabase → n8n → Evolution API → Google Sheets

```
Trigger: Novo registro em Supabase (tabela: clientes)
→ n8n: Dispara sequência de 5 mensagens (dias 0, 1, 3, 5, 7)
  - Dia 0: "Bem-vindo! Aqui está o que precisamos de você" + checklist
  - Dia 1: "Já enviou o contrato social? Clique aqui para upload"
  - Dia 3: "Faltam 2 documentos. Quer ajuda?"
  - Dia 5: "Tudo recebido! Seu CNPJ está em processo"
  - Dia 7: "CNPJ pronto! Aqui estão seus próximos passos"
→ Google Sheets: Atualiza status do onboarding
→ Supabase: Marca etapa concluída
```

**Impacto:** Onboarding que levava 3-4 interações manuais agora roda sozinho. Cliente se sente acompanhado sem Hilder mover um dedo.

### 💰 Workflow 3: Lembrete de Obrigações + Cobrança
**Stack:** Supabase (calendário fiscal) → n8n (cron) → Evolution API

```
Trigger: Cron job diário (8h)
→ n8n: Consulta Supabase - obrigações fiscais dos próximos 5 dias
→ Para cada cliente com obrigação pendente:
  - Evolution API: "Olá [nome], lembrete: seu DAS vence em 3 dias. Valor: R$ X. Pix/boleto: [link]"
→ n8n: Cron semanal - clientes com boleto em aberto >5 dias
  - Evolution API: "Notamos que o boleto de [mês] ainda está em aberto. Precisa de ajuda?"
→ Supabase: Loga envios e status
```

**Impacto:** Zero esquecimento de obrigações. Reduz inadimplência. Cliente percebe profissionalismo.

---

## 4. Estratégia de Abordagem

### Script de Primeiro Contato (LinkedIn/WhatsApp para Hilder)

> "Hilder, tudo bem? Vi que a Servir Contabilidade é especializada em profissionais da saúde em BH — parabéns pelo nicho, faz muito sentido.
>
> Trabalho com automação para escritórios contábeis e montei um chatbot de WhatsApp que faz triagem automática de mensagens de clientes. Quero te dar ele de graça — sem compromisso, sem pegadinha.
>
> É um teste que estou fazendo com escritórios do tamanho da Servir. Se funcionar pra você, ótimo. Se não, sem problemas.
>
> Posso te mostrar em 15 min como funciona?"

### Sequência pós-entrega:
1. **Semana 1:** Entrega o Workflow 1 (chatbot WhatsApp) → funciona imediatamente
2. **Semana 2:** Mostra métricas ("você economizou X horas essa semana")
3. **Semana 3:** Oferece Workflow 2 (onboarding) como "upgrade"
4. **Semana 4:** Propõe contrato mensal para manutenção + Workflow 3 + novos

---

## 5. Estimativa de Valor

| Item | Valor de mercado | Custo Graventum |
|------|-----------------|----------------|
| Chatbot WhatsApp | R$ 2.000-4.000 setup | ~4h de trabalho |
| Onboarding automático | R$ 1.500-3.000 setup | ~3h de trabalho |
| Lembretes + cobrança | R$ 1.000-2.000 setup | ~2h de trabalho |
| **Total "presente"** | **R$ 4.500-9.000** | **~9h de trabalho** |
| **Contrato mensal sugerido** | **R$ 800-1.500/mês** | Manutenção + novas automações |

---

## 6. Próximos Passos

- [ ] Encontrar perfil do Hilder no LinkedIn (buscar "Hilder contador BH Servir")
- [ ] Personalizar o chatbot com FAQs reais de contabilidade para saúde
- [ ] Preparar demo funcional do Workflow 1 (n8n + Evolution API)
- [ ] Abordar via LinkedIn ou WhatsApp direto do site
- [ ] Documentar case para replicar em outros escritórios

---

*Relatório gerado por Graventum AI — Pesquisa de mercado para estratégia Automação Fantasma*
