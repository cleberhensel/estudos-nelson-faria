# 07 — Stack MVP e Matriz de Decisão

## Stack recomendado final (violão + microfone + browser)

### Dependências npm (MVP)

| Pacote | Versão | Modo | Peso |
|--------|--------|------|------|
| `pitchy` | 4.x | Nota | ~15 KB |
| `tone` | 15.x | Metrônomo | ~200 KB |
| `essentia.js` | latest | Onset (opcional) | ~2 MB WASM |
| `@spotify/basic-pitch` | 1.x | Acorde Worker | ~1 MB + model |
| `@playground-sessions/pitch-detection-analysis` | 0.1.x | Poly fallback | ~500 KB + TF |

**MVP mínimo:** `pitchy` + `tone` + diff custom (~zero ML).

**MVP completo:** + `@spotify/basic-pitch` no Worker.

---

## Matriz modo × tecnologia × sprint

| Modo | Sprint | Hot path | Warm path | Referência |
|------|--------|----------|-----------|------------|
| **Nota/afinador** | 1 | Pitchy Worklet | — | JSON `NoteTarget` |
| **Acorde estático** | 2 | Onset Worklet | Basic Pitch Worker | JSON voicing |
| **Progressão** | 3 | + Tone Transport | diff + timing | JSON steps[] |
| **Import GP** | 4 | — | Python/PyGuitarPro | GP5 → JSON |

---

## Ranking completo — 25 tecnologias

| # | Tech | Cat. | Score | Sprint |
|---|------|------|-------|--------|
| 1 | Pitchy (McLeod) | Pitch | 9.5 | 1 |
| 2 | Pitch-set diff (custom) | Acorde | 9.5 | 2 |
| 3 | Tone.js Transport | Ritmo | 9.0 | 3 |
| 4 | RMS/flux onset (JS) | Ritmo | 9.0 | 2 |
| 5 | JSON voicing schema | Ref | 9.0 | 1 |
| 6 | Essentia.js WASM | Onset | 8.5 | 2+ |
| 7 | badlogic/tuner YIN | Pitch | 8.5 | 1 alt |
| 8 | @playground-sessions/pitch-detection | Poly | 8.5 | 2+ |
| 9 | @spotify/basic-pitch | AMT window | 8.0 | 2 |
| 10 | audiojs/pitch-detection NNLS | Chroma | 7.5 | 3 opt |
| 11 | CREPE tiny TF.js | Pitch | 7.5 | 1 alt |
| 12 | aubiojs | Pitch/onset | 7.0 | alt |
| 13 | PyGuitarPro → JSON | Author | 8.0 | 4 |
| 14 | music21 MusicXML | Author | 8.0 | 4 |
| 15 | HPS peak pick (custom) | Acorde | 7.5 | 2 |
| 16 | SuperFlux (calibrate) | Ritmo | 7.0 | 3 |
| 17 | Solitito ONNX | Chord ID | 6.5 | future |
| 18 | chord_detector Rust | Chord ID | 6.0 | future |
| 19 | Essentia ChordsDetection | Chord ID | 5.5 | avoid MVP |
| 20 | CREMA | Chord ID | 5.0 | avoid MVP |
| 21 | Demucs | Stems | 3.0 | N/A |
| 22 | MT3/MIROS | Multi AMT | 2.0 | N/A |
| 23 | ACRCloud | ID música | 1.0 | N/A |
| 24 | Audiveris OMR | Scan | 4.0 | side |
| 25 | LLM feedback | Pedagogia | 5.0 | pós-sessão |

---

## Métricas de sucesso (eval interno)

Usar **GuitarSet** + simulação mic:

| Métrica | Alvo MVP | Ferramenta |
|---------|----------|------------|
| Pitch cents MAE (mono) | < 15 cents | mir_eval |
| Chord pitch recall | > 85% | custom vs JAMS |
| Onset error | < 100 ms vs GT | mir_eval |
| False onset rate | < 5% / min | logs |
| Worklet CPU | < 15% desktop | profiler |

---

## Riscos técnicos

| Risco | Mitigação |
|-------|-----------|
| Polyphony confunde Pitchy | Gate Modo; NMF/Basic Pitch |
| Latência Basic Pitch | Só pós-strum, spinner UI |
| iOS AudioContext | user gesture, Tone.start() |
| Voicing alternativo válido | `allowedVoicings[]` na lição |
| Ruído ambiente | calibrar gate 1s |

---

## Roadmap 4 sprints (recap)

### Sprint 1 — Afinador (2 sem)
- AudioWorklet + Pitchy
- JSON NoteTarget
- UI cents + agulha

### Sprint 2 — Acorde (3 sem)
- Onset RMS
- HPS + diffVoicing
- Basic Pitch fallback Worker
- 5 acordes abertos

### Sprint 3 — Ritmo (2 sem)
- Tone.js metrônomo
- Progressão 2 acordes
- onset_error UI

### Sprint 4 — Content (2 sem)
- GP5 → JSON pipeline
- Diagrama cordas UI

---

## Conclusão

O tutor de violão ao vivo **não precisa** do stack pesado de extração de músicas gravadas. Precisa de:

1. **Pitch monofónico rápido** (Pitchy/YIN)
2. **Diff voicing** contra lição JSON (não Chordify)
3. **Onset + metrônomo** (RMS + Tone.js)
4. **Worker ML opcional** (Basic Pitch) para acordes densos

Esta stack é **implementável inteiramente no browser**, alinhada com patentes de mercado (Yousician), e mensurável com GuitarSet.

---

## Referências bibliográficas

1. McLeod & Wyvill, 2005 — McLeod Pitch Method
2. Cheveigné & Kawahara, 2002 — YIN
3. Fujishima, 1999 — Realtime chord recognition (chroma templates)
4. Mauch & Dixon, 2010 — NNLS chroma
5. Kim et al., 2018 — CREPE
6. Bittner et al., 2022 — Basic Pitch (ICASSP)
7. Correya et al., 2020 — Essentia.js (ISMIR)
8. US9218748B2 — Yousician exercise system
9. Xi et al., 2018 — GuitarSet (ISMIR)
