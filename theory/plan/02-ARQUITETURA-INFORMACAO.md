# 02 — Arquitetura de Informação

---

## Hierarquia de conteúdo

```
Área Teoria Musical
└── Trilha (curso principal)
    └── Nível (1–8)
        └── Módulo (tema coeso, 3–8 aulas)
            └── Aula (15–25 min leitura + prática)
                ├── Blocos de conteúdo (markdown + componentes)
                ├── Laboratório interativo (opcional)
                ├── Exercício(s)
                └── Referências (obras, aulas relacionadas)
```

### Entidades auxiliares

| Entidade | Função |
|----------|--------|
| **Desafio semanal** | Aplicação cross-módulo ("3 cadências em 3 estilos") |
| **Ficha de referência** | Cola rápida (intervalos, campo, ritmos) |
| **Glossário** | Termos linkados inline |
| **Mapa mental** | Visão do nível inteiro |

---

## Navegação principal

### Hub (`/teoria`)

- Hero com progresso global (% trilha, streak opcional)
- Cards dos 8 níveis (bloqueado / em andamento / concluído)
- Atalhos: "Continuar de onde parei", "Fichas", "Desafio da semana"
- Filtro por eixo: Fundamentos · Harmonia · Ritmo · Arranjo · Brasil

### Sidebar da área

```
Teoria Musical
├── Trilha principal
├── Por eixo
│   ├── Fundamentos
│   ├── Campo harmônico
│   ├── Construção de acordes
│   ├── Progressões & cadências
│   ├── Ritmo & groove
│   ├── Arranjo no violão
│   └── Música brasileira
├── Laboratórios
├── Fichas de referência
└── Glossário
```

### Aula (layout)

```
┌─────────────────────────────────────────────────────────┐
│ Topbar: breadcrumb · busca · tema                       │
├──────────────┬──────────────────────────────────────────┤
│ TOC aula     │ Conteúdo (blocos + componentes)          │
│ (sticky)     │                                          │
│              │ ┌─ Callout ─────────────────────────┐    │
│ Progresso    │ │ Laboratório interativo            │    │
│ mini-bar     │ └───────────────────────────────────┘    │
│              │ Exercício                                │
│ Anterior /   │ Obras relacionadas (cards)               │
│ Próxima      │                                          │
└──────────────┴──────────────────────────────────────────┘
```

---

## Metadados de aula (schema proposto)

```typescript
interface TheoryLesson {
  id: string;                    // "n3-m2-a4-campo-maior"
  slug: string;
  level: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8;
  moduleId: string;
  title: string;
  subtitle?: string;
  durationMin: number;
  prerequisites: string[];       // lesson ids
  tags: string[];                // "campo-harmonico", "bossa", "violao"
  blocks: TheoryBlock[];
  exercises: Exercise[];
  relatedWorks?: string[];       // work ids no catálogo
  relatedChords?: string[];      // nomes p/ dicionário
}

type TheoryBlock =
  | { type: "prose"; markdown: string }
  | { type: "callout"; variant: CalloutVariant; title: string; body: string }
  | { type: "component"; componentId: string; props: Record<string, unknown> }
  | { type: "example"; workId: string; excerpt?: string; annotation?: string };

type CalloutVariant = "tip" | "ear" | "practice" | "br" | "warn" | "deep";
```

---

## Estados de progresso

| Estado | UI | Regra |
|--------|-----|-------|
| `locked` | Card opaco, cadeado | Pré-requisitos não cumpridos |
| `available` | Card normal | Pode iniciar |
| `in_progress` | Barra parcial | Visitou, não completou exercício |
| `completed` | Check verde | Exercício mínimo OK ou "marcar como lido" |
| `mastered` | Estrela (opcional) | Quiz ≥ 80% |

Storage local (MVP): `data/user/theory-progress.json`

---

## Busca

Indexar: títulos, tags, termos do glossário, nomes de acordes, obras citadas.

Sugestões rápidas: "campo harmônico", "II-V-I", "partido alto", "pestana", "modo dórico".

---

## Responsividade

| Breakpoint | Comportamento |
|------------|---------------|
| Desktop | TOC lateral + conteúdo largo; labs em 2 colunas quando couber |
| Tablet | TOC colapsável |
| Mobile | TOC drawer; labs full-width; bottom nav "Teoria" |

---

## Acessibilidade

- Landmarks: `nav`, `main`, `aside`
- Diagramas SVG com `aria-label` e texto alternativo
- Braço interativo: navegação por teclado (setas entre trastes)
- Contraste AA (tokens TocaAí)
- Vídeos futuros: legendas PT-BR
