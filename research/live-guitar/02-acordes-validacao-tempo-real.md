# 02 — Validação de Acordes em Tempo Real

> Modo: **batida** ou **dedilhado** — comparar som do microfone com **voicing esperado** da lição.
>
> Distinto de “detectar qual acorde é esta música” (Chordify, CREMA).

---

## Duas famílias de técnica

| Família | Ideia | Melhor para | Latência |
|---------|-------|-------------|----------|
| **A — Pitch-set diff** | Extrair MIDI pitches pós-strum → comparar com set esperado | **Validação pedagógica** | 50–300 ms |
| **B — Chroma + template** | NNLS/PCP 12-dim → cosine vs template `Am` | Identificar símbolo + smoothing | 30–100 ms/frame |
| **C — ML chord classifier** | CNN/Transformer → label `Am7` | Jazz, voicings ambíguos | 100–500 ms |

**MVP music-tutor:** **A** como core, **B** como fallback quando A não extrai notas suficientes.

---

## Ranqueamento — Técnicas (1 violão, 1 mic)

| Rank | Técnica | Feedback “nota errada” | Feedback “nota faltando” | Implementação |
|------|---------|------------------------|--------------------------|---------------|
| **1** | **Pitch-set diff** vs referência | ✅ extra pitch class | ✅ missing MIDI | Custom + FFT/Basic Pitch |
| **2** | **Playground CREPE+NMF** poly | ✅ até 4–6 pitches | ✅ por MIDI | npm MIT |
| **3** | **@spotify/basic-pitch** janela 400ms | ✅ note events | ✅ | Worker TF.js |
| **4** | **audiojs/pitch-detection** NNLS+templates | ⚠️ só símbolo | ❌ | npm, JS puro |
| **5** | **Essentia ChordsDetection** | ⚠️ label only | ❌ | WASM, mix-oriented |
| **6** | **Adam Stark / chord_detector** | ⚠️ símbolo | ❌ | Rust→WASM possível |
| **7** | **Solitito ONNX** | ⚠️ label jazz | ❌ | Rust, CPU |
| **8** | **CREMA / Chordino** | ❌ mix/gravação | ❌ | Server |

---

## Técnica #1 — Pitch-set diff (recomendada)

### Pipeline

```
Onset (strum) detectado
  → capturar janela [t, t+400ms]
  → extrair pitches {midi, confidence}[]
  → normalizar pitch classes + oitavas relevantes
  → diff vs voicing esperado
```

### Algoritmo de comparação

```typescript
interface VoicingNote {
  midi: number;
  string: number;
  optional: boolean;  // nota omitível (ex. quinta)
  muted?: boolean;    // corda deve estar muda
}

function diffVoicing(detected: number[], expected: VoicingNote[], toleranceCents = 35) {
  const toPc = (m: number) => m % 12;
  const detPcs = new Set(detected.map(toPc));
  const missing = expected
    .filter(n => !n.muted && !n.optional)
    .filter(n => !detPcs.has(toPc(n.midi)));
  const expectedPcs = new Set(expected.filter(n => !n.muted).map(n => toPc(n.midi)));
  const extra = [...detPcs].filter(pc => !expectedPcs.has(pc));
  return {
    match: missing.length === 0 && extra.length === 0,
    missing,  // → "falta o Si"
    extra,    // → "nota a mais" / corda não abafada
  };
}
```

### Extração de pitches na janela

| Método | Prós | Contras |
|--------|------|---------|
| **Peak picking FFT/CQT** | Rápido (~5 ms), sem ML | Harmónicos falsos |
| **HPS** (Harmonic Product Spectrum) | Robusto violão | 2+ notas próximas confunde |
| **Basic Pitch** 400 ms | Melhor recall notas | 100–300 ms inferência |
| **CREPE+NMF poly** | Até 6 vozes | TF.js pesado |

**Recomendação:** FFT/HPS **primeiro**; se `detected.length < expected.length - 1` → retry com Basic Pitch no Worker.

---

## Técnica #2 — NNLS Chroma (Mauch & Dixon, ISMIR 2010)

- **Paper:** Approximate Note Transcription for Improved Chord ID
- **Lib browser:** [audiojs/pitch-detection](https://github.com/audiojs/pitch-detection) — `chroma.nnls()`, `chord.match()`, `chord.smooth()` Viterbi
- **Fluxo Fujishima 1999:** PCP 12-bin → cosine vs 24 templates (major/minor triads)

**Limitação pedagógica:** diz **“soa Am”**, não **“falta E na corda 4”**. Usar só para:
- confirmar símbolo quando pitch-set ambíguo
- modo “livre” (aluno toca, app adivinha acorde)

**Essentia.js `ChordsDetection`:** mesmo paradigma template HPCP — calibrado para **mix**, não lição 1-instrumento. Latência OK, **precisão voicing** inferior a pitch-set com referência.

---

## Técnica #3 — Basic Pitch em janela pós-strum

```typescript
// Worker — não bloquear UI
import { BasicPitch } from '@spotify/basic-pitch';

const notes = await basicPitch.evaluateModel(audioBuffer400ms, ...);
// notes → { startTime, pitchMidi, amplitude }
const active = notes.filter(n => n.amplitude > threshold);
diffVoicing(active.map(n => n.pitchMidi), lesson.voicing);
```

- **Licença:** Apache 2.0
- **Quando:** acordes com 4–6 notas, dedilhados rápidos, FFT falha
- **Thresholds default:** onset 0.25, frame 0.25 (ajustar ↑ para menos falsos positivos)

---

## Técnica #4 — Solitito (referência OSS guitar-specific)

- **Repo:** [github.com/greblus/solitito](https://github.com/greblus/solitito)
- **Model:** ONNX, CQT 144 + chroma 12 = 156 dims, 16 kHz, context 32 frames (~0,5 s)
- **Classes:** root + quality (maj, min, 7, m7, …) + “Note” monofónico
- **Treino:** GuitarSet entre outros

**Fit:** validar **símbolo** de acorde jazz; **não** substitui diff voicing para “dedo errado”. Portar para WASM interessante em **Fase 3**.

---

## Produtos — o que fazem (técnica inferida)

| Produto | Abordagem provável | Evidência |
|---------|-------------------|-----------|
| **Yousician** | frequency + salience + timing vs exercise data | [US9218748B2](https://patents.google.com/patent/US9218748B2) |
| **Uberchord** | chord detection + fretboard UI | App store / reviews |
| **Simply Guitar** | Similar Yousician (JoyTunes) | Mesmo segmento |
| **Rocksmith** | pitch + possível divisão espectral (patente Ubisoft) | US9839852 (disputada) |

Patente Yousician (claims relevantes):
- Parâmetros: **pitch, salience, timing, volume, articulation**
- Comparação **item-a-item**: nota, intervalo, **acorde**
- Feedback **online durante** o exercício
- ML **self-learning** on user recordings (produto maduro)

---

## Matriz — Tipo de feedback × Técnica

| Feedback desejado | Pitch-set | NNLS/chroma | Basic Pitch | Solitito ML |
|-------------------|-----------|-------------|-------------|-------------|
| Acorde certo/errado | ✅ | ✅ | ✅ | ✅ |
| Nota faltando | ✅ | ❌ | ✅ | ❌ |
| Nota extra (corda aberta) | ✅ | ❌ | ✅ | ❌ |
| Posição dedo errada mesma altura | ❌ | ❌ | ❌ | ❌ |
| Voicing alternativo válido | ⚠️ whitelist | ⚠️ | ⚠️ | ⚠️ |

---

## Calibração — batida de violão

1. **Onset** dispara captura (ver cap. 03)
2. Ignorar **200 ms** de transient caótico → janela análise `[200, 500] ms` pós-onset
3. **RMS gate:** strum fraco → “toque mais forte”
4. **Sustain:** re-analisar até 1,5 s se exercício pede “deixar soar”

---

## Stack recomendado — Modo acorde

```
Worklet: onset + RMS
  → postMessage({ type: 'strum', buffer: Float32Array(400ms) })
Worker: HPS peaks → diffVoicing
  → se inconclusivo: BasicPitch.evaluateModel
  → postMessage({ missing, extra, match })
Main: render diagrama GP + highlight corda errada
```

Próximo: [03 — Ritmo e onset](./03-ritmo-onset-metronomo.md)
