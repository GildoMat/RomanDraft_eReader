# Slide promozionali — come pubblicarle nel repository GitHub

Repository: `https://github.com/GildoMat/RomanDraft_eReader` (pubblico, ramo `main`).
L'app legge **solo** `promo/slides.json` e i file che quel manifest indica.

## 1. Cosa copiare nel repository
Copia la cartella `promo/` di questo zip nella radice del repository:

```
RomanDraft_eReader/
└── promo/
    ├── slides.json
    └── covers/
        ├── esempio_copertina_verticale.jpg
        └── esempio_scheda_orizzontale.jpg
```
Verifica nel browser che questo indirizzo mostri il JSON:
`https://raw.githubusercontent.com/GildoMat/RomanDraft_eReader/main/promo/slides.json`
(il repository deve essere pubblico; con un repository privato l'app non vede nulla).

## 2. Il file slides.json
| Campo | Significato |
|---|---|
| `version` | Numero intero. **Cambialo ogni volta che vuoi che le slide ricompaiano** a chi le ha già viste. |
| `maxShows` | Quante aperture dell'app mostrano le slide per quella `version` (1–3, default 2). Poi silenzio finché `version` non cambia. |
| `enabled` | `false` = spegne tutto senza cancellare il file. |
| `items` | Fino a 8 slide, mostrate a rotazione. |
| `items[].image` | Percorso relativo a `promo/` (es. `covers/libro.jpg`) oppure URL `https://` completo. JPG/PNG/WebP, **max 2 MB**. |
| `items[].text` | Didascalia (max 280 caratteri). Può esserci anche senza immagine. |
| `items[].seconds` | Durata della slide, 2–15 s (default 5). |
| `items[].link` | **Facoltativo.** Se presente (solo `https://`) la slide è toccabile e apre il browser; se manca, nessun tocco. |

Ogni slide deve avere almeno `image` o `text`. Voci non valide vengono ignorate senza bloccare le altre.

## 3. Immagini
- Copertine **verticali**: ~800×1200 px. Schede **orizzontali**: ~1200×630 px. Si adattano da sole (nessun ritaglio).
- Su un'immagine più grande di 1400 px l'app riduce comunque in memoria. Meno peso = download più rapido.

## 4. Come si comporta l'app
- Il manifest viene scaricato **in background** (timeout 4–6 s) al più ogni 6 ore; all'avvio l'app non aspetta mai la rete.
- **Prima installazione:** il primo avvio scarica, le slide compaiono dal **secondo** avvio.
- Se `slides.json` non esiste (404) o è vuoto, non compare nulla.
- Un manifest non valido viene scartato e resta in uso la versione precedente.
- Nessun tracciamento: l'app invia solo la richiesta HTTPS del file (più un'intestazione ETag per risparmiare banda).
- L'utente può chiudere le slide con ✕ sulla home: per quella `version` non compaiono più.
