# Protocollo lavori — 30 giugno 2026

Sessione di lavoro su **CL-FOOD / `clfoodweb`**: catalogo Frolsi, redesign editoriale di scheda prodotto e catalogo, rollover immagini, pannello admin, import del catalogo continuativi Frolsi e del catalogo pasta Gallo (Primeluci), nuove landing page e foglio Excel di gestione.

Questo documento riassume **cosa è stato fatto**, le **soluzioni/decisioni tecniche** e lo **stato attuale** del progetto. Aggiornare il [CHANGELOG.md](./CHANGELOG.md) e il [TODO.md](./TODO.md) coerentemente.

---

## 1. Cosa è stato fatto

### 1.1 Logo
- Inserito il **logo ufficiale** (`cl-food_logo_trasparente.png` → `public/logo.png`) nell'header. Su sfondo chiaro (header) si mostra l'immagine; nel footer (sfondo scuro) resta il wordmark testuale adattivo. Componente: `src/components/site/Wordmark.tsx`.

### 1.2 Catalogo Frolsi — Panettoni 2025 (linea festiva)
- Sostituiti i **3 panettoni segnaposto** (agrumi, caramello, pistacchio) con gli **11 prodotti ufficiali** del catalogo Frolsi 2025: 8 panettoni + 3 pandori.
- Prezzi **privato** (dal catalogo Privati) e **azienda/B2B** (dal catalogo Aziende), differenza B2B costante di CHF 5.00.
- Immagini scatola ritagliate dal PDF del catalogo, standardizzate su sfondo bianco quadrato (`public/products/frolsi-*.jpg`).
- Testi editoriali completi (storia) in **IT/DE/FR**, con EAN, SKU, shelf-life.

### 1.3 Schema database esteso (Supabase, progetto `zgnyjbxvxoupdaeuohtm`)
Aggiunte colonne alla tabella `products`:
- `story_it / story_de / story_fr` — testo editoriale lungo
- `ean`, `sku`, `shelf_life`
- `image_hover_url` — foto "golosa" mostrata al passaggio del mouse (rollover)
- `gallery_urls text[]` — immagini extra per la galleria scheda prodotto

La RPC **`get_products`** è stata aggiornata per restituire i nuovi campi **mantenendo intatto il gating prezzi** (non loggato → prezzo nascosto; privato → `price_private`; azienda → `price_b2b`). I tipi TypeScript (`src/integrations/supabase/types.ts`, `src/lib/products.ts`) sono allineati.

### 1.4 Scheda prodotto editoriale (`src/routes/prodotto.$slug.tsx`)
- Layout editoriale professionale: breadcrumb, hero con immagine sticky, chip peso/formato, riga "trust".
- **Galleria immagini**: visore principale + striscia di miniature (principale + foto fetta + copertine extra da `gallery_urls`), con zoom morbido.
- Pannello **specifiche tecniche** (peso, conservazione/shelf-life, EAN, SKU).
- Sezione **"La storia"** con capolettera; abbinamenti + certificazioni; prodotti correlati.

### 1.5 Catalogo (`src/routes/catalogo.tsx`)
- Header editoriale, **toolbar sticky** con ricerca e **dropdown moderni** (Brand, Categoria, Ordina) su Radix Popover, chip filtri attivi rimovibili, contatore, "azzera tutto".
- **Filtri via URL** (`validateSearch`): `/catalogo?brand=frolsi&cat=panettoni&q=...` apre il catalogo già filtrato (con guardia sul primo render per non azzerare la categoria).

### 1.6 ProductCard "golosa" + rollover (`src/components/site/ProductCard.tsx`)
- Card più appetitosa (zoom morbido, glassa calda in hover, badge linea).
- **Rollover immagine**: al passaggio del mouse la foto scatola sfuma (crossfade) nella **foto fetta** dove disponibile (`image_hover_url`), con badge "Gusta".

### 1.7 Pannello admin (`src/routes/admin.tsx`)
- Riquadro dedicato **"Prezzi per tipo account"** (Privato + Azienda/B2B) con differenza calcolata e nota su chi vede cosa.
- Gestione dei nuovi campi: **Shelf life, EAN, SKU, Storia IT/DE/FR**, **Immagine rollover (hover)** e **Galleria** (una URL per riga). Upload foto parametrizzato (principale/hover).

### 1.8 Categoria Legumi
- **Rimossa** la categoria "Legumi Siciliani" (era già priva di prodotti).

### 1.9 Catalogo Frolsi — Continuativi 2026 (linea da forno)
Dal brochure "Continuativi 2026", stesso stile editoriale (storia IT/DE/FR + SKU + foto):
- **Aggiornati** (prezzi invariati): Muffin (Cacao con foto rollover, Limone, Arancia); Brioche (con Tuppo, senza Tuppo, Salata); Buns/Sbuns (Classico 2/3 pz, Maxi, Black, 7 Cereali); 8 Frollini/Biscotti in vaschetta esistenti (storia + shelf-life 365 giorni).
- **Aggiunti come nuovi inattivi** (in attesa prezzo): Muffin Vaniglia; Brioche Vaniglia e Gocce di Cioccolato; Sbuns Rosa; linea **Frollini "Le Novelle"** (8 gusti con foto confezione: Classici, Classici alla Panna, Gocce di Cioccolato, Cacao, Arancia e Zenzero, Miele e Zenzero, Limone, Integrali).

### 1.10 Landing page e navigazione
- Split di "Panettoni & Dolci" in due landing **professionali**: **`/panettoni`** (panettoni, pandori) e **`/forno`** (brioche, buns, muffin, frollini).
- Navbar aggiornata (voci Panettoni e Forno, label IT/DE/FR); **`/dolci` reindirizza** a `/panettoni`.
- **Home**: i panettoni sono "in evidenza" (4 card reali con rollover); tile categorie passati a 4 (Pasta · Olio · Panettoni · Forno).
- **Colombe disattivate** (non visibili nello shop né nella landing).
- Su `/panettoni` la sezione regalo è in fondo come **banner** a tutta larghezza; la sezione "Maestria" sta sotto il banner con **pulsante al catalogo già filtrato** sui panettoni.

### 1.11 Catalogo pasta Gallo — Primeluci
Dal "CATALOGO 2025 EN":
- **Creata categoria "Le Integrali"** (`integrali`).
- **25 prodotti esistenti aggiornati** con EAN, tempo di cottura (in descrizione) e **storia della famiglia Gallo** (IT/DE/FR).
- **66 nuovi formati aggiunti come inattivi** (senza prezzo, senza immagine), con descrizione + cottura + EAN + storia, collegati alla linea corretta (`line_it`) per la pagina `/pasta`.
- `/pasta` aggiornata per mostrare anche la linea Integrali.

### 1.12 Foglio Excel di gestione
- **`Catalogo_CLFOOD_gestione.xlsx`** (in `cartella-dati-clfoodweb/`): foglio "Prodotti" con tutte le colonne editabili (prezzi privato/azienda, immagini, galleria, `active`), "Categorie" e "Istruzioni". Pensato per gestire l'intero catalogo e fare il re-import.

---

## 2. Soluzioni / decisioni tecniche

- **Gating prezzi preservato**: ogni modifica alla RPC `get_products` mantiene la logica privato/azienda/non-loggato.
- **Nuovi prodotti = inattivi** (`active=false`) finché non si hanno i prezzi privato/azienda: non compaiono nello shop ma sono già pronti in admin/Excel.
- **Foto**: usate le immagini del brochure solo dove pulite e con sapore inequivocabile; altrimenti mantenute le immagini esistenti (per evitare abbinamenti sbagliati). Rollover aggiunto dove c'è una foto "fetta" affidabile.
- **Filtri catalogo via URL**: `validateSearch` + inizializzazione stato dai search param + guardia sul primo `useEffect` per non azzerare la categoria al mount.
- **Redirect** `/dolci → /panettoni` via `beforeLoad`.
- **Duplicati pasta**: alcuni nomi nuovi sono simili agli esistenti (es. "Spaghetto/Spaghettoni" vs "Spaghetti", "Ditali" vs "Ditalini"); inseriti come prodotti nuovi separati per **non sovrascrivere** gli attivi — da unire/sistemare dal foglio.
- **Operazioni massive** (export Excel, import pasta) eseguite via subagente con query a blocchi per restare sotto il limite di output.

---

## 3. Stato attuale (30 giugno 2026)

**Prodotti totali: 143** (63 attivi · 80 inattivi)

| Brand | Totali | Attivi | Inattivi |
|---|---|---|---|
| Primeluci (pasta) | 94 | 28 | 66 |
| Frolsi (dolci/forno) | 44 | 30 | 14 |
| CL-FOOD (olio/salse) | 5 | 5 | 0 |

**Categorie pasta**: Classiche (62), Speciali (6), Trafilate al Bronzo (19), **Le Integrali (7, nuova)**.
**Categorie Frolsi**: Panettoni (panettoni+pandori), Colombe (disattivate), Brioche, Muffin, Sbuns, Frollini & Biscotti.

**Live e funzionante**: logo, scheda prodotto editoriale + galleria, catalogo con filtri/dropdown e filtri via URL, ProductCard con rollover, pannello admin completo, landing `/panettoni` e `/forno`, home con panettoni in evidenza, distinzione prezzi privato/azienda. TypeScript compila pulito (`npx tsc --noEmit`).

**In attesa**: gli **80 prodotti inattivi** (66 pasta + 14 Frolsi continuativi) restano nascosti finché non si inseriscono i **prezzi** e si imposta `active=TRUE`.

> Nota build: nell'ambiente sandbox `vite/esbuild` non gira (binari macOS nel `node_modules`), quindi la verifica automatica usata è `tsc --noEmit`. In locale su Mac `npm run dev` funziona.

---

## 4. TODO / prossimi passi

- 🔴 **Prezzi nuovi prodotti**: inserire prezzi privato/azienda per i 66 formati pasta e i 14 Frolsi continuativi → poi `active=TRUE` (anche in blocco). Strumento: `Catalogo_CLFOOD_gestione.xlsx`.
- 🟡 **Immagini pasta**: i nuovi formati pasta sono senza foto.
- 🟡 **Dedupe pasta**: unire i formati nuovi simili agli esistenti (spaghetto/spaghetti, ditali/ditalini, ecc.).
- 🟡 **Re-import dall'Excel**: definire la procedura per ricaricare il foglio nel database in automatico.
- 🟢 **Google Sheets**: caricare il catalogo gestione su Google Drive/Sheets (rimasto in sospeso).
- 🔴 **Deploy Cloudflare** (dal TODO precedente): il Worker serve ancora "Hello world", manca l'adapter/preset Cloudflare per TanStack Start; poi collegare il dominio cl-food.ch.

---

## 5. File principali toccati

```
src/components/site/Wordmark.tsx        logo immagine
src/components/site/ProductCard.tsx     card golosa + rollover
src/components/site/Header.tsx          nav Panettoni/Forno
src/routes/prodotto.$slug.tsx           scheda prodotto editoriale + galleria
src/routes/catalogo.tsx                 dropdown moderni + filtri via URL
src/routes/admin.tsx                    prezzi, story, ean/sku/shelf, hover, galleria
src/routes/index.tsx                    home: panettoni in evidenza + tile
src/routes/panettoni.tsx                NUOVA landing panettoni
src/routes/forno.tsx                    NUOVA landing forno
src/routes/dolci.tsx                    redirect → /panettoni
src/routes/pasta.tsx                    linea Integrali
src/lib/i18n.tsx                        label nav/pillars IT/DE/FR
src/lib/products.ts                     tipo Product esteso
src/integrations/supabase/types.ts      tipi DB estesi
public/products/*                       immagini prodotto (Frolsi + hover)
cartella-dati-clfoodweb/Catalogo_CLFOOD_gestione.xlsx  foglio gestione
```

Database: tabella `products` (nuove colonne) + RPC `get_products` aggiornata + categoria `integrali` creata + colombe `active=false`.
