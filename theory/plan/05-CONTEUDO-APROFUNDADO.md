# 05 — Conteúdo Aprofundado por Eixo

> Desenvolvimento temático — o que cada eixo ensina em profundidade.

---

## Eixo A — Fundamentos no violão

### A.1 Afinação e mapa cromático

O violão é um **mapa bidimensional**: horizontal = tempo/casa, vertical = corda = registro.

Conteúdo:
- Afinação padrão e por que E A D G B e (quintas + terça)
- Nomear notas até 12º traste (equivalência oitava)
- Exercício auditivo: cantar nota antes de tocar
- **Brasil:** violão de 7 cordas (H) — menção e diferenças no baixo

### A.2 Intervalos

Conteúdo:
- Tabela completa: uníssono → oitava (nome, semitons, inversão)
- Intervalos consonantes vs dissonantes — contexto harmônico
- Intervalos no braço: mesma corda vs cordas cruzadas
- **Prática:** "Intervalo do dia" — terça de jobimiana em 5 obras

### A.3 Leitura de cifra como representação teórica

- Acorde = símbolo compacto de empilhamento
- Slash chords = baixo independente
- Parenteses: D7(9) vs D9 — nuance BR vs US
- Capo e transposição mental

---

## Eixo B — Campo harmônico

### B.1 Construção do campo (maior)

Para cada grau:
| Grau | Acorde | Função | Sensação |
|------|--------|--------|----------|
| I | C7M | T | repouso |
| ii | Dm7 | SD | movimento |
| iii | Em7 | T (subst.) | cor |
| IV | F7M | SD | abertura |
| V | G7 | D | tensão |
| vi | Am7 | T | relativa |
| vii° | Bm7(b5) | D | pré-dominante |

Conteúdo visual: tabela interativa + braço (shape de cada grau em CAGED).

### B.2 Campo menor

- Menor natural vs harmônico vs melódico — tabela comparativa
- V7 em menor harmônico (terça maior na dominante)
- ii° vs ii7 — contexto clássico vs jazz

### B.3 Funções harmônicas (aprofundamento)

- **Tônica (T):** I, iii, vi — onde "casa"
- **Subdominante (SD):** IV, ii — "sai de casa"
- **Dominante (D):** V, vii° — "precisa resolver"
- Fluxograma de movimentos permitidos (Schenker simplificado)

### B.4 Campo harmônico na MPB

Análises guiadas (trechos):

1. **Garota de Ipanema** (Fá maior) — II–V–I e tensões
2. **Chega de Saudade** — bossa e pré-dominantes
3. **Construção** (Chico) — progressão narrativa
4. **Carinhoso** (Pixinguinha) — choro e modulação
5. **O Que É Que a Baiana Tem?** — ritmo + harmonia

Cada análise: cifra anotada com graus + diagrama fluxo + shapes violão.

---

## Eixo C — Construção de acordes

### C.1 Empilhamento de terças

Diagrama interativo:
```
    13
    11
    9
    7
    5
    3
    1  ← fundamental
```

Conteúdo:
- Terça maior (4 semitons) vs menor (3)
- Quinta justa, diminuta, aumentada
- Sétima: M (11 sem) vs m (10 sem) vs dim (9 sem)

### C.2 Tétrades e extensions

- Quando 9 "substitui" 1 (omitir fundamental em voicings)
- 11 em acorde de 7M — evitar (menor) vs usar (#11 Lydian)
- 13 vs b13 — cor bossa vs tensão

### C.3 Voicings no violão (MPB)

Estudo comparativo por acorde:

| Acorde | Voicing aberto | Voicing fechado | Voicing Jobim |
|--------|----------------|-----------------|---------------|
| D7M(9) | X 5 4 2 0 0 | X X 0 11 10 9 | … |
| G7(13) | … | … | … |

Integração com `data/chords/dictionary.json` existente.

### C.4 Acordes alterados e passagem

- G7(b9) → Cm
- Db7 → C (trítono)
- Diminuto como passagem entre I e ii

---

## Eixo D — Progressões e cadências

### D.1 Vocabulário de cadências

| Nome | Símbolos | Exemplo C | Sensação |
|------|----------|-----------|----------|
| Autêntica | V7–I | G7–C | resolução forte |
| Plagal | IV–I | F–C | "amen" |
| Deceptiva | V7–vi | G7–Am | surpresa |
| Half cadence | → V | …–G7 | pausa |
| Backdoor | bVII7–I | Bb7–C | MPB/jazz |

### D.2 Progressões recorrentes MPB

- I–VI–II–V (*Só Tinha de Ser com Você*)
- I–IV–#IVdim–I–III7–VI7–II7–V7 (clássico choro)
- Ciclo de quintas: III7–VI7–II7–V7–I

### D.3 Modulação

Tipos com exemplos BR:
- Dominante comum
- Tom relativo
- Semi-tom ascendente (choro)
- Modulação por pivô (Djavan)

---

## Eixo E — Ritmo brasileiro

### E.1 Bossa nova

- Pattern baixo: tônica–quinta–tônica–quinta alternando
- Syncopation na mão direita
- BPM 120–140, feel "swing leve"
- Diagrama: grade 16ths interativa

### E.2 Samba

- Partido alto vs batucada no violão solo
- Contratempo na síncope
- Exemplo: *Samba em Prelúdio* (Baden Powell)

### E.3 Choro

- Ritmo 2/4 vs 3/4 em maxixe
- Harmonia rápida + modulação
- Referência: Dilermando Reis obras

### E.4 Regional

- Baião: bass pattern Nordeste
- Frevo, xote — menção e recursos
- Toquinho: violão como percussão

---

## Eixo F — Arranjo

### F.1 Camadas no violão solo

```
┌─────────────────────────────┐
│ Melodia (agudo)             │
├─────────────────────────────┤
│ Harmonia (meio)             │
├─────────────────────────────┤
│ Baixo (grave)               │
├─────────────────────────────┤
│ Percussão (ataques, mutes)  │
└─────────────────────────────┘
```

### F.2 Processo de arranjo (8 passos)

1. Ouvir original 3×
2. Identificar forma (AABA, verso-refrão)
3. Mapear harmonia com graus
4. Escolher tom no violão (capo?)
5. Escrever baixo
6. Preencher harmonia
7. Inserir melodia onde couber
8. Intro/outro + dinâmica

### F.3 Estudo de arranjos existentes

- GP files Dilermando no acervo
- Comparação cifra vs arranjo publicado
- "O que o arranjador mudou harmonicamente?"

---

## Eixo G — Glossário essencial (amostra)

| Termo | Definição curta |
|-------|-----------------|
| Cadência | Fórmula de acordes que fecha frase |
| Tensão | Nota de extensão (9, 11, 13) ou alteração |
| Voicing | Disposição vertical das notas |
| Campo harmônico | Acordes nativos de uma escala/tonalidade |
| Modulação | Mudança de tom |
| Partido alto | Padrão rítmico de samba |
| Levada | Padrão de acompanhamento |
| Pestana | Barra com dedo indicador |
| CAGED | Sistema 5 formas móveis |

(Glossário completo: 120+ termos — Fase 2)
