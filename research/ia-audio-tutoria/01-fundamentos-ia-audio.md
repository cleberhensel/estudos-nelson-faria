# 01 — Fundamentos de IA para Áudio

## Por que áudio é diferente de texto

Música e áudio são **sinais contínuos de alta taxa de amostragem** (44,1 kHz = 44.100 amostras/segundo). Um minuto de áudio estéreo contém milhões de pontos de dados. LLMs de texto operam em tokens discretos (~4 caracteres); modelos de áudio precisam **comprimir** o sinal antes de gerar ou entender.

### Representações comuns

| Representação | O que é | Uso típico |
|---------------|---------|------------|
| **Waveform** | Amplitude no tempo | Pitch tracking (CREPE), separação de fontes |
| **Espectrograma / Mel** | Frequência × tempo | MIR clássico, Demucs, muitos classificadores |
| **Tokens de codec neural** | Latentes discretos (EnCodec, DAC) | MusicGen, modelos autoregressivos |
| **Latentes contínuos (SAME, VAE)** | Espaço compacto semântico-acústico | Stable Audio 3, edição/inpainting |
| **Simbólico (MIDI, ABC, MusicXML)** | Notas, duração, instrumento | Tutoria, partitura, controle fino |

---

## Três paradigmas generativos dominantes

### 1. Autoregressivo (AR)

Gera áudio **token a token**, como um LLM gera texto.

- **Exemplos:** MusicGen, Jukebox (legado), partes de Suno/Udio (não totalmente público)
- **Prós:** Controle temporal forte; condicionamento por melodia (MusicGen-melody)
- **Contras:** Erros acumulam ao longo do tempo; inferência sequencial (mais lenta)
- **Detalhe técnico (MusicGen):** Transformer sobre EnCodec 32 kHz, 4 codebooks a 50 Hz → ~50 passos AR por segundo de áudio, com delay entre codebooks para predição paralela

### 2. Difusão / Flow Matching

Parte de ruído e **refina iterativamente** até áudio coerente.

- **Exemplos:** Stable Audio 1–3, MusicLDM, ETTA
- **Prós:** Alta fidelidade; inpainting e duração variável nativos
- **Contras:** Múltiplos passos de inferência (mitigado com destilação adversarial)
- **Stable Audio 3:** flow matching + DiT (Diffusion Transformer) + autoencoder SAME

### 3. Híbridos

Combinam AR para estrutura + difusão/flow para qualidade local.

- **Exemplos:** DiTAR (patches AR + DiT intra-patch), cascatas Noise2Music
- **Tendência 2025–2026:** flow matching ganha em **eficiência** sem perder qualidade vs difusão clássica

### Estudo comparativo (AR vs Flow Matching)

Paper *Auto-Regressive vs Flow-Matching: a Comparative Study of Modeling Paradigms for Text-to-Music Generation* (2025) fixou dados, latente (EnCodec), backbone e avaliou:

| Eixo | Tendência observada |
|------|---------------------|
| Qualidade perceptual | Competitivo entre paradigmas |
| Aderência ao prompt | Depende mais de dados/condicionamento |
| Inpainting | Flow matching / difusão tende a ser mais flexível |
| Eficiência de inferência | Flow matching geralmente mais rápido |
| Robustez a hiperparâmetros | Flow matching mais estável |

**Conclusão prática:** para produto, escolha por **latência, licença e API** — não só por arquitetura.

---

## Pipeline típico de um gerador moderno

```
Texto / MIDI / Áudio de referência
        ↓
   Encoder de condicionamento (T5, CLAP, etc.)
        ↓
   Modelo generativo (AR / Flow / DiT)
        ↓
   Latente ou tokens de codec
        ↓
   Decoder neural (SAME, EnCodec, vocoder)
        ↓
   Waveform 44,1 kHz (ou 48 kHz)
```

---

## Latência e arquitetura de produto

| Cenário | Latência típica | Onde roda |
|---------|-----------------|-----------|
| Pitch feedback (tutor) | < 50 ms | Browser (AudioWorklet) |
| Análise de acordes (offline) | 1–10 s / faixa | Browser WASM ou servidor |
| Geração MusicGen 8 s | 5–30 s | GPU servidor |
| Suno faixa 2 min | ~20–40 s | API assíncrona (poll/webhook) |
| Stable Audio 3 Medium 6 min | ~1–2 s (H200) | GPU local ou API |
| Conversa GPT-4o voz | ~230–320 ms | API streaming |

**Regra de ouro para tutoria:** separar **loop de tempo real** (pitch, metrônomo, UI) de **loop assíncrono** (LLM, geração, transcrição completa).

---

## Tokenizadores de áudio relevantes

| Tokenizador | Org | Papel |
|-------------|-----|-------|
| **EnCodec** | Meta | Base do MusicGen; 32 kHz, codebooks múltiplos |
| **SAME** | Stability AI | Semantic-Acoustic Music Encoder; stereo 44,1 kHz, 256-d latents |
| **DAC** | Descript | Alternativa open para compressão |
| **Whisper encoder** | OpenAI | Usado em Music Flamingo como encoder de áudio |

---

## Glossário rápido

- **MIR** — Music Information Retrieval: extrair informação de áudio (tom, acordes, BPM)
- **AMT** — Automatic Music Transcription: áudio → notas/MIDI
- **ALM / LALM** — (Large) Audio-Language Model: áudio + texto → texto
- **Inpainting** — regenerar segmento de áudio mantendo contexto
- **Stem** — faixa isolada (vocals, drums, bass, etc.)
- **HPCP / Chromagram** — representação de classes de altura (12 notas) ao longo do tempo
