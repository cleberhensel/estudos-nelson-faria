# Teoria Musical — Implementação Estrutural

> **Status:** infra T0–T5 entregue no app Vite/TS  
> **Data:** 2026-06-08  
> **Plano origem:** `.cursor/plans/teoria_infra_estrutural_5d4df519.plan.md`

---

## Entregue

### T0 — Fundação

| Item | Local |
|------|-------|
| Rotas hash | `#/theory`, `#/theory/level/:n`, `#/theory/lesson/:id`, `#/theory/lab/:id`, `#/theory/glossary` — `src/router.ts`, `src/main.ts` |
| Schemas Zod | `src/lib/schemas.ts` (curriculum, lesson, blocks, glossary, index, annotation, progress) |
| Dados | `data/theory/` — curriculum, glossary, lessons stub, annotations |
| Loaders | `loadCurriculum`, `loadLesson`, `loadGlossary`, `loadTheoryIndex`, `loadTheoryAnnotation` |
| Páginas | `src/pages/theory/hub.ts`, `lesson.ts`, `lab.ts`, `glossary.ts` |
| Shell | `src/components/theory/theory-shell.ts` |
| Nav app | item **Teoria** em `src/components/app-shell.ts` |
| Validação | `scripts/validate.ts` + `scripts/build-theory-index.ts` |

### T1 — Kernel musical

`src/lib/music/` — `notes`, `intervals`, `scales`, `harmonic-field`, `chord-spelling`, `cadences`, `transpose`.

Testes: `tests/theory/harmonic.test.ts`.

### T2 — Componentes TH-*

`src/components/theory/` — registry, callout, harmonic-lab, chord-builder, cadence-keyboard, work-link, chord-link, progress-ring, interval-ruler, render-blocks.

Estilos: `src/styles/theory.css` (espelhado no styleguide HTML).

### T3 — Motor de aula + progresso

- `src/lib/theory/lesson-engine.ts` — navegação prev/next, progresso por nível
- `src/lib/theory/progress.ts` — localStorage `tocai:theory-progress`
- `src/lib/theory/exercises.ts` + `exercises-ui.ts` — mc, order, self_check
- Scroll-spy TOC em `render-blocks.ts`
- Export/import via `src/lib/prefs.ts` (`theoryProgress` em backup JSON)

### T4 — Integração cross-app

| Fluxo | Implementação |
|-------|---------------|
| Teoria → Obra | query `?from=theory` + banner em `src/pages/work.ts` |
| Teoria → Dicionário | link construtor em `src/pages/chords.ts` |
| Obra → Teoria | `TheoryAnnotation` + overlay graus na cifra (`song-panel.ts`) |
| Estudo obra → Teoria | links em `src/components/study-panel.ts` |
| work-link / chord-link | componentes registry |

Anotação exemplo: `data/theory/annotations/oceano.json`.

### T5 — Labs + pipeline

- Labs standalone: `#/theory/lab/campo?key=G`, `#/theory/lab/chord-builder`
- `build-theory-index.ts` → `data/theory/index.json`
- Busca hub: `src/lib/theory/search.ts` (index + fallback glossário)

---

## Como testar

```bash
npm run dev
# Abrir #/theory
# Aula demo: #/theory/lesson/n3-m1-a1 (sem pré-requisito)
# Depois: #/theory/lesson/n3-m1-a2
# Overlay graus: #/works/oceano (toggle "Graus harmônicos")
```

```bash
npm test
npm run build:data
npm run validate
npm run build
```

---

## Conteúdo editorial (fora deste escopo)

Preencher incrementalmente `data/theory/lessons/*.json` — infra valida via Zod e `build-theory-index`.

---

## Próximos incrementos opcionais

- Mais aulas e annotations por obra
- Metrônomo no rhythm-grid (reusar `src/lib/metronome.ts`)
- Sync GP ↔ overlay harmônico
- Cloud sync de progresso
