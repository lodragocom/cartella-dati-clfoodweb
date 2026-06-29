# Changelog / Cronologia

Cronologia di cosa è stato fatto sul progetto CL-FOOD (`clfoodweb`).
Aggiornare in cima a ogni intervento rilevante.

---

## 30 giugno 2026 — Catalogo Frolsi/pasta, redesign editoriale, landing, Excel gestione

- Catalogo Frolsi **Panettoni 2025** (11 ufficiali) e **Continuativi 2026** (forno): aggiornati esistenti + nuovi inattivi.
- Import **catalogo pasta Gallo (Primeluci)**: 66 nuovi formati inattivi, 25 esistenti aggiornati, nuova categoria **Le Integrali**.
- **Schema esteso**: `story_*`, `ean`, `sku`, `shelf_life`, `image_hover_url`, `gallery_urls`; RPC `get_products` aggiornata (gating prezzi preservato).
- **Redesign**: scheda prodotto editoriale + galleria, catalogo con dropdown moderni + filtri via URL, ProductCard "golosa" + rollover, pannello admin (prezzi privato/azienda, story, hover, galleria).
- **Landing** nuove `/panettoni` e `/forno` (split di Panettoni & Dolci), nav aggiornata, redirect `/dolci`, home con panettoni in evidenza. Colombe disattivate.
- **Foglio Excel** di gestione dell'intero catalogo (`Catalogo_CLFOOD_gestione.xlsx`).
- Stato: **143 prodotti** (63 attivi, 80 inattivi in attesa prezzi). Dettagli completi in **[PROTOCOLLO-2026-06-30.md](./PROTOCOLLO-2026-06-30.md)**.

## 17 giugno 2026 — Sistema email completo + documentazione

- Brandizzati **6 template email di sistema** (registrazione, magic link, reset password, cambio email, invito, codice verifica) via Management API.
- Verificato **SMTP Resend** in Supabase (noreply@cl-food.ch), dominio cl-food.ch verificato su Resend.
- **Deployata** la Edge Function `send-order-email` (v1, ACTIVE) via Management API.
- Impostati i secret `ORDER_FROM_EMAIL`, `ORDER_NOTIFY_EMAIL`; aggiunto `RESEND_API_KEY`.
- **Test reale superato**: ordine di prova → `{ok:true}` → email inviate → ordine cancellato.
- Creata questa **cartella-dati-clfoodweb** con tutta la documentazione.

## ~11–16 giugno 2026 — Migrazione da Lovable + infrastruttura

- Migrato tutto **fuori da Lovable** (dismesso): Supabase proprio (`zgnyjbxvxoupdaeuohtm`), repo, hosting.
- Repo rinominati `sicily-table` → **`clfoodweb`**; allineati GitHub (`origin`) + Forgejo privato (`lodrago`).
- Ricostruito schema DB su Supabase proprio (hand-built SQL): tabelle, RPC `get_products` (gating prezzi), trigger `handle_new_user`, ruoli/`is_admin`, storage, **60 prodotti + 12 categorie**.
- Disattivata conferma email (registrazione immediata); admin assegnato solo a info@cl-food.ch.
- Predisposto Worker Cloudflare `clfoodweb` (deploy app ancora da completare).

## Fasi di sviluppo iniziali

- Brand identity CL-FOOD (logo, palette verde/oro/arancio/crema).
- Sito full-stack: catalogo, schede prodotto (pasta, olio, panettoni/Frolsi).
- Registrazione + gating prezzi B2B/privati; dashboard admin + filtri/ricerca.
- Redesign editoriale + funnel + carrello/preventivo; checkout con bonifico fittizio.
- SEO (meta, Open Graph, dati strutturati, sitemap).
- Pagine legali (Disclaimer, AGB, Nutzungsbedingungen, Datenschutz, Cookies, Impressum) localizzate **IT/DE/FR**.
