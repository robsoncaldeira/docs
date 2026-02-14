# Guia: Atualizar Métricas do Site Graventum (graventum.com)

**Data:** 14/02/2026  
**Contexto:** O repositório https://github.com/robsoncaldeira/graventum.com não está acessível (privado ou requer auth). Este guia serve para quando você tiver o projeto aberto localmente.

---

## O que alterar

| Atual (site) | Novo (imagem de referência) |
|--------------|-----------------------------|
| 0+ Hours saved | **500+** Hours saved |
| 0+ Companies served | **15+** Companies served |
| 0% Client satisfaction | **97%** Client satisfaction |

---

## Onde procurar no projeto (Next.js 14)

O site graventum.com usa Next.js 14 + Vercel. As métricas do hero provavelmente estão em:

### 1. Arquivo de dados/config
```
src/data/stats.json
src/config/site.ts
src/constants.ts
content/stats.json
```

### 2. Componente do Hero
```
src/components/Hero.tsx
src/components/Stats.tsx
src/components/HomeHero.tsx
app/(home)/page.tsx
pages/index.tsx
```

### 3. Busca por texto
```bash
cd graventum.com
grep -r "Hours saved" .
grep -r "Companies served" .
grep -r "Client satisfaction" .
grep -r "0+" .
```

---

## Exemplo de alteração (se for JSON)

```json
{
  "hoursSaved": "500+",
  "companiesServed": "15+",
  "clientSatisfaction": "97%"
}
```

## Exemplo de alteração (se for TSX/JSX)

```tsx
// Antes
const stats = [
  { value: "0+", label: "Hours saved" },
  { value: "0+", label: "Companies served" },
  { value: "0%", label: "Client satisfaction" },
];

// Depois
const stats = [
  { value: "500+", label: "Hours saved" },
  { value: "15+", label: "Companies served" },
  { value: "97%", label: "Client satisfaction" },
];
```

---

## Verificação

Após alterar:
1. `npm run dev` — testar localmente
2. Verificar as 3 métricas no hero
3. Commit + push para deploy no Vercel

---

## Credibilidade dos números

Conforme análise anterior: use apenas números que você consiga justificar (diagnósticos, pilots, projetos anteriores). Se 500+, 15+, 97% forem reais ou projetados com base em cases — ok. Se não, ajuste para valores defensáveis.
