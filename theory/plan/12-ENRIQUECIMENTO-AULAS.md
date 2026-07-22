# 12 — Mapa de enriquecimento editorial (profundidade)

> **Problema:** `upgrade-theory-lessons.ts` gerou aulas que passam no audit (105/100) mas repetem o mesmo texto genérico, os mesmos exercícios e progressões irrelevantes ao título. Score ≠ qualidade pedagógica.
>
> **Padrão ouro:** [`n3-m1-a2.json`](../../data/theory/lessons/n3-m1-a2.json) + [`aula-exemplo.html`](../../apps/styleguide/screens/teoria-musical/aula-exemplo.html) + [`05-CONTEUDO-APROFUNDADO.md`](./05-CONTEUDO-APROFUNDADO.md).
>
> **Regra:** cada aula tem **conceito único**, **obra com trecho comentado**, **exercícios que só fazem sentido nessa aula**, **micro-prática tocável em 3 min**.

**Última auditoria (`npm run audit:theory:depth`):** 96 aulas · **0 genéricas** · **78+ rico** · restante misto (conteúdo válido, heurística conservadora). N3: **14/14** sem genéricas (CI: `audit:theory:depth:ci`).

## Critérios de profundidade (checklist humano)

| # | Critério | Anti-padrão (rejeitar) |
|---|----------|------------------------|
| D1 | `prose` explica o conceito com exemplo concreto antes da regra | "esta aula dá nome ao que seus dedos já sentem" |
| D2 | `exercise intro` recupera aula **anterior** com pergunta específica | "qual é o foco desta aula?" genérico |
| D3 | Componentes com props alinhados ao título (tom, acordes, escala certos) | II–V–I em Dó numa aula de Ré menor |
| D4 | `callout practice` com shapes, compassos ou graus nomeados | "toque 2 compassos e repita" |
| D5 | `feedbackOk`/`feedbackErr` citam harmonia/intervalo/grau | "Correto!" / "Não." |
| D6 | Obra: trecho + comentário de **graus** (não só "cadência bossa") | annotation copiada do mapa sem ligação à aula |
| D7 | ≥1 exercício impossível de responder sem ter lido a aula | MC com "ancorar no violão" vs "decorar" |

## Pipeline editorial

```
05-CONTEUDO + brief abaixo → JSON manual → tocar no violão → audit:theory + audit:theory:depth → merge
```

**Não rodar** `npm run upgrade:theory` em aulas já enriquecidas.

---

## Nível 1 — O violão como mapa

| ID | Conceito-núcleo | Obra âncora | Componentes-chave | Status |
|----|-----------------|-------------|-------------------|--------|
| n1-m1-a1 | Afinação EADGBE; 6ª=1ª (Mi oitava) | se-ela-perguntar | fretboard, play-note | **enriquecida** |
| n1-m1-a2 | Casas, pestana, cordas soltas vs paradas | se-ela-perguntar | fretboard, play-note | **enriquecida** |
| n1-m1-a3 | Shape na cifra = diagrama de dedos | flor-de-lis | fretboard-chord, work-link | **enriquecida** |
| n1-m2-a1 | Nomenclatura PT/EN; Dó=C | flor-de-lis | fretboard, play-note | **enriquecida** |
| n1-m2-a2 | Notas Mi/Ré até 12º; oitava na 12ª casa | flor-de-lis | fretboard, play-note | **enriquecida** |
| n1-m2-a3 | Mapa 6×12; equivalência oitava | se-ela-perguntar | fretboard, play-scale | **enriquecida** |
| n1-m3-a1 | 2ª 3ª 4ª — semitons e no braço | flor-de-lis | interval-ruler, play-interval | **enriquecida** |
| n1-m3-a2 | 5ª e 8ª — alicerce da afinação | se-ela-perguntar | interval-ruler, play-interval | **enriquecida** |
| n1-m3-a3 | Semitom vs tom; cromático 1º traste | flor-de-lis | fretboard, play-interval | **enriquecida** |
| n1-m3-a4 | **Desafio:** quintas de Lá em 3 regiões | se-ela-perguntar | fretboard, self_check | **enriquecida** |

---

## Nível 2 — Escalas e tonalidade

| ID | Conceito-núcleo | Obra âncora | Componentes-chave | Status |
|----|-----------------|-------------|-------------------|--------|
| n2-m1-a1 | Fórmula T-T-S; por que S é único | lilas | play-scale, prose com escala | **enriquecida** |
| n2-m1-a2 | Dó maior posição aberta; dedos 2-1-0 | flor-de-lis | fretboard-scale, play-scale | **enriquecida** |
| n2-m1-a3 | Shapes móveis Sol/Ré; pestana | lilas | fretboard-scale CAGED | **enriquecida** |
| n2-m1-a4 | Círculo quintas; armaduras | oceano | circle-fifths, harmonic-table | **enriquecida** |
| n2-m2-a1 | 3 menores comparadas (áudio) | construcao | 3× fretboard-scale, play-scale | **enriquecida** |
| n2-m2-a2 | Relativa Am↔C; mesmo campo | flor-de-lis | harmonic-lab major+minor | **enriquecida** |
| n2-m2-a3 | Lá menor no braço; v7 harmônico | construcao | fretboard-scale minorHarmonic | **enriquecida** |
| n2-m3-a1 | 5 formas CAGED acorde maior | flor-de-lis | fretboard-chord ×3 | **enriquecida** |
| n2-m3-a2 | Conectar CAGED subindo braço | lilas | fretboard-scale + chord | **enriquecida** |
| n2-m3-a3 | Escala dentro de cada forma | oceano | fretboard-scale + CAGED | **enriquecida** |
| n2-m3-a4 | Pentatônica MPB/blues | oceano | fretboard-scale penta | **enriquecida** |
| n2-m3-a5 | **Desafio:** Dó em 3 regiões | lilas | self_check + practice | **enriquecida** |

---

## Nível 3 — Campo harmônico (prioridade 1)

| ID | Conceito-núcleo | Obra / trecho | Micro-prática | Status |
|----|-----------------|---------------|---------------|--------|
| n3-m1-a1 | Algarismos romanos; maiúsc/minúsc; grau ≠ acorde fixo | Garota: **II–V–I** em F (Gm7–C7–F7M) | Nomear graus de Am–F–C–G | **enriquecida** |
| n3-m1-a2 | Campo diatônico C; funções T/SD/D | Garota + Construção | II–V–I em Dó com shapes | **ouro** |
| n3-m1-a3 | Famílias T, SD, D; fluxo Schenker simples | Corcovado refrão | SD→D→T com F7M–G7–C7M | **enriquecida** |
| n3-m1-a4 | Pré-dominante ii vs IV; quando cada uma | Wave intro | Comparar Dm7→G7 vs F7M→G7 | **enriquecida** |
| n3-m2-a1 | Campo em G, D, F; transposição mental | Insensatez | Montar campo em Sol no lab | **enriquecida** |
| n3-m2-a2 | Menor harmônico; V7 com 3ª maior; vii° | Chega de Saudade A7→Dm | A7(b9) resolução em Dm | **enriquecida** |
| n3-m2-a3 | Menor melódico; VI7 e IV7 elevados | Insensatez ponte | Ouvir 6ª e 7ª graus alterados | **enriquecida** |
| n3-m3-a1 | Análise Chega: Dm + modulação D maior | Seção A annotation | Baixo cromático Dm/C; E7/B | **enriquecida** |
| n3-m3-a2 | Construção: narrativa Em; B7 surpresa | construcao annotation | Mapear graus por verso | **enriquecida** |
| n3-m3-a3 | I–VI–II–V–I turn-around bossa | Garota passagem | Toque C–Am7–Dm7–G7–C7M | **enriquecida** |
| n3-m3-a4 | bVII, bVI emprestados modal | Águas de Março | Identificar bVII em trecho | **enriquecida** |
| n3-m3-a5 | **Desafio:** harmonizar melodia 4 notas | Corcovado | D-F-A-C → escolher acordes | **enriquecida** |
| n3-m3-a6 | Ficha campos até 4# (referência) | — | harmonic-table multi-key | **enriquecida** |
| n3-m3-a7 | Quiz N3 intercalado m1–m3 | — | 8 perguntas específicas | **enriquecida** |

---

## Nível 4 — Construção de acordes

| ID | Conceito-núcleo | Obra | Foco profundo | Status |
|----|-----------------|------|---------------|--------|
| n4-m1-a1 | 4 qualidades tríade; semitons 3ª/5ª | desafinado | Comparar C, Cm, C°, C+ | **enriquecida** |
| n4-m1-a2 | Empilhamento terças visual | wave | chord-builder 1-3-5-7 | **enriquecida** |
| n4-m1-a3 | 3 voicings tríade no braço | insensatez | fretboard-chord | **enriquecida** |
| n4-m2-a1 | 7M, 7, m7, m7M — diferença sonora | garota | play-chord comparativo | **enriquecida** |
| n4-m2-a2 | m7(b5); trítono com V7 | chega-de-saudade | Em7(b5)–A7 | **enriquecida** |
| n4-m2-a3 | dim7 simétrico; 3 tons | desafinado | play-chord dim7 | **enriquecida** |
| n4-m3-a1 | Tensões 9 11 13 — quando somar | wave | voicing-grid | **enriquecida** |
| n4-m3-a2 | 7(9) bossa; omitir fundamental | garota Gm7 C7(9) | shapes abertos | **enriquecida** |
| n4-m3-a3 | Alterações b9 #11 b13 | desafinado | play-chord | **enriquecida** |
| n4-m3-a4 | sus4, 7sus4, 7(13) | corcovado | progression | **enriquecida** |
| n4-m4-a1 | Slash chords C/E | aguas-de-marco | fretboard + slash | **enriquecida** |
| n4-m4-a2 | Voicing aberto vs fechado | wave | voicing-grid | **enriquecida** |
| n4-m4-a3 | **Desafio:** G7(13) 3 jeitos | garota | self_check | **enriquecida** |
| n4-m4-a4 | Quiz N4 | — | tensões + tríades | **enriquecida** |

---

## Níveis 5–8

Ver [`04-CURRICULO-PROGRESSIVO.md`](./04-CURRICULO-PROGRESSIVO.md) e [`05-CONTEUDO-APROFUNDADO.md`](./05-CONTEUDO-APROFUNDADO.md) eixos D–H.

| Nível | Aulas | Status |
|-------|-------|--------|
| N5 Progressões/cadências | 12 (n5-m1-a1 … n5-m3-a4) | **enriquecidas** |
| N6 Ritmo brasileiro | 10 (n6-m1-a1 … n6-m3-a3) | **enriquecidas** |
| N7 Arranjo violão | 12 (n7-m1-a1 … n7-m3-a5) | **enriquecidas** |
| N8 Harmonia avançada | 12 (n8-m1-a1 … n8-m3-a6) | **enriquecidas** |

Quizzes/certificação enriquecidos: `n4-m4-a4`, `n5-m3-a4`, `n6-m3-a3`, `n8-m3-a6`.

**Ondas de enriquecimento:**

1. **Onda P1** — N3 completo (14 aulas) — **concluída**
2. **Onda P2** — N1–N2 fundamentos (22) — **concluída**
3. **Onda P3** — N4–N5 harmonia aplicada (26) — **concluída**
4. **Onda P4** — N6–N8 ritmo/arranjo/avançado (34) — **concluída**

---

## Template de brief (copiar por aula)

```markdown
### {id} — {title}
- **Conceito (1 frase):**
- **Recuperação (intro):** pergunta sobre {aula anterior}
- **Ouça:** acordes/notas exatos
- **Núcleo:** 2–3 parágrafos + componente
- **Obra:** {workId} — trecho — graus: ...
- **Prática:** passos numerados com shapes
- **Exercícios:** mc (específico), match, order/dictation, self_check
- **Deep/BR:** fato histórico ou decisão de compositor
```
