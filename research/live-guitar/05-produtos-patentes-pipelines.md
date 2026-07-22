# 05 — Produtos, Patentes e Pipelines Inferidos

> O que apps de tutoria **realmente fazem** (evidência técnica, não marketing).

---

## Mapa produto → pipeline provável

| Produto | Input | Extração inferida | Comparação | Evidência |
|---------|-------|-------------------|------------|-----------|
| **Yousician** | Mic acústico | pitch + salience + timing + volume | vs exercise data item-a-item | [US9218748B2](https://patents.google.com/patent/US9218748B2/en) |
| **GuitarBots** (precursor) | Mic | mesmo assignee Ovelin/Yousician | idem | mesma família patentes |
| **Simply Guitar** (JoyTunes) | Mic | DSP + ML (não público) | gamified score | mercado |
| **Uberchord** | Mic | chord recognition | fretboard | app |
| **Rocksmith** | Cabo/analógico + mic | pitch tracking densidade | nota-a-nota | US9839852 |
| **Fender Play** | Vídeo + menos RT | human teacher, play-along | limitado | produto |
| **Solitito** (OSS) | Mic | CQT+chroma → ONNX | label chord | [HF model card](https://huggingface.co/greblus/solitito-ai) |

---

## Patente Yousician — US9218748B2 (Ovelin/Yousician Oy)

**Inventores:** Kaipainen, Thur · Prioridade 2011 · Assignee atual: Yousician Oy

### Claims técnicos relevantes

1. **Parâmetros de sinal:** frequency, amplitude (pitch + **salience**), timing, duration, volume, articulation, strumming direction
2. **Conversão:** microfone → parameter data (fig. pipeline analise)
3. **Comparação:** parameter data vs **exercise data** (nota, intervalo, acorde pré-definido)
4. **Feedback online** durante execução — não batch
5. **ML adaptativo:** “techniques for recognizing notes can be **self learning** based on collected audio data”
6. **Skill levels** desbloqueiam exercícios — gamificação ligada a métricas objetivas

### Implicação para music-tutor

A patente **valida legalmente** o modelo:

```
exercise_data (simbólico) + audio_analysis (DSP/ML) → diff → feedback RT
```

**Não** exige MIDI interface (diferente de US7030307B2 citado como prior art inferior).

---

## Rocksmith / Ubisoft — US9839852

- Disputa com Yousician — patent claims **demasiado amplas**, Yousician **venceu** 2019
- Rocksmith usa **cabo** + processamento; implica **pitch tracking** de alta densidade no espectro
- Lição: hardware dedicado melhora SNR; mic-only exige mais gate/calibragem

---

## Yousician engineering (público)

- [YouTube: CTO explica audio recognition + ML](https://www.youtube.com/watch?v=69oHzhLiyEE) — pitch, timing, ML em produção
- Job posting ML/AI: “audio recognition technology”, multimodal data, Python modules

**Pipeline inferido:**
1. Feature extraction clássica (pitch/salience/onset)
2. Classificadores ML treinados em dados de utilizadores
3. Comparação com tab/score sincronizado na UI
4. Latency compensation (mencionado em claims)

---

## Clones e referências open source

| Projeto | Stack | Fidelidade Yousician |
|---------|-------|---------------------|
| [Chris-Zbrojkiewicz/guitar-tuner](https://github.com/Chris-Zbrojkiewicz/guitar-tuner) | Pitchy + Worklet | Só afinador |
| [badlogic/tuner](https://github.com/badlogic/tuner) | YIN + Worklet | Só afinador |
| [greblus/solitito](https://github.com/greblus/solitito) | Rust + ONNX chords | Acordes RT |
| [arulandu/chordy](https://github.com/arulandu/chordy) | C++ chroma templates | Chord ID |
| [VannRR/chord_detector](https://github.com/vannrr/chord_detector) | Rust Stark thesis | Chord ID |
| [audiojs/pitch-detection](https://github.com/audiojs/pitch-detection) | JS chroma+chord | Chord ID |

**Gap OSS:** nenhum clone completo **lição + diff voicing + ritmo + gamificação** — oportunidade music-tutor.

---

## Técnicas por tipo de exercício (Yousician-like)

| Exercício | Técnica provável |
|-----------|------------------|
| Single note | monophonic pitch tracker |
| Interval | 2 sequential pitch detections |
| Chord | multi-pitch salience ou chord template |
| Melody line | note-by-note onset+pitch |
| Play-along song | score following + backing sync |
| Strumming pattern | onset timing vs grid |

---

## Diferenciação music-tutor (violação evitada / valor)

| Aspecto | Yousician | music-tutor MVP |
|---------|-----------|-----------------|
| Instrumentos | 5+ | **Violão first** |
| Engine | Fechado | **OSS stack documentado** |
| Voicing diff | Desconhecido | **Pitch-set explícito** |
| LLM | Recente | Opcional pós-sessão |
| Web-first | App nativo | **PWA** |

Próximo: [06 — Referência de lições](./06-referencia-licoes-voicing.md)
