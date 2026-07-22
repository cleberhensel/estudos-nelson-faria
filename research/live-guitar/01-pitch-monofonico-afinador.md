# 01 — Pitch Monofónico e Afinador

> Modo: **1 corda**, dedilhado, ou arpejo nota-a-nota dentro do acorde.

---

## Técnicas ranqueadas (tempo real, browser)

| Rank | Técnica / lib | Algoritmo | Latência/frame | Precisão violão | Licença | WASM/JS |
|------|---------------|-----------|----------------|-----------------|---------|---------|
| **1** | **pitchy** | McLeod Pitch Method | ~1–3 ms @2048 | Boa monofónico | **0BSD** | JS puro |
| **2** | **badlogic/tuner** | YIN + parabolic | ~1–5 ms | Muito boa | OSS | TS + Worklet |
| **3** | **aubiojs** | YIN / default | ~2–5 ms | Boa | MIT | WASM |
| **4** | **CREPE tiny/small** | CNN f0 | ~15–40 ms | **>90% @10¢** | Apache | TF.js |
| **5** | **@playground-sessions/pitch-detection-analysis** | CREPE + autocorr fallback | 20–50 ms | Alta + poly opcional | MIT | TF.js |
| **6** | Autocorrelação / AMDF | Clássico | <2 ms | Média (harmónicos) | — | JS |
| **7** | HPS (Harmonic Product Spectrum | FFT | 3–8 ms | Boa timbres ricos | — | JS |

---

## Análise profunda — Top 3

### 1. Pitchy (McLeod Pitch Method)

- **Paper:** [A Smarter Way to Find Pitch](http://www.cs.otago.ac.nz/tartini/papers/A_Smarter_Way_to_Find_Pitch.pdf) — McLeod & Wyvill, 2005
- **Repo:** [github.com/ianprime0509/pitchy](https://github.com/ianprime0509/pitchy) · npm `pitchy@4.1.0`
- **API:** `PitchDetector.forFloat32Array()` → `findPitch(buffer, sampleRate)` → `[Hz, clarity]`
- **Por que #1 MVP:** zero deps pesadas, roda **dentro do AudioWorklet**, clarity 0–1 como gate natural
- **Violão:** range 82 Hz (Mi grave) – 1319 Hz (Mi agudo traste 12) — configurar `minHz`/`maxHz` no wrapper

```javascript
// Padrão AudioWorklet (adaptado de guitar-tuner, badlogic/tuner)
import { PitchDetector } from 'pitchy';

const detector = PitchDetector.forFloat32Array(2048);
const [frequency, clarity] = detector.findPitch(samples, sampleRate);
if (clarity > 0.85) {
  const cents = 1200 * Math.log2(frequency / expectedHz);
}
```

**Referências OSS:** [Chris-Zbrojkiewicz/guitar-tuner](https://github.com/Chris-Zbrojkiewicz/guitar-tuner) (Pitchy + Worklet + React)

---

### 2. YIN (badlogic/tuner)

- **Paper:** Cheveigné & Kawahara, 2002 — estimador f0 por diferença de função
- **Repo:** [github.com/badlogic/tuner](https://github.com/badlogic/tuner)
- **Claim:** ~1–5 ms para 2048 samples; parabolic interpolation sub-sample
- **Extra:** note-aware smoothing — estabiliza UI sem perder mudança de nota
- **Quando preferir a Pitchy:** violões com **harmónicos fortes** onde McLeod oscila; validar A/B no dispositivo alvo

---

### 3. CREPE (TensorFlow.js)

- **Paper:** Kim et al., ICASSP 2018 — CNN waveform → 360 bins cents
- **Browser:** `@playground-sessions/pitch-detection-analysis` encapsula CREPE + resample 16 kHz + Viterbi opcional
- **Trade-off:** 10–20× mais CPU que Pitchy; usar quando **precisão cents** > latência (lições avançadas de afinação)
- **Model sizes:** tiny → full; MVP: **small** ou **tiny** no Worker (não Worklet — model load)

| Modelo CREPE | Params | Uso |
|--------------|--------|-----|
| tiny | ~1.9M | Mobile preview |
| small | ~4.5M | **MVP balanceado** |
| medium+ | >20M | Evitar browser mobile |

---

## Parâmetros calibrados — violão acústico

| Parâmetro | Valor sugerido | Notas |
|-----------|----------------|-------|
| `sampleRate` | 44100 | Padrão Web Audio |
| `bufferSize` | 2048–4096 | 46–93 ms @44.1k — compromisso latência/estabilidade |
| `hopSize` | = bufferSize (Worklet) | 1× por callback |
| `clarity` / confidence min | 0.75–0.85 | Abaixo: “não ouvi nota clara” |
| `tolerance_cents` iniciante | ±20–25 | Pedagógico |
| `tolerance_cents` intermediário | ±10–15 | |
| high-pass | ~80 Hz | Remove rumble |
| `expectedHz` range | 80–1400 Hz | Standard tuning + trastes altos |

---

## Comparar pitch detectado vs lição (Modo nota)

```typescript
interface NoteTarget {
  string: 1 | 2 | 3 | 4 | 5 | 6;
  fret: number;
  midi: number;
  toleranceCents: number;
}

function evaluateMonophonic(detectedHz: number, clarity: number, target: NoteTarget) {
  if (clarity < 0.8) return { status: 'unclear' };
  const expectedHz = 440 * Math.pow(2, (target.midi - 69) / 12);
  const cents = 1200 * Math.log2(detectedHz / expectedHz);
  if (Math.abs(cents) <= target.toleranceCents) return { status: 'ok', cents };
  return { status: cents < 0 ? 'flat' : 'sharp', cents };
}
```

**Validar corda:** só fiável se **uma nota soa** (Modo 1). Com outras cordas abertas a vibrar, gate por **harmónicos esperados da corda alvo** ou pedir **palm mute** nas outras.

---

## Playground Sessions — polifonia opcional no Modo nota

`@playground-sessions/pitch-detection-analysis` (Out 2025, MIT):

- `maxPolyphony: 1–6`, `useCrepe: true`, `useNMF: true`, `useWorklet: true`
- **Uso no tutor:** arpejo “toque corda 5, depois 4…” — NMF separa parcialmente
- **Custo:** TF.js ~500 KB + modelo; reservar para Modo 2 se pitch-set simples falhar

---

## O que evitar no Modo nota

| Tecnologia | Por quê não |
|------------|-------------|
| Basic Pitch contínuo | Overkill; latência 100–500 ms |
| Essentia pitch | Não é foco; onset sim |
| FFT peak sem gate | Confunde harmónicos de acordes adjacentes |

---

## Stack recomendado — Modo nota

```
AudioWorklet → pitchy.findPitch → clarity gate → cents vs target.midi → UI agulha
Fallback Worker: CREPE small se clarity baixa persistente
```

Próximo: [02 — Acordes ao vivo](./02-acordes-validacao-tempo-real.md)
