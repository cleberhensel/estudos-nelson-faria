# 10 — Guia de Authoring de Conteúdo (Teoria Musical)

> Como transformar a pesquisa pedagógica e harmônica em aulas JSON validáveis.
> Lê junto com [01-VISAO-E-TOM-DE-VOZ.md](./01-VISAO-E-TOM-DE-VOZ.md), [04-CURRICULO-PROGRESSIVO.md](./04-CURRICULO-PROGRESSIVO.md) e [05-CONTEUDO-APROFUNDADO.md](./05-CONTEUDO-APROFUNDADO.md).

---

## 1. Tríade de contexto (princípio mestre)

O aluno-alvo já toca por cifra. A teoria é **legenda do que ele já sente**. Todo conceito-núcleo deve aparecer em **três lugares ao mesmo tempo**:

1. **No braço** (visual) — `fretboard` / `chord-builder` / `harmonic-lab`.
2. **No som** (auditivo) — `ear-trainer`, `progression` (tocável), bloco `exercise` `dictation`.
3. **Numa obra brasileira real** (cultural) — bloco `example` com `workId`, `work-link`, ou `TheoryAnnotation`.

Fluxo da aula: **tocar/ouvir → nomear → regra → recuperar → criar**.

---

## 2. Modelo pedagógico (P1–P9) → bloco JSON

| # | Princípio | Como vira bloco |
|---|-----------|-----------------|
| P1 | **Concreto → abstrato** (dual coding) | Ordene: `component` (faz/ouve) → `prose` (nomeia) → `callout` → `exercise`. Nunca só texto. |
| P2 | **Recuperação ativa** | Abra a aula com `exercise` `intro` puxando a aula anterior; feche conceito com pergunta. |
| P3 | **Espaçamento** | Use `review: { concept, intervalDays }` no exercício; bloco final "Revisão" mistura aulas N-1, N-3, N-7. |
| P4 | **Intercalação** | Exercícios finais misturam 1–2 conceitos já vistos, não só o da aula. |
| P5 | **Scaffolding / carga cognitiva** | 1 conceito-núcleo por aula; `callout` `tip` para apoio sob demanda; `difficulty` cresce. |
| P6 | **Escada de Bloom** | Sequência de `kind`: `mc` (lembrar) → `match`/`order` (entender) → `build` (aplicar) → `dictation` (analisar) → criar. |
| P7 | **Ancorar no instrumento** | Todo conceito abstrato ganha um `fretboard`/`chord-builder` "no seu braço". |
| P8 | **Feedback formativo** | `feedbackOk`/`feedbackErr` por alternativa; `confidencePrompt` antes de revelar. |
| P9 | **Gamificação saudável** | Progresso por maestria (quiz ≥70% desbloqueia), sem streak punitivo. |

---

## 3. Contrato de blocos (`TheoryLesson.blocks`)

Schema em [`src/lib/schemas.ts`](../../src/lib/schemas.ts).

### `prose`
```json
{ "type": "prose", "id": "intro", "markdown": "Texto **em markdown** simples." }
```

### `callout` — variantes: `tip` `ear` `practice` `br` `warn` `deep`
```json
{ "type": "callout", "variant": "br", "title": "História", "body": "..." }
```
- `ear` = "ouça isto"; `practice` = micro-prática no violão; `br` = contexto brasileiro; `deep` = aprofundamento opcional.

### `component`
```json
{ "type": "component", "id": "lab", "componentId": "harmonic-lab", "props": { "defaultKey": "C", "mode": "major" } }
```

### `example` (obra real)
```json
{ "type": "example", "workId": "garota-de-ipanema", "excerpt": "Fmaj7  G7  Gm7 C7", "annotation": "II7 lídio-b7, não V/V." }
```

### `exercise` — kinds: `mc` `order` `self_check` `match` `dictation` `build` `true_false`
```json
{ "type": "exercise", "exerciseId": "ex-n3-a2-q1", "kind": "mc", "difficulty": "core",
  "question": "Qual a função de G7 em Dó maior?",
  "options": [
    { "label": "Dominante", "correct": true, "feedbackOk": "Isso — tensão que resolve em C." },
    { "label": "Subdominante", "feedbackErr": "Não — subdominante é F7M (IV)." }
  ] }
```

---

## 4. Tipos de exercício (campos por kind)

| kind | Campos | Uso (Bloom) |
|------|--------|-------------|
| `mc` | `options[]` com `correct` | Lembrar/entender |
| `true_false` | `answer: bool`, `options[0].feedbackOk/Err` | Lembrar |
| `order` | `orderAnswer: string[]` | Entender (sequência/cadência) |
| `match` | `pairs: [{left,right}]` | Entender (associar grau↔função↔acorde) |
| `build` | `buildChoices: string[]`, `buildAnswer: string[]` | Aplicar (montar acorde/escala) |
| `dictation` | `playChords: string[]`, `options[]` | Analisar (ouvir → identificar) |
| `self_check` | `question` | Metacognição/prática |

Campos pedagógicos comuns: `tags[]`, `difficulty: intro|core|challenge`, `confidencePrompt`, `review: { concept, intervalDays }`.

**Desbloqueio (P9):** a aula conta como concluída quando a média dos exercícios respondidos atinge `PASSING_THRESHOLD` (70%) — ver [`progress.ts`](../../src/lib/theory/progress.ts).

---

## 5. `componentId` válidos

Fonte da verdade: [`registry-types.ts`](../../src/lib/theory/registry-types.ts).

| componentId | Props principais | Para |
|-------------|------------------|------|
| `harmonic-lab` / `harmonic-table` / `circle-fifths` | `defaultKey`, `mode` (`major`/`minor`/`minorHarmonic`) | Campo harmônico |
| `chord-builder` | — | Empilhar terças |
| `keyboard` | — | Tríade no teclado |
| `interval-ruler` | `from`, `to` | Intervalo |
| `cadence-flow` | `key`, `variant` | Cadência estática |
| `progression` / `modulation` | `key`, `mode`, `chords[]`, `functions[]` | Progressão tocável |
| `fretboard` / `fretboard-scale` / `fretboard-chord` | `key`, `scale`, `chord`, `frets` | Braço (notas/escala/acorde) |
| `ear-trainer` | `key`, `degrees[]` | Percepção de graus |
| `voicing-grid` | `title`, `voicings: [{label,chord,shape,note}]` | Comparar voicings |
| `rhythm-grid` | `pattern` (`bossa`/`samba`/`choro`/`baiao`), `bpm` | Levadas |
| `gloss-link` | `term`, `id`, `def` | Termo de glossário inline |
| `work-link` | `workId`, `title`, `lessonId` | Link p/ Song View |
| `chord-link` | `chord`, `label` | Link p/ dicionário |
| `progress-ring` | `percent` | Anel de progresso |

`scale` aceita: `major`, `minor`, `penta-major`, `penta-minor`, `dorian`, `mixolydian`.

---

## 6. Checklist por aula (padrão mínimo)

- [ ] `id`/`level`/`moduleId`/`title`/`durationMin` corretos
- [ ] `tags[]` e (quando houver) `relatedWorks[]` / `relatedChords[]` preenchidos
- [ ] Abertura com `exercise` `intro` (recuperação — P2) a partir da 2ª aula do módulo
- [ ] ≥1 `component` ancorado no instrumento (P7)
- [ ] ≥1 `callout` `practice` (micro-prática no violão)
- [ ] Quando MPB: 1 `example`/`work-link` + (ideal) `TheoryAnnotation` da obra
- [ ] Escada de Bloom: do `mc`/`true_false` ao `build`/`dictation`/criar (P6)
- [ ] Blocos-chave têm `id` (entram no sumário/scroll-spy)
- [ ] Aulas `*-a{última}` do módulo = quiz de nível (≥5 perguntas)

---

## 7. Pipeline e qualidade

```bash
npm run build:theory      # reindexa + valida componentIds + avisos de obras órfãs
npm run validate          # valida todos os JSON contra schema
npm run audit:theory      # score de qualidade editorial (meta ≥70)
npm run audit:theory:ci   # falha CI se alguma aula < 70 ou refs órfãs
npm test                  # testes de kernel/exercícios
```

IA pode gerar o **rascunho** a partir das análises de [05-CONTEUDO](./05-CONTEUDO-APROFUNDADO.md) — sempre com **revisão humana** antes de `validate`. Nunca conteúdo em massa sem revisão.

---

## 8. Receita por nível (exemplos compactos)

### N1 — Intervalos (`n1-m3-a1`)

```json
{ "type": "component", "componentId": "play-interval", "props": { "from": "C", "to": "D", "label": "Segunda maior" } }
{ "type": "component", "componentId": "fretboard", "props": { "frets": 12, "key": "C" } }
{ "type": "example", "workId": "flor-de-lis", "excerpt": "Am7  D7  G7M", "annotation": "Terças na voz guia." }
```

### N3 — Campo harmônico (`n3-m1-a2` = padrão ouro)

Ver [`data/theory/lessons/n3-m1-a2.json`](../../data/theory/lessons/n3-m1-a2.json) e [`_template.json`](../../data/theory/lessons/_template.json).

Mapa de obras: [11-MAPA-OBRAS-PEDAGOGICAS.md](./11-MAPA-OBRAS-PEDAGOGICAS.md).

**Profundidade editorial:** [12-ENRIQUECIMENTO-AULAS.md](./12-ENRIQUECIMENTO-AULAS.md). Não usar `npm run upgrade:theory` em aulas já enriquecidas — ele só preenche checklist. Validar com `npm run audit:theory:depth`.
