# 08 — Matriz de Decisão e Roadmap

## Árvore de decisão: qual tecnologia usar?

```
Preciso de...
│
├─ Feedback pitch em tempo real (< 50 ms)?
│   └─ Browser: Pitchy ou CREPE-TF.js + AudioWorklet
│   └─ Nativo: AudioKit / custom DSP
│   └─ Piano: Web MIDI API (melhor que mic)
│
├─ Explicar teoria / adaptar lição?
│   └─ GPT-4o / Gemini + RAG (pgvector)
│   └─ Offline: Gemma/Llama local (qualidade menor)
│
├─ Analisar gravação completa (harmonia, forma)?
│   └─ Music Flamingo ou MOSS-Music (self-host GPU)
│   └─ Alternativa cloud: Gemini com áudio (menos profundo)
│
├─ Transcrever para MIDI?
│   └─ Basic Pitch (Apache 2.0, mono/poly 1 inst)
│
├─ Separar stems?
│   └─ Demucs htdemucs (server GPU) ou demucs-onnx (edge)
│
├─ Gerar backing instrumental?
│   └─ Comercial seguro: Stable Audio 3 / Lyria
│   └─ Protótipo: MusicGen (NC — não shippar comercial)
│   └─ Loops: Mubert API
│
├─ Gerar música com vocais?
│   └─ Suno/Udio API (legal em fluxo) ou Lyria (enterprise)
│
├─ Gerar exercício em MIDI editável?
│   └─ MIDI-LLM / Magenta / regras + music21
│
└─ Conversa por voz com tutor?
    └─ GPT-4o Realtime / Gemini Live API
```

---

## Matriz por requisito não-funcional

| Requisito | Opção A | Opção B | Opção C |
|-----------|---------|---------|---------|
| **Privacidade máxima** | Essentia.js + Pitchy local | Gemma local | Sem gravação na cloud |
| **Menor custo operacional** | Browser-first MIR | MiniMax via FAL | MusicGen self-host NC |
| **Melhor qualidade vocal** | Suno v5.5 | Udio | ElevenLabs v2 |
| **Compliance enterprise** | Lyria + Vertex | Stable Audio licensed | ElevenLabs enterprise |
| **Time-to-MVP 4 semanas** | React + Tone + Pitchy | + GPT-4o texto | Sem geração |
| **Offline** | PWA + WASM models | ❌ ALMs 7B+ impraticável mobile | MIDI-only mode |

---

## Stack sugerida para MVP music-tutor

### Escopo MVP

- **1 instrumento:** piano (MIDI) ou monofónico (voz/violino)
- **Feedback:** pitch + timing vs referência
- **Pedagogia:** LLM texto com RAG de teoria
- **Sem geração** de música completa (fase 2)

### Stack concreta

| Camada | Escolha | Alternativa |
|--------|---------|-------------|
| Frontend | React + TypeScript | Vue/Svelte |
| Áudio | Tone.js + Web Audio | — |
| Pitch RT | Pitchy (MVP) → CREPE-TF (v1.1) | — |
| MIDI | @tonejs/midi + Web MIDI API | — |
| Partitura | OpenSheetMusicDisplay | abcjs |
| Backend | FastAPI (Python) | Hono/Node |
| LLM | OpenAI GPT-4o mini (custo) | Gemini Flash |
| RAG | pgvector + chunks teoria | Vertex AI |
| Auth | Clerk / Auth.js | — |
| Hosting | Vercel + Railway/Fly GPU | Cloudflare (sem GPU nativo) |

### Estimativa de esforço

| Epic | Semanas | Dependência |
|------|---------|-------------|
| Audio engine + pitch | 2 | — |
| UI exercício + score | 2 | Audio |
| LLM tutor + RAG | 1,5 | Conteúdo curado |
| Auth + progress | 1 | — |
| Polish + mobile PWA | 1,5 | — |
| **Total MVP** | **~8 semanas** | 1 dev full-stack |

---

## Roadmap em fases

### Fase 0 — Discovery ✅ (este documento)

- [x] Mapa de modelos LLM e áudio
- [x] Stack e arquitetura de referência
- [ ] Validar instrumento alvo com usuários
- [ ] Protótipo pitch-only (1 semana spike)

### Fase 1 — MVP Tutor Core

- Pitch feedback tempo real
- 10 lições estáticas + LLM Q&A
- Progress tracking
- Metronome + referência Tone.js

### Fase 2 — Inteligência musical

- Basic Pitch integration (upload)
- Essentia.js key/chords
- Music Flamingo para análise de gravações
- Exercícios gerados (MIDI-LLM ou template)

### Fase 3 — Geração e produção

- Stable Audio 3 backing tracks
- Demucs para play-along custom
- Voice tutor (GPT-4o Realtime)

### Fase 4 — Plataforma

- Multi-instrumento
- Catálogo licenciado
- Social / teachers marketplace
- Mobile nativo se PMF web confirmado

---

## Gaps e perguntas abertas

| # | Pergunta | Impacto |
|---|----------|---------|
| 1 | Piano MIDI first ou microfone universal? | Define input layer |
| 2 | Público BR — conteúdo em PT nativo? | RAG + prompts |
| 3 | B2C vs B2B (escolas de música)? | Compliance, preço |
| 4 | Offline é must-have? | Descarta ALM cloud |
| 5 | Geração de áudio no MVP ou não? | +4 semanas + legal |
| 6 | Qual tolerância cents por nível? | Pedagogia + UX |

---

## Custos operacionais estimados (MVP 1000 MAU)

| Serviço | Custo/mês (ordem de grandeza) |
|---------|-------------------------------|
| Vercel Pro | $20 |
| Railway/Fly API | $20–50 |
| OpenAI GPT-4o mini (100k msgs) | $50–150 |
| HF Inference (ALM — se usar) | $100–500 GPU |
| **Total sem geração** | **~$100–250** |
| + Suno/Stable Audio (geração) | +$0,05–0,15/user ativo |

---

## KPIs de validação por fase

| Fase | North star | Guardrails |
|------|------------|------------|
| MVP | D7 retention > 25% | Pitch latency P95 < 80 ms |
| Fase 2 | Sessions/week > 3 | ALM cost < $0,10/session |
| Fase 3 | Generated track usage > 30% | Zero copyright strikes |

---

## Referências cruzadas rápidas

| Preciso entender... | Ler |
|---------------------|-----|
| AR vs difusão vs flow | [01](./01-fundamentos-ia-audio.md) |
| Music Flamingo vs GPT-4o | [02](./02-llms-e-audio-language-models.md) |
| Suno vs Stable Audio | [03](./03-geracao-de-audio-e-musica.md) |
| CREPE vs Basic Pitch | [04](./04-analise-musical-mir.md) |
| Tone.js + TF.js | [05](./05-stack-desenvolvimento.md) |
| Risco Suno lawsuit | [06](./06-legal-e-licenciamento.md) |
| Pipeline CrescendAI | [07](./07-arquiteturas-apps-tutoria.md) |

---

## Conclusão

O ecossistema em 2026 permite construir um **tutor musical credível** combinando:

1. **MIR determinístico** no browser (pitch, timing)
2. **LLM** para pedagogia e adaptação
3. **ALM** (Music Flamingo / MOSS) para análise profunda sob demanda
4. **Geração** opcional via Stable Audio / Lyria quando compliance importar

O erro mais comum é tentar **um modelo fazer tudo**. A arquitetura vencedora é **pipeline especializado** com loops de latência bem separados — exatamente como apps profissionais (Yousician) e research state-of-the-art (CrescendAI) demonstram.

**Próxima ação recomendada:** spike de 3 dias — AudioWorklet + Pitchy + UI de nota alvo vs nota tocada — antes de qualquer integração LLM.
