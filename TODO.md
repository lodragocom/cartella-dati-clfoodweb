# TODO / Prossimi passi

## 🔴 Priorità: deploy del sito su Cloudflare

Il Worker `clfoodweb` serve ancora "Hello world". Per servire la vera app TanStack Start:

1. Aggiungere l'**adapter / preset Cloudflare** per TanStack Start nel progetto
   (configurazione in `vite.config.ts` / `app.config`), così la build produce un Worker valido.
2. Commit + push → Workers Builds ribuilda in automatico.
3. Verificare che `https://clfoodweb.lodragosalvatore.workers.dev/` mostri il sito reale.
4. Collegare il **dominio custom cl-food.ch** al Worker (Cloudflare → Worker → Custom Domains).
5. Confermare nameserver Hostpoint → Cloudflare (già impostati per il DNS email).

## 🟡 Pagamenti reali (quando si vorrà)

- Oggi: solo **bonifico** (fittizio, nessun pagamento reale). Carta e TWINT mostrati come "presto disponibili".
- Da integrare in futuro: Stripe (carta) + TWINT.

## 🟢 Migliorie opzionali

- Email di sistema multilingua (oggi i template auth sono in italiano).
- Test end-to-end del flusso registrazione Azienda (P.IVA) → preventivo → email.
- Backup periodico del database (export schema + dati).

## ✅ Già completato

Vedi [CHANGELOG.md](./CHANGELOG.md) e [README.md](./README.md) per lo stato completo.
Sistema email, catalogo, auth, funnel, pagine legali, SEO, repository: **fatti**.
