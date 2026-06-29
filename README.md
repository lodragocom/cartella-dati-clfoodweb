# 📁 cartella-dati-clfoodweb

Documentazione completa e sempre aggiornata del progetto **CL-FOOD** (sito web `clfoodweb`).
Serve a memorizzare lo stato del progetto, l'architettura, le credenziali (inventario, non i valori) e i prossimi passi.

> ⚠️ **Sicurezza**: in questa cartella NON vanno i valori delle chiavi segrete.
> Vedi [CREDENZIALI.md](./CREDENZIALI.md) per l'inventario. I valori reali, se proprio servono in locale,
> vanno in `segreti.local` (escluso da git tramite `.gitignore`).

---

## Indice

| File | Contenuto |
|---|---|
| [ARCHITETTURA.md](./ARCHITETTURA.md) | Stack tecnologico, struttura cartelle, pagine/route |
| [SUPABASE.md](./SUPABASE.md) | Database, schema, RPC, trigger, RLS, storage, Edge Functions |
| [EMAIL.md](./EMAIL.md) | Sistema email completo (template auth + SMTP + funzione ordini) |
| [DEPLOY-DNS.md](./DEPLOY-DNS.md) | Hosting, dominio, DNS, repository git |
| [CREDENZIALI.md](./CREDENZIALI.md) | Inventario di tutte le credenziali (nome, dove sta, come rigenerarla) |
| [TODO.md](./TODO.md) | Cosa manca / prossimi passi |
| [CHANGELOG.md](./CHANGELOG.md) | Cronologia di cosa è stato fatto |

---

## Cos'è CL-FOOD

Attività con sede a **Zürich** (fondatore **Calogero Lombardo**) che importa e distribuisce
prodotti enogastronomici **siciliani e italiani artigianali** in tutta la Svizzera.

**3 linee di prodotto:**
- **Pasta Primeluci** (Pastificio Gallo) — pasta fresca seccata, mandato
- **Olio EVO, legumi, salse** — produzione propria CL-FOOD
- **Panettoni e dolci Frolsi** (pistacchio, caramello, agrumi…) — mandato

**Modello del sito**: non un classico shop, ma un **funnel B2B-first**.
Prezzi nascosti fino alla registrazione · distinzione **Privato** vs **Azienda/B2B** (con P.IVA, prezzi diversi) ·
registrazione → selezione prodotti → preventivo automatico → pagamento (bonifico, carta, TWINT) ·
focus privati sui **panettoni regalo**.

**Brand**: "siciliano caldo e autentico". Logo `cl-food lombardo`.
Colori: verde oliva `#4F5D2A`, oro `#E0A52C`, arancio `#D2691E`, crema `#F6EFE3`.

**Lingue**: trilingue **IT / DE / FR**.

---

## Stato sintetico (17 giugno 2026)

| Area | Stato |
|---|---|
| Sito front-end (TanStack Start) | ✅ sviluppato (catalogo, funnel, checkout, legale, SEO) |
| Database Supabase (60 prodotti, 12 categorie) | ✅ popolato e attivo |
| Auth (registrazione Privato/Azienda + admin) | ✅ funzionante |
| Sistema email (template + SMTP + funzione ordini) | ✅ completo e testato |
| Repository GitHub + privato (git.lodrago.ch) | ✅ allineati |
| **Deploy su Cloudflare (dominio cl-food.ch)** | ⏳ **da completare** (serve adapter Cloudflare) |

Dettagli e prossimi passi in [TODO.md](./TODO.md).
