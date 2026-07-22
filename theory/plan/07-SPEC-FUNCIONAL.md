# 07 — Especificação Funcional (SDD)

| Campo | Valor |
|-------|-------|
| ID | SPEC-12 |
| Domínio | BC Teoria & Pedagogia |
| Status | Planejamento + protótipo |
| Depende de | SPEC-03, SPEC-08, SPEC-10 |
| Complementa | SPEC-07 (estudo por obra) |

---

## 1. Objetivo

Oferecer **trilha de teoria musical** integrada ao TocaAí — conteúdo progressivo, componentes visuais interativos, exercícios e ligação ao catálogo de obras brasileiras, com **foco em violão**.

---

## 2. Escopo MVP (Fase 1)

### Incluído

- Hub teoria com 8 níveis (conteúdo real Nível 1–3, resto placeholder)
- 3 aulas completas demo (Nível 1 aula 1, Nível 3 campo harmônico, Nível 6 bossa)
- 15+ componentes interativos (protótipo HTML)
- Progresso local (JSON)
- Links para Song View e dicionário
- Glossário estático (50 termos)

### Fora de escopo MVP

- Áudio / metrônomo real
- Sync GP ↔ teoria
- Certificados / social
- Conteúdo gerado por IA
- App mobile nativo

---

## 3. Modelo de domínio

```typescript
// Agregado principal
interface TheoryCurriculum {
  id: "main";
  levels: TheoryLevel[];
  glossary: GlossaryEntry[];
}

interface TheoryLevel {
  id: string;
  number: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8;
  title: string;
  description: string;
  modules: TheoryModule[];
}

interface TheoryModule {
  id: string;
  title: string;
  lessons: TheoryLessonMeta[];
}

interface TheoryLessonMeta {
  id: string;
  title: string;
  durationMin: number;
  status: LessonStatus;
}

interface UserTheoryProgress {
  userId: "local";
  completedLessons: string[];
  quizScores: Record<string, number>;
  lastVisited?: string;
  streakDays?: number;
  bookmarks: string[];
}

interface GlossaryEntry {
  id: string;
  term: string;
  definition: string;
  seeAlso?: string[];
}
```

---

## 4. Casos de uso

### UC-01: Entrar na área Teoria

- **Given** usuário no Hub principal
- **When** clica "Teoria Musical" na sidebar
- **Then** vê trilha, progresso % e "Continuar"

### UC-02: Estudar aula

- **Given** aula disponível
- **When** abre aula
- **Then** vê conteúdo, labs interativos, TOC, navegação prev/next

### UC-03: Completar exercício

- **Given** exercício no fim da aula
- **When** responde corretamente (≥ mínimo)
- **Then** aula marcada `completed`, próxima desbloqueada

### UC-04: Usar laboratório (campo harmônico)

- **Given** componente TH-HARMONIC-TABLE
- **When** seleciona tom no círculo de quintas
- **Then** tabela + braço atualizam acordes e shapes

### UC-05: Ir para obra relacionada

- **Given** card TH-WORK-LINK em aula
- **When** clica obra
- **Then** navega Song View com contexto (query `?from=theory&lesson=id`)

### UC-06: Consultar glossário

- **Given** termo linkado inline
- **When** clica
- **Then** popover ou página glossário com definição

---

## 5. Requisitos não-funcionais

| Req | Meta |
|-----|------|
| Performance | Labs interativos < 16ms frame |
| Offline | Conteúdo estático cacheable (SSG) |
| A11y | WCAG 2.1 AA em componentes MVP |
| i18n | PT-BR only (MVP) |
| Dados | Aulas em JSON/Markdown em `data/theory/` |

---

## 6. Estrutura de arquivos (alvo)

```
data/theory/
  curriculum.json
  glossary.json
  lessons/
    n1-m1-a1.json
    n3-m1-a2.json
  assets/           # SVGs estáticos se necessário

src/
  components/theory/
    fretboard.ts
    circle-fifths.ts
    harmonic-table.ts
    chord-builder.ts
    ...
  pages/theory/
    hub.ts
    lesson.ts

apps/styleguide/screens/teoria-musical/   # protótipo (atual)
```

---

## 7. Critérios de aceite (Fase 1 implementação)

- [ ] Hub renderiza 8 níveis com estado progresso
- [ ] ≥ 3 aulas consumíveis de `data/theory/`
- [ ] TH-FRETBOARD, TH-CIRCLE-FIFTHS, TH-HARMONIC-TABLE funcionais
- [ ] TH-CHORD-BUILDER nomeia acorde corretamente (tétrades)
- [ ] Quiz MC persiste score local
- [ ] Links para ≥ 5 obras do catálogo
- [ ] Sidebar app principal inclui entrada Teoria
- [ ] Protótipo HTML documentado no styleguide index

---

## 8. Riscos

| Risco | Mitigação |
|-------|-----------|
| Escopo conteúdo enorme | Trilha completa planejada, produção incremental |
| Componentes complexos | Protótipo valida UX antes de TS |
| Duplicação com dicionário | Teoria explica; dicionário referencia shapes |
| Notação BR vs US | Documento 01 fixa convenções |

---

## 9. Roadmap

| Fase | Entrega | Prazo sugerido |
|------|---------|----------------|
| 0 | Plano + protótipo HTML | ✓ agora |
| 1 | Níveis 1–3 + 8 componentes core | +4 semanas |
| 2 | Níveis 4–6 + ritmo + áudio | +6 semanas |
| 3 | Níveis 7–8 + overlay Song View | +6 semanas |
