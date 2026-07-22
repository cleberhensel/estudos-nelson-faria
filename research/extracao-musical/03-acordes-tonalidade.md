# 03 — Acordes e Tonalidade

## Definição

**Extração de acordes** produz sequência `(timestamp, símbolo)` — ex.: `0.0–2.4s: Am7`, `2.4–4.8s: Gmaj7`.

**Detecção de tom (key)** estima tonalidade global ou local: `A minor`, `F# major`.

**Cifra** (lead sheet brasileiro/português) usa mesma simbologia: `Am7`, `G/B`, `F#m7(b5)` — parsing é problema de **NLP musical + harmonia**, não só MIR.

---

## Ranqueamento — Reconhecimento de acordes em áudio

| Rank | Sistema | Vocabulário | Dados treino | Força | Fraqueza |
|------|---------|-------------|--------------|-------|----------|
| **1** | **ChordFormer** (2025) | Grande (triads→extended) | Humphrey & Bello 1217 songs | SOTA rare chords | Pesado, pesquisa |
| **2** | **madmom DeepChroma** | Harte + extensions | Billboard, Isophonics | Pipeline maduro | Python only |
| **3** | **CREMA** | 602 classes + inversões | McFee & Bello structured | JAMS output, CLI | Só acordes |
| **4** | **Essentia ChordsDetection** | Template HPCP | Clássico MIR | **Essentia.js WASM** | Pop complexo |
| **5** | **Chordino** (Vamp) | NNLS Chroma + HMM | Plugin C++ | Usado pelo **Chordify** | Precisa host Vamp |
| **6** | **librosa + HMM custom** | Configurável | DIY | Prototipagem | Qualidade variável |

---

## Análise aprofundada — Referências-chave

### ChordFormer (arXiv 2502.11840)

- Arquitetura **Conformer** (conv + transformer) para dependências longas
- **Reweighted loss** para acordes raros (sétimas, sus, alterações)
- Avaliado em **1217 músicas** (Isophonics + Billboard + MARL), 5-fold CV
- Supera métodos de large-vocabulary transcription + decomposição estrutural

**Lição:** progresso recente vem de **vocabulário grande + balanceamento de classes**, não só mais dados.

---

### Tese "Chord Recognition with Deep Learning" (Dez 2025, arXiv:2512.22621)

Findings empíricos importantes:
- Classificadores ** falham em acordes raros**
- **Pitch augmentation** melhora accuracy significativamente
- Features de modelos generativos **não ajudaram**
- **Dados sintéticos** (AAM — Artificial Audio Multitracks) promissores para treino
- Integração com **beat detection** melhora interpretabilidade e resultados

**Implicação produto:** exibir acordes **alinhados ao beat** (não frame arbitrário) — Essentia `ChordsDetectionBeats`.

---

### Chordino / NNLS Chroma

- Plugin Vamp open source ([isophonics.net/nnls-chroma](http://www.isophonics.net/nnls-chroma))
- **Chordify** usa NNLS Chroma como base do engine de acordes (confirmado no site do projeto)
- McGill Billboard inclui NNLS Chroma pré-computado para todas as faixas

**Pipeline Essentia + Chordino:** benchmark ACE2017 (`seffka/ACE2017` no GitHub) compara Chordino, madmom, Essentia side-by-side.

---

### CREMA (bmcfee)

```bash
python -m crema.analyze -o song.jams song.mp3
```

- Modelo structured prediction (McFee & Bello) + **tracking de baixo/inversão**
- 602 classes efectivas; `N` = no-chord, `X` = out-of-vocabulary (power chords)
- Output **JAMS** → interoperável com mir_eval

---

### Essentia.js (browser)

| Algoritmo | Output |
|-----------|--------|
| `KeyExtractor` | key + scale (major/minor) |
| `ChordsDetection` | acordes em janela HPCP |
| `ChordsDetectionBeats` | acordes por segmento rítmico |

Usado em produção (ex.: TagMyBeat) com Web Workers — **key/chords client-side** sem upload.

---

## Extração de acordes a partir de dados simbólicos

Quando há **MIDI ou MusicXML**, inferir acordes é mais fiável que áudio:

### music21

```python
from music21 import converter, harmony, chord

score = converter.parse('lead.mxl')
# Acordes explícitos no MusicXML:
for el in score.recurse().getElementsByClass(harmony.ChordSymbol):
    print(el.figure, el.offset)

# Inferir de simultaneidades:
chfy = score.chordify()
for c in chfy.recurse().getElementsByClass(chord.Chord):
    print(harmony.chordSymbolFigureFromChord(c))
```

**Limitação:** `chordify()` assume **clássico tonal**; jazz/pop com voicings omitidos gera símbolos imprecisos.

### POP909-CL / BACHI (2025)

- **909 pop songs** com track de acordes **human-reviewed** em MIDI
- Alinhamento beat 24-grid (compatível POP909)
- Paper: boundary-aware symbolic chord recognition

**Uso:** treinar/eval modelos simbólicos; ground truth para comparação com Essentia/CREMA.

---

## Bases de conhecimento — Progressões de acordes

### Chordonomicon (666k+ songs)

- **Hugging Face:** [ailsntua/Chordonomicon](https://huggingface.co/datasets/ailsntua/Chordonomicon)
- Progressões simbólicas + género, sub-género, data, **Spotify ID**
- Estrutura de secções (verso, refrão…)
- Representação grafo para GNN
- Scripts: transposição, chord→notas, binary 12-semitone

**Paper:** arXiv:2410.22046 (Out 2024)

**Relevância:** **lookup** "qual progressão típica em pop 2020" ou join Spotify ID → cifra — **não** áudio-to-chord.

---

## Cifra vs acorde detectado vs acorde tocado

| Tipo | Fonte | Exemplo |
|------|-------|---------|
| **Cifra / leadsheet** | Editor humano, Chordonomicon | `Am7 G C F` |
| **Instructed** | GuitarSet leadsheet_chords | Acordes escritos para session |
| **Performed** | Inferido das notas tocadas | Voicing real pode ser `Am7(no5)` |
| **Detected (áudio)** | CREMA/Essentia | Root often correct, quality debatable |

**Pedagogia:** comparar aluno vs **instructed**; análise de estilo vs **performed**.

---

## Detecção de tom (key)

| Método | Input | Notas |
|--------|-------|-------|
| Essentia KeyExtractor | áudio | Krumhansl-Kessler profiles |
| librosa `feature.chroma_cqt` + template | áudio | Prototipagem |
| music21 `analyze('key')` | score | Simbólico, preciso se partitura completa |

Modulações: key local requer janela deslizante + suavização HMM.

---

## Ranqueamento — Datasets de acordes

| Rank | Dataset | Tamanho | Tipo | Licença |
|------|---------|---------|------|---------|
| 1 | **Chordonomicon** | 666k progressões | Símbolo + metadata | Aberto (HF) |
| 2 | **McGill Billboard** | ~890 | Áudio + acordes | Pesquisa |
| 3 | **Isophonics** | Beatles, Queen, etc. | Áudio + lab | Pesquisa |
| 4 | **POP909-CL** | 909 | MIDI + acordes expert | Pesquisa |
| 5 | **RWC-Pop** | 100 | Áudio + acordes | Pesquisa |
| 6 | **GuitarSet** | 360 exc. | Chords instructed+inferred | Zenodo |

---

## Recomendações music-tutor

| Cenário | Abordagem |
|---------|-----------|
| "Qual tom?" em upload MP3 | Essentia.js KeyExtractor |
| "Quais acordes?" playback | Essentia ChordsDetectionBeats ou CREMA (server) |
| Lição com cifra fixa | MusicXML `<harmony>` ou Chordonomicon lookup |
| Validar acorde tocado (guitarra) | Chroma do stem guitar (Demucs 6s) + template |
| Treinar modelo custom | POP909-CL + pitch augmentation + ChordFormer architecture |

Próximo: [04 — Instrumentos e Stems](./04-instrumentos-source-separation.md)
