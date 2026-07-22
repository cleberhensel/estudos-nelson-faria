# 06 — Dinâmicas de Prática

> Como o usuário **aprende fazendo** — exercícios, quizzes, desafios e loops.

---

## Tipos de atividade

| Tipo | Duração | Objetivo | Frequência |
|------|---------|----------|------------|
| **Micro-prática** | 2 min | Fixar 1 conceito no violão | Cada bloco |
| **Exercício de aula** | 5–10 min | Consolidar aula | Fim de cada aula |
| **Quiz** | 3–5 min | Avaliar retenção | Fim de módulo |
| **Desafio** | 15–30 min | Aplicação criativa | Fim de nível |
| **Laboratório** | Livre | Explorar interativamente | Durante aula |
| **Projeto** | 1–3 h | Arranjo/análise completa | Níveis 7–8 |

---

## Micro-prática (template)

Cada aula inclui ≥ 1 bloco `callout-practice`:

```markdown
### Prática agora (2 min)
1. Toque a nota **Mi** nas cordas 1, 2 e 6.
2. Suba uma **terça maior** a partir de **Sol** na corda Ré.
3. Compare o som — maior soa "cheio", menor soa "triste".
```

Critério: instruções **sem ambiguidade** — corda + traste + dedo quando relevante.

---

## Exercícios por categoria

### E1 — Identificação

- "Qual grau é **Am7** em **Dó maior**?" (MC)
- "Clique todas as **terças** de **Ré** no braço" (interativo)
- "Ouça e escolha: maior ou menor?" (áudio Fase 2)

### E2 — Construção

- Montar **Dm7** empilhando terças no construtor
- Montar cadência **II–V–I** em **Sol** arrastando chips
- Escrever cifra de campo harmônico de **Fá maior**

### E3 — Aplicação no violão

- Tocar escala de **Lá menor** em 2 posições
- Tocar 4 voicings de **G7(13)** da tabela
- Executar pattern bossa 8 compassos

### E4 — Análise

- Anotar graus em trecho de cifra (input inline)
- Identificar modulação em trecho de *Djavan*
- Comparar 2 voicings: qual mais "bossa"?

### E5 — Criativo

- Harmonizar 4 notas de melodia com campo
- Criar intro 2 compassos em **D7M**
- Reharmonizar verso simples (substituir 1 acorde)

---

## Sistema de quiz

### Regras

- Mínimo 5 questões por quiz de módulo
- Feedback imediato com explicação visual (não só certo/errado)
- Nota ≥ 70% para desbloquear próximo módulo (configurável)
- Refazer ilimitado — mostrar questões alternativas

### Tipos de questão

| `type` | UI Component | Exemplo |
|--------|--------------|---------|
| `mc` | TH-QUIZ-MC | "V7 em Dó maior é:" |
| `order` | TH-QUIZ-ORDER | Ordenar II–V–I |
| `match` | Pares clicáveis | Grau ↔ Acorde |
| `fretboard` | TH-FRETBOARD | Marcar notas |
| `true_false` | 2 botões | "VII° é dominante" |

---

## Desafios por nível

| Nível | Desafio | Critério sucesso |
|-------|---------|------------------|
| 1 | Mapa de quintas no braço | 6 localizações corretas |
| 2 | 3 escalas, 3 regiões | Auto-check + vídeo opcional |
| 3 | Harmonizar 8 compassos | Quiz análise |
| 4 | 5 acordes com tensão | Construtor validado |
| 5 | Analisar 1 MPB completa | 80% graus corretos |
| 6 | Mesma harmonia, 3 ritmos | Gravação áudio Fase 3 |
| 7 | Arranjo 32 compassos | Checklist completo |
| 8 | Análise + reharm 1 obra | Rubrica |

---

## Desafio semanal (hub)

Rotação automática:
- Semana A: "II–V–I em 3 tons diferentes"
- Semana B: "Identifique o gênero pelo ritmo"
- Semana C: "3 voicings Jobim para G7(13)"

Integração repertório: sugerir obras do catálogo que exercitam o tema.

---

## Spaced repetition (Fase 2)

Conceitos marcados "difícil" retornam em:
- +1 dia: micro-prática
- +3 dias: 2 questões quiz
- +7 dias: desafio mini

Algoritmo SM-2 simplificado; storage em `theory-progress.json`.

---

## Gamificação leve (opcional)

- **Streak** dias consecutivos (sem punição por quebrar)
- **Badges:** "Campo Harmônico", "Bossa Groove", "Choro Survivor"
- **Sem leaderboard** — foco estudo solo

---

## Rubrica — Projeto arranjo (Nível 7)

| Critério | Peso | Descrição |
|----------|------|-----------|
| Forma clara | 20% | Intro, A, B, outro identificáveis |
| Baixo independente | 20% | Linha grave coerente |
| Harmonia correta | 25% | Graus alinhados ao tom |
| Ritmo de gênero | 20% | Pattern reconhecível |
| Criatividade | 15% | Algo pessoal |

Autoavaliação + checklist; revisão por pares = fora de escopo MVP.

---

## Ligação obra ↔ exercício

Fluxo "Aplicar na música":
1. Aula sobre II–V–I
2. Botão "Praticar em Garota de Ipanema"
3. Song View abre com overlay graus (Fase 2)
4. Exercício: marcar onde ocorre II–V–I

Dados: `relatedWorks` em metadados da aula.
