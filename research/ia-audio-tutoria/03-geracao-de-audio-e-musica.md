# 03 — Geração de Áudio e Música

## Panorama do mercado (Maio 2026)

O mercado dividiu-se em **três faixas**:

1. **APIs comerciais full-song** (vocais + letras + produção) — Suno, Udio, Lyria
2. **Ferramentas de produção editável** — ElevenLabs Music v2, Suno Studio
3. **Open source / self-host** — Stable Audio 3, MusicGen, Riffusion

---

## Tier 1 — Líderes comerciais (música completa com vocais)

### Suno v5 / v5.5

| Aspecto | Detalhe |
|---------|---------|
| Força | Vocais realistas, estrutura verso/refrão, até ~8 min |
| Benchmark | ELO ~1293 (referência de mercado em 2025–2026) |
| Sample rate | 44,1 kHz (v5 vs 24 kHz em v3) |
| API | Enterprise + third-party gateways; latência ~22 s para clip 2 min |
| Features | Stem export, custom voice (verified), fine-tune até 3 modelos |
| Legal | Processos RIAA ativos; Warner settled (nov 2025); UMG/Sony litigando |
| Preço API | Premium (~$0,08–0,12/min via gateways) |

**Melhor para:** demos, conteúdo social, protótipos onde qualidade vocal é crítica.

**Evitar para:** produto enterprise sem clareza de licenciamento de training data.

### Udio

| Aspecto | Detalhe |
|---------|---------|
| Força | Fidelidade técnica (48 kHz), inpainting maduro, experimentação criativa |
| Arquitetura | Transformer long-context (coerência intra-faixa) |
| Legal | UMG settled → plataforma **Starstruck** (2026); Merlin, Kobalt, Believe licenciados |
| Sony | Ainda em litígio |
| Stems | Export temporariamente desabilitado em alguns momentos (verificar status) |

**Melhor para:** usuários avançados, edição por seções (via Starstruck).

### Google Lyria 3 Pro

| Aspecto | Detalhe |
|---------|---------|
| Acesso | Vertex AI (preview), Gemini API, AI Studio, Gemini app, Google Vids, ProducerAI |
| Duração max | ~3 min (vs 8 min Suno) |
| Diferencial | **SynthID watermark**, dados licenciados + YouTube licenciado |
| Enterprise | Opção mais defensável legalmente para B2B |
| Fine-tuning | Não (vs Suno sim) |
| Stem export | Não |

**Melhor para:** integração GCP, apps com compliance, geração programática B2B.

---

## Tier 2 — Produção e controle editorial

### ElevenLabs Music v2 (Maio 2026)

- **Paradigma DAW:** edição por seções, inpainting, continuidade estrutural, SFX embutidos
- **Transições:** mudança de gênero mid-track
- **Dados:** treino com licenciamento comercial (desde v1)
- **API:** ElevenAPI "coming soon" no launch; enterprise via sales
- **Comparação:** vocais raw ainda atrás de Suno v5; ganha em **workflow de produção**

### Stable Audio 3 (Stability AI — Maio 2026)

Família unificada com arquitetura **SAME + flow matching + DiT**:

| Modelo | Params | Hardware | Duração max | Open weights |
|--------|--------|----------|-------------|--------------|
| Small-SFX | 433M | **CPU** | 120 s | Sim (HF) |
| Small-Music | 433M | **CPU** | 120 s | Sim (HF) |
| Medium | 1,4B | GPU CUDA | 380 s (~6:20) | Sim (HF) |
| Large | 2,7B | API only | 380 s | Não |

**Inferência:** ~0,44 s (Small) / ~1,31 s (Medium) em H200 para faixa completa.

**Modos:** text-to-audio, audio-to-audio, inpainting/continuação, LoRA fine-tune.

**Licenciamento:** WMG + UMG deals; indemnização enterprise; CC + dados licenciados.

**Repo:** [github.com/Stability-AI/stable-audio-3](https://github.com/Stability-AI/stable-audio-3)

**Melhor para:** self-host, instrumental, SFX, apps que precisam rodar em MacBook (Small).

### Mubert / Soundraw / AIVA

- **Mubert:** loops e streams de fundo (API pública, foco instrumental)
- **Soundraw / AIVA:** composição royalty-free para criadores
- Menor realismo vocal; bom para **background educacional**

---

## Tier 3 — Open source (self-host)

### Meta MusicGen / AudioCraft

| Modelo | Params | GPU mín | Licença weights |
|--------|--------|---------|-----------------|
| musicgen-small | 300M | 8 GB (clip curto) | CC-BY-NC 4.0 |
| musicgen-medium | 1,5B | 16 GB | CC-BY-NC 4.0 |
| musicgen-melody | 1,5B | 16 GB | + condicionamento melódico |
| musicgen-large | 3,3B | 24 GB+ | CC-BY-NC 4.0 |

**Também no AudioCraft:** AudioGen (SFX), EnCodec, MAGNeT (non-AR), JASCO (acordes+melodia+bateria), AudioSeal (watermark).

```python
from audiocraft.models import MusicGen
model = MusicGen.get_pretrained("facebook/musicgen-medium")
model.set_generation_params(duration=8)
wav = model.generate(["upbeat jazz piano in C major"])
```

**Limitação NC:** uso comercial restrito — verificar compliance.

### Riffusion

- Espectrograma → áudio; self-host friendly
- Qualidade inferior a Suno; controle fino sobre elementos espectrais
- Útil para experimentação e pipelines custom

### MiniMax Music (via FAL.ai)

- ~$0,035/geração — **ordem de magnitude mais barato** que subscriptions
- Boa opção para **alto volume API** em protótipos

---

## Matriz comparativa de geração

| Plataforma | Vocais | Instrumental | Inpainting | API | Open | Comercial seguro |
|------------|--------|--------------|------------|-----|------|------------------|
| Suno v5.5 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | Parcial (Studio) | Gateway | Não | Incerto |
| Udio | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Limitada | Não | Melhorando (UMG) |
| Lyria 3 Pro | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | Limitado | Vertex/Gemini | Não | ⭐⭐⭐⭐ |
| ElevenLabs v2 | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Beta | Não | ⭐⭐⭐⭐ |
| Stable Audio 3 | N/A* | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | Sim (Large) | Small/Med | ⭐⭐⭐⭐ |
| MusicGen | N/A | ⭐⭐⭐ | Limitado | Self | Sim | NC only |
| Mubert | N/A | ⭐⭐⭐ (loops) | Não | Sim | Não | ⭐⭐⭐ |

*Stable Audio 3 foco instrumental/SFX; vocais não são o core.

---

## Padrão de integração API (geração assíncrona)

Quase todos os provedores comerciais usam **task-based async**:

```
POST /generate → { task_id }
GET /tasks/{id} → { status: pending|complete, url? }
ou Webhook on complete
```

**Implicações para dev:**

- Fila de jobs (Celery, BullMQ, Cloud Tasks)
- UI com progresso e cancelamento
- Cache de resultados (R2/S3) — URLs expiram
- Retry com idempotency keys

---

## Casos de uso no music-tutor

| Caso | Recomendação |
|------|--------------|
| Backing track para exercício | Stable Audio 3 Small (local) ou Mubert API |
| Demo "como deveria soar" | MusicGen-melody condicionado em MIDI do exercício |
| Exemplo vocal (cantores) | Suno/ElevenLabs — apenas se licenciado |
| SFX de feedback (acerto/erro) | Stable Audio Small-SFX ou AudioGen |
| Harmonização de exercício | MIDI-LLM ou JASCO (AudioCraft) — output MIDI editável |

---

## Evolução histórica (contexto)

| Ano | Marco |
|-----|-------|
| 2023 | MusicLM (Google), AudioLM — hierárquico, research |
| 2023 | MusicGen, AudioCraft open source |
| 2024 | Stable Audio 2.0 (3 min, 44,1 kHz); Suno/Udio launch |
| 2024 | RIAA lawsuits (jun) |
| 2025 | Settlements UMG-Udio, Warner-Suno |
| 2025 | Music Flamingo, AR vs FM study |
| 2026 | Stable Audio 3, ElevenLabs Music v2, MOSS-Music, Lyria 3 Pro GA |

---

## Referências

- [Stable Audio 3 paper](https://arxiv.org/abs/2605.17991)
- [MusicGen docs](https://facebookresearch.github.io/audiocraft/docs/MUSICGEN.html)
- [ElevenLabs Music v2 review](https://www.buildfastwithai.com/blogs/elevenlabs-music-v2-review-2026)
- [Google Lyria 3 Pro overview](https://nerdleveltech.com/google-lyria-3-pro-ai-music-generation)
