# 11 — Mapa de obras pedagógicas

> Referência para authoring, ingestão de cifras e ligação aula ↔ Song View.
> Fonte de dados: [`scripts/theory/obras-pedagogicas.ts`](../../scripts/theory/obras-pedagogicas.ts)

---

## Tier 1 — Análise harmônica N3–N5

| workId | Título | Artista | Annotation | CifraClub |
|--------|--------|---------|------------|-----------|
| `garota-de-ipanema` | Garota de Ipanema | Tom Jobim | sim | [link](https://www.cifraclub.com.br/tom-jobim/garota-de-ipanema/) |
| `chega-de-saudade` | Chega de Saudade | Tom Jobim | sim | [link](https://www.cifraclub.com.br/tom-jobim/chega-de-saudade/) |
| `corcovado` | Corcovado | Tom Jobim | sim | [link](https://www.cifraclub.com.br/tom-jobim/corcovado/) |
| `wave` | Wave | Tom Jobim | sim | [link](https://www.cifraclub.com.br/tom-jobim/wave/) |
| `insensatez` | Insensatez | Tom Jobim | sim | [link](https://www.cifraclub.com.br/tom-jobim/insensatez/) |
| `aguas-de-marco` | Águas de Março | Tom Jobim | sim | [link](https://www.cifraclub.com.br/tom-jobim/aguas-de-marco/) |
| `construcao` | Construção | Chico Buarque | sim | [link](https://www.cifraclub.com.br/chico-buarque/construcao/) |
| `carinhoso` | Carinhoso | Pixinguinha | sim | [link](https://www.cifraclub.com.br/pixinguinha/carinhoso/) |
| `o-que-e-que-a-baiana-tem` | O Que É Que a Baiana Tem? | Dorival Caymmi | sim | [link](https://www.cifraclub.com.br/dorival-caymmi/o-que-e-que-a-baiana-tem/) |

## Tier 2 — Modulação, ritmo, análise avançada

| workId | Título | Annotation | Uso principal |
|--------|--------|------------|---------------|
| `samba-de-uma-nota-so` | Samba de Uma Nota Só | sim | Ciclo de quintas N5 |
| `ingenuo` | Ingênuo | sim | Modulação N7–N8 |
| `oceano` | Oceano | sim | Modulação Djavan N5/N8 |
| `samba-de-verao` | Samba de Verão | sim | Modulação N4/N5 |
| `desafinado` | Desafinado | sim | Dissonância N4/N8 |
| `triste` | Triste | sim | Harmonia Jobim N8 |
| `so-danco-samba` | Só Danço Samba | sim | Ritmo bossa N6 |

## Tier 3 — Fundamentos N1–N2 (já no catálogo)

| workId | Título | Artista |
|--------|--------|---------|
| `flor-de-lis` | Flor de Lis | Djavan |
| `lilas` | Lilás | Djavan |
| `se-ela-perguntar` | Se Ela Perguntar | Dilermando Reis |

---

## Pipeline de ingestão

```bash
npm run ingest:theory-obras          # Tier 1 + Tier 2
npm run cifra:normalize
npm run build:chords
npm run build:index
```

## Integração na aula (tríade)

1. `example` com `workId` + `youtube` (opcional)
2. `song-chart` com `annotation` (graus pedagógicos)
3. `work-link` → Song View com cifra completa

---

## Nota legal

Cifras importadas com `source: "cifraclub"` e `sourceUrl` no JSON da obra. Uso educacional local.
