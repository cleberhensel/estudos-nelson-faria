# 03 — Componentes UI (Teoria Musical)

> Catálogo de componentes visuais e interativos. Protótipo em `apps/styleguide/screens/teoria-musical/componentes.html`.

---

## Legenda

| Símbolo | Significado |
|---------|-------------|
| 🟢 | MVP (protótipo + Fase 1) |
| 🟡 | Fase 2 |
| 🔵 | Fase 3+ |

---

## 1. Shell & navegação

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-SHELL` | App shell teoria | Sidebar própria + breadcrumb + progresso global | 🟢 |
| `TH-HUB-LEVEL` | Card de nível | Nível 1–8, % conclusão, módulos filhos | 🟢 |
| `TH-MODULE-CARD` | Card de módulo | Título, duração, tags, lock state | 🟢 |
| `TH-LESSON-TOC` | Sumário da aula | Âncoras sticky, progresso scroll-spy | 🟢 |
| `TH-PATH` | Trilha visual | Linha conectando aulas concluídas (timeline horizontal/vertical) | 🟢 |
| `TH-PROGRESS-RING` | Anel de progresso | Hub hero — % trilha | 🟡 |

---

## 2. Tipografia & conteúdo

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-CALLOUT` | Callout | Variantes: tip, ear, practice, br, warn, deep | 🟢 |
| `TH-PROSE` | Bloco texto | Markdown enriquecido com links glossário | 🟢 |
| `TH-GLOSS-LINK` | Link glossário | Popover definição curta | 🟡 |
| `TH-QUOTE-SONG` | Citação musical | Trecho cifra + contexto obra | 🟢 |
| `TH-STEP-LIST` | Lista numerada | Passos de exercício no violão | 🟢 |
| `TH-COMPARE` | Comparador lado a lado | Ex: acorde simples vs. riqueza bossa | 🟢 |

---

## 3. Violão & braço

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-FRETBOARD` | Braço interativo | 6 cordas, highlight notas/acordes/escalas | 🟢 |
| `TH-FRETBOARD-SCALE` | Escala no braço | Padrão CAGED ou 3NPS selecionável | 🟢 |
| `TH-FRETBOARD-CHORD` | Acorde no braço | Dots + pestana + dedos (reusa gp-fretboard) | 🟢 |
| `TH-STRING-LABEL` | Indicador cordas | E A D G B e + afinação | 🟢 |
| `TH-CAPO-BAR` | Simulador capo | Desloca casa base | 🟡 |
| `TH-FINGERING` | Diagrama dedilhado | T/1/2/3/4 por corda | 🟢 |

---

## 4. Teoria visual (não-violão)

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-KEYBOARD` | Teclado 1 oitava | Highlight intervalos/escala | 🟢 |
| `TH-INTERVAL-RULER` | Régua de intervalos | Semitons visuais entre duas notas | 🟢 |
| `TH-CIRCLE-FIFTHS` | Círculo de quintas | Clicável — seleciona tom, mostra armadura | 🟢 |
| `TH-NOTE-STAFF` | Pentagrama simplificado | Opcional Fase 2 — violão prioriza tab | 🔵 |

---

## 5. Harmonia

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-HARMONIC-TABLE` | Tabela campo harmônico | Graus I–VII, funções, acordes, emojis função | 🟢 |
| `TH-CHORD-BUILDER` | Construtor empilhamento | Terças: 1–3–5–7–9… preview nome | 🟢 |
| `TH-CHORD-FORMULA` | Fórmula acorde | Blocos coloridos (fundamental, terça, etc.) | 🟢 |
| `TH-PROGRESSION` | Sequência acordes | Chips clicáveis + play visual | 🟢 |
| `TH-CADENCE-FLOW` | Fluxograma cadência | Nós: pré-dominante → dominante → tônica | 🟢 |
| `TH-FUNCTION-MAP` | Mapa funções T/SD/D | Diagrama tonal estilo Schenker simplificado | 🟡 |
| `TH-MODULATION` | Diagrama modulação | Setas entre tons (choro, MPB) | 🟡 |
| `TH-SUBSTITUTE` | Tabela substituições | trítono, backdoor, modal interchange | 🟡 |

---

## 6. Ritmo & groove (Brasil)

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-RHYTHM-GRID` | Grade rítmica | Células on/off, padrão bossa/samba/baião | 🟢 |
| `TH-PATTERN-CARD` | Card padrão | Nome, gênero, BPM sugerido, diagrama mão direita | 🟢 |
| `TH-STRUM-PATTERN` | Setas dedilhado | ↓ ↑ p + acentos | 🟢 |
| `TH-METRONOME-MOCK` | Metrônomo visual | Pulso + subdivisão (sem áudio MVP) | 🟡 |

---

## 7. Arranjo

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-VOICING-GRID` | Grade voicings | Acorde × registro (grave/médio/agudo) | 🟢 |
| `TH-BASS-LINE` | Linha de baixo | Números traste cordas E/A | 🟢 |
| `TH-TEXTURE-LAYERS` | Camadas arranjo | Solo / harmonia / baixo / percussão violão | 🟡 |
| `TH-ARRANGE-CHECKLIST` | Checklist arranjo | Intro, verso, pontes, outro | 🟡 |

---

## 8. Exercícios & avaliação

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-QUIZ-MC` | Múltipla escolha | 4 opções, feedback imediato | 🟢 |
| `TH-QUIZ-IDENTIFY` | Identificar no braço | "Clique a terça de Dó" | 🟡 |
| `TH-QUIZ-ORDER` | Ordenar | Montar cadência arrastando chips | 🟢 |
| `TH-PRACTICE-TIMER` | Timer prática | 2/5/10 min + checklist | 🟡 |
| `TH-SELF-CHECK` | Autoavaliação | "Consegui tocar sem olhar?" | 🟢 |

---

## 9. Integração catálogo

| ID | Nome | Descrição | Status |
|----|------|-----------|--------|
| `TH-WORK-LINK` | Card obra relacionada | Link Song View + acordes destacados | 🟢 |
| `TH-CHORD-LINK` | Link dicionário | Inline → diagrama | 🟢 |
| `TH-APPLY-BANNER` | Banner "Aplicar" | "Abrir Garota de Ipanema com graus" | 🟡 |

---

## Props comuns (TypeScript)

```typescript
interface FretboardProps {
  root: string;              // "C"
  mode?: "chord" | "scale" | "intervals";
  scale?: string;            // "major", "dorian"
  chord?: string;            // "Am7"
  highlightFrets?: number[][]; // [string, fret][]
  baseFret?: number;
  showNotes?: boolean;
  interactive?: boolean;
}

interface HarmonicTableProps {
  key: string;               // "C"
  mode: "major" | "minor";
  showFunctions?: boolean;
  showBrazilianExamples?: boolean;
  onDegreeClick?: (degree: string) => void;
}

interface ChordBuilderProps {
  root: string;
  layers: ("3" | "5" | "7" | "9" | "11" | "13")[];
  quality: "M" | "m" | "7" | "m7" | "7M" | "dim" | "aug";
}
```

---

## Tokens CSS adicionais (teoria)

```css
--th-fundamental: var(--accent);
--th-third: var(--accent-2);
--th-fifth: var(--violet);
--th-seventh: var(--amber);
--th-extension: var(--rose);
--th-tonic-fn: #34d399;
--th-subdom-fn: #22d3ee;
--th-domin-fn: #fbbf24;
--th-predom-fn: #a78bfa;
```

Arquivo: `teoria.css` estende `tocai.css` — não duplica shell.

---

## Matriz componente × módulo

| Componente | Fund. | Campo | Acordes | Progr. | Ritmo | Arranjo | BR |
|------------|-------|-------|---------|--------|-------|---------|-----|
| Fretboard | ● | ● | ● | ● | ○ | ● | ● |
| Circle fifths | ○ | ● | ○ | ● | ○ | ○ | ○ |
| Harmonic table | ○ | ● | ● | ● | ○ | ● | ● |
| Chord builder | ● | ○ | ● | ○ | ○ | ● | ○ |
| Rhythm grid | ○ | ○ | ○ | ○ | ● | ○ | ● |
| Cadence flow | ○ | ● | ○ | ● | ○ | ○ | ● |
| Work link | ● | ● | ● | ● | ● | ● | ● |

● = uso central · ○ = uso ocasional
