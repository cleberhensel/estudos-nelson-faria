# 06 — Legal e Licenciamento

> **Disclaimer:** isto não é aconselhamento jurídico. Use como mapa de riscos para decisões de produto e engenharia.

---

## Por que importa para desenvolvedores

Construir software para músicos envolve **três camadas legais distintas**:

1. **Licença do modelo/tool** (open source, API terms)
2. **Direitos sobre training data** (como o modelo foi treinado)
3. **Direitos sobre output** (quem possui a música gerada)

Confundir essas camadas é a causa #1 de risco em startups de AI music.

---

## Panorama processual (Maio 2026)

### RIAA vs Suno e Udio (desde jun 2024)

| Parte | Suno | Udio |
|-------|------|------|
| Warner Music | **Settled** (nov 2025) + partnership | **Settled** |
| Universal Music | Litigando | **Settled** (out 2025) → Starstruck |
| Sony Music | **Litigando** (+61k obras adicionais, mai 2026) | **Litigando** (+30k obras) |
| Suno defense | Fair use (cita Bartz v. Anthropic) | — |
| Udio admissões | — | Scraping YouTube confirmado em discovery |

### Implicações práticas

- **Suno:** API comercial existe, mas fundação legal ainda contestada; Warner partnership sinaliza caminho licenciado futuro
- **Udio/Starstruck:** modelo migrando para **catálogo licenciado** com revenue share (~$0,002–0,005/geração para UMG)
- **Independentes:** class action alega ~40M tracks no training Suno, 60% de artistas independentes sem compensação

---

## Modelos com posição legal mais defensável

| Provider | Argumento legal |
|----------|-----------------|
| **Stable Audio 3** | WMG + UMG deals; dados licenciados + CC; indemnização enterprise |
| **ElevenLabs Music** | Treino com licenciamento comercial declarado desde v1 |
| **Google Lyria 3** | Dados licenciados + YouTube licenciado; **SynthID** watermark |
| **MusicGen** | Meta-owned + licensed (20k h) — mas weights **CC-BY-NC** |
| **Suno/Udio** | Disputado / em transição para licenciamento |

---

## Tipos de licença de software (dev checklist)

| Licença | Pode usar em produto pago? | Notas |
|---------|---------------------------|-------|
| MIT / Apache 2.0 | Sim | Basic Pitch, Demucs, CREPE, Tone.js |
| CC-BY-NC 4.0 | **Não comercial** | MusicGen weights |
| AGPL | Cuidado | Essentia — pode exigir open source da app |
| API Terms | Ler ToS | Direitos de output variam por tier |
| NVIDIA Noncommercial | **Não comercial** | Alguns checkpoints Audio Flamingo |

**Sempre verificar:** license file no repo + `MODEL_CARD` no Hugging Face.

---

## Direitos sobre output gerado

| Fonte | Tendência (2026) |
|-------|------------------|
| Suno Pro/API | Uso comercial permitido em tiers pagos (verificar ToS vigente) |
| Udio/Starstruck | Revenue share com rights holders em outputs derivados de catálogo |
| Stable Audio API | Terms comerciais + indemnização (enterprise) |
| MusicGen NC | Uso comercial **proibido** |
| US Copyright Office | Obra **100% AI-generated** — registro limitado; exige contribuição humana significativa |

**Para tutor:** exercícios gerados por AI + edição humana do professor provavelmente qualificam como obra derivada com autoria mista — documentar workflow.

---

## Voice cloning e direitos de imagem vocal

- ElevenLabs, Suno verified voices, Udio artist modes
- **ElevenLabs case settled**; leis estaduais US (TN, CA, etc.) expandem voice rights
- **Não clonar voz de artista** sem licença explícita

---

## Dados do usuário (GDPR / LGPD)

Apps de tutoria capturam **gravações de performance** — dados pessoais se identificáveis:

- Consentimento explícito para gravação
- Política de retenção (ex.: 30 dias)
- Opção de processamento **local-only** (browser) como diferencial de privacidade
- DPA com providers de LLM se áudio for enviado à cloud

---

## Watermarking e detecção

| Tecnologia | Provider |
|------------|----------|
| SynthID | Google (Lyria, Gemini ecosystem) |
| AudioSeal | Meta / AudioCraft |
| Audible Magic | Industry fingerprinting (usado em lawsuits) |

Relevante se o tutor **gerar** áudio — algumas DSPs exigem disclosure de conteúdo AI.

---

## Recomendações para o music-tutor

### MVP (baixo risco)

- MIR open source permissivo (Basic Pitch, CREPE, Essentia)
- LLM via API com ToS claros (OpenAI, Google) para **texto** e explicações
- **Não** redistribuir MusicGen NC em produto comercial
- Gravações processadas localmente quando possível

### Fase 2 (geração)

- Stable Audio 3 Small (instrumental/SFX) ou Lyria via Vertex se GCP
- Evitar dependência única de Suno até clareza legal
- Documentar licença de cada track gerada para o usuário

### Fase 3 (vocal / artist content)

- Parcerias licenciadas (UMG-style) ou conteúdo 100% original do usuário
- Content ID interno se plataforma permitir upload

---

## Timeline de referência

| Data | Evento |
|------|--------|
| Jun 2024 | RIAA suits Suno + Udio |
| Set 2025 | Bartz v. Anthropic $1,5B settlement |
| Out 2025 | UMG settles Udio |
| Nov 2025 | Warner settles Suno |
| Jan 2026 | Merlin-Udio deal |
| Abr 2026 | Kobalt, Believe-Udio |
| Mai 2026 | UMG/Sony expandem Suno lawsuit (+61k tracks) |
| Jul 2026 | Suno summary judgment hearing (scheduled) |

---

## Fontes

- [RIAA v. Suno/Udio — AI Vortex](https://www.aivortex.io/legal/ai-case-law/suno-udio-music-ai/)
- [AI Music Lawsuits Timeline — Dynamoi](https://dynamoi.com/learn/ai-music-distribution/ai-music-copyright-cases-timeline)
- [State of Vocal Data Licensing 2026](https://thevocalmarket.com/blogs/enterprise/state-of-vocal-data-licensing-2026)
