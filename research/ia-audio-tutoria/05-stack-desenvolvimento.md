# 05 — Stack de Desenvolvimento

## Camadas de um app musical moderno

```
┌─────────────────────────────────────────────────────────┐
│  UI (React, SwiftUI, etc.)                              │
├─────────────────────────────────────────────────────────┤
│  Áudio I/O — Web Audio API / AVAudioEngine / JUCE       │
├─────────────────────────────────────────────────────────┤
│  Síntese & scheduling — Tone.js / SuperCollider / etc.  │
├─────────────────────────────────────────────────────────┤
│  MIR client — Essentia.js / CREPE-TF.js / Pitchy        │
├─────────────────────────────────────────────────────────┤
│  MIDI — Web MIDI API / @tonejs/midi / python-rtmidi     │
├─────────────────────────────────────────────────────────┤
│  ML inference — TF.js / ONNX Runtime Web / WASM         │
├─────────────────────────────────────────────────────────┤
│  Backend — FastAPI / Rust Axum / Node                     │
├─────────────────────────────────────────────────────────┤
│  GPU workers — PyTorch models, filas Celery/Bull        │
├─────────────────────────────────────────────────────────┤
│  LLM / ALM APIs — OpenAI, Gemini, self-hosted HF         │
└─────────────────────────────────────────────────────────┘
```

---

## Web Audio API (fundamento browser)

- **AudioContext:** relógio de amostras de alta precisão
- **Nodes:** Gain, BiquadFilter, Analyser, Convolver, AudioWorklet
- **Constraint:** `AudioContext` requer **gesto do usuário** para iniciar (click/tap)
- **Mobile:** Safari limitado; Chrome recomendado para apps de tutoria

### AudioWorklet vs ScriptProcessor

| | AudioWorklet | ScriptProcessor (deprecated) |
|--|--------------|------------------------------|
| Thread | Audio thread | Main thread |
| Latência | Baixa | Alta |
| Uso | Pitch em tempo real | Evitar |

---

## Tone.js

Framework de alto nível sobre Web Audio — **padrão de facto** para apps web musicais.

**Features:**

- Transport global (BPM, compasso, sincronização)
- Sintetizadores (Synth, FM, AM), efeitos, samples
- Scheduling musical: `"4n"`, `"8t"`, `@+1m`
- Integração com [@tonejs/midi](https://tonejs.github.io/Midi/)

```javascript
import * as Tone from "tone";

await Tone.start(); // após click do usuário
const synth = new Tone.Synth().toDestination();
Tone.Transport.bpm.value = 90;
synth.triggerAttackRelease("C4", "4n", Tone.now() + 0.5);
```

**Stats:** ~14,7k stars; MIT license; TypeScript.

**Limitação:** MIDI files precisam conversão para JSON via `@tonejs/midi`.

---

## Essentia.js

- Port WASM do Essentia (UPF Barcelona)
- Key detection, BPM, spectrum, HPCP **no browser**
- Padrão: Web Worker para não bloquear UI
- Usado em produção com Next.js + Cloudflare (TagMyBeat, 2025)

---

## Inferência ML no browser

### TensorFlow.js

- Backend: WebGL (GPU), WASM, WebGPU (emergente)
- CREPE, Magenta, modelos custom
- `@tensorflow/tfjs` ~4.22+

### ONNX Runtime Web

- Modelos PyTorch exportados → ONNX
- Demucs-onnx, modelos custom
- Backends: WASM, WebGPU
- **Recomendado** para extensões e apps sem backend ML

### Transformers.js (@xenova/transformers)

- Modelos Hugging Face no browser
- Whisper tiny para STT leve; modelos maiores pesados

### Comparativo TF.js vs ONNX Web

| Critério | TF.js | ONNX Runtime Web |
|----------|-------|------------------|
| Ecossistema Magenta | Nativo | Export manual |
| Modelos HF | Via Transformers.js | Via ONNX export |
| Performance | Boa (WebGL) | Boa (WASM/WebGPU) |
| Tamanho bundle | Médio-grande | Médio |

---

## MIDI — protocolo e bibliotecas

### Web MIDI API

- Acesso a controladores USB no Chrome/Edge
- **Safari:** suporte limitado/inexistente
- Útil para tutores de piano/teclado (latência mínima vs microfone)

### Bibliotecas

| Lib | Plataforma | Uso |
|-----|------------|-----|
| @tonejs/midi | JS | Parse/serialize MIDI ↔ Tone.js |
| webmidi | JS | Abstração Web MIDI API |
| pretty_midi / mido | Python | Manipulação server-side |
| music21 | Python | Análise teórica, MusicXML |

---

## Backend e workers

### Padrão SonicAI (referência acadêmica 2025)

- **FastAPI** + **Celery** + **Redis** + **PostgreSQL**
- MusicGen lazy-loaded em GPU
- Fallback **DSP sintético** (osciladores sine/saw/square) sem GPU
- Frontend: React 19 + Zustand + WaveSurfer.js

### Padrão CrescendAI (referência avançada 2026)

- **Rust Axum** on Cloudflare Workers
- HF Inference Endpoint para modelos custom
- WebSocket para observações em tempo real
- D1/R2/KV/Durable Objects
- Groq (rápido) + Claude (pedagogia)

### Quando usar GPU server-side

- Demucs, MusicGen medium+, ALMs 7B+
- Batch transcription de lições
- Fine-tune LoRA (Stable Audio 3)

---

## Visualização de áudio

| Lib | Uso |
|-----|-----|
| WaveSurfer.js | Waveform + regions + sync com playback |
| p5.js / canvas | Pitch roll, piano roll custom |
| VexFlow / OpenSheetMusicDisplay | Partitura interativa |
| abcjs | Notação ABC no browser |

---

## Mobile nativo

| Plataforma | Stack comum |
|------------|-------------|
| iOS | AVAudioEngine, AudioKit, Core ML |
| Android | Oboe, TFLite, MediaPipe |
| Cross | Flutter + dart:ffi para DSP; React Native (limitado para áudio RT) |

Yousician-like apps tipicamente **nativos** no loop de áudio; web para MVP/prototipo.

---

## Observabilidade e qualidade de LLM

Para pipelines pedagógicos (Melody Sage, CrescendAI):

- **RAG:** Vertex AI Vector Search, Pinecone, pgvector
- **Eval datasets:** prompts de teoria musical + rubricas
- **Platforms:** Maxim AI, LangSmith — regressão de prompts

---

## Docker e deploy

```yaml
# Padrão mínimo com worker GPU
services:
  api:
    image: music-tutor-api
    ports: ["8000:8000"]
  worker:
    image: music-tutor-worker
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]
  redis:
    image: redis:7
```

**Edge vs cloud:**

- Pitch/MIR leve → edge (browser)
- Geração/ALM → cloud GPU
- Híbrido reduz custo e melhora privacidade

---

## Dependências open source críticas (checklist)

| Pacote | Licença | Comercial OK? |
|--------|---------|---------------|
| Tone.js | MIT | Sim |
| Essentia | AGPL / Apache* | Verificar build |
| Basic Pitch | Apache 2.0 | Sim |
| CREPE | MIT | Sim |
| Demucs | MIT | Sim |
| MusicGen weights | CC-BY-NC 4.0 | **Não comercial** |
| Stable Audio 3 Small/Med | Stability license | Verificar terms |
| librosa | ISC | Sim |

*Essentia.js builds podem variar — confirmar licença do wrapper.

---

## Recursos de aprendizado

- [Web Audio API MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [Tone.js docs](https://tonejs.github.io/docs/)
- [Essentia tutorials](https://essentia.upf.edu/tutorial.html)
- [AudioCraft API](https://facebookresearch.github.io/audiocraft/api_docs/audiocraft/index.html)
- [ONNX Runtime Web](https://onnxruntime.ai/docs/tutorials/web/)
