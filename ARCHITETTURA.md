# Architettura & Stack

## Stack tecnologico

| Livello | Tecnologia |
|---|---|
| Framework | **TanStack Start** (React SSR) — package name `tanstack_start_ts` |
| Build | **Vite** + `@tanstack/router-plugin` + `vite-tsconfig-paths` |
| Routing | **@tanstack/react-router** (file-based in `src/routes`) |
| Data fetching | **@tanstack/react-query** |
| Stile | **Tailwind CSS** (`@tailwindcss/vite`) + `tailwind-merge` + `tw-animate-css` |
| UI kit | **shadcn/ui** (Radix UI primitives) — config in `components.json` |
| Form | **react-hook-form** + **zod** + `@hookform/resolvers` |
| Animazioni | **framer-motion** |
| Toast | **sonner** |
| Icone | **lucide-react** |
| Grafici | **recharts** (admin) |
| Backend / DB / Auth | **Supabase** (`@supabase/supabase-js`) |
| Package manager | **bun** (`bun.lock`, `bunfig.toml`) — presente anche `package-lock.json` |

## Struttura cartelle (root del progetto)

```
clfoodweb/
├── src/
│   ├── routes/              # pagine (file-based routing)
│   ├── integrations/supabase/client.ts   # client Supabase (legge VITE_SUPABASE_*)
│   ├── lib/                 # auth.tsx, cart, i18n, util
│   └── components/          # componenti UI (shadcn + custom)
├── public/
│   └── products/            # immagini prodotti (<slug>.jpg)
├── supabase/
│   ├── functions/send-order-email/   # Edge Function email ordini
│   └── migrations/          # 7 migration SQL
├── supabase-setup.sql       # schema completo + 60 prodotti (script di setup)
├── _setup-catalogo.sql      # catalogo
├── email-templates-supabase.md   # i 6 template email (riferimento)
├── .env                     # VITE_SUPABASE_* (chiavi PUBBLICHE) — versionato
├── .env.local               # idem, override locale — gitignored
├── package.json
└── vite.config.ts
```

## Pagine / route (`src/routes`)

| Route | Pagina |
|---|---|
| `index.tsx` | Home / landing |
| `catalogo.tsx` | Catalogo con filtri e ricerca |
| `prodotto.$slug.tsx` | Scheda prodotto |
| `pasta.tsx` · `olio.tsx` · `dolci.tsx` · `regali.tsx` | Categorie / vetrine |
| `registrati.tsx` | Registrazione (toggle Privato / Azienda con P.IVA) |
| `accedi.tsx` | Login |
| `account.tsx` | Area utente |
| `admin.tsx` | Dashboard admin (gestione catalogo) |
| `preventivo.tsx` | Carrello / preventivo |
| `checkout.tsx` | Checkout (bonifico/carta/TWINT) → invoca `send-order-email` |
| `ordine-confermato.tsx` | Conferma ordine |
| `chi-siamo.tsx` · `contatti.tsx` · `news.tsx` | Pagine istituzionali |
| `disclaimer · agb · nutzungsbedingungen · datenschutz · cookies · impressum` | Pagine legali (IT/DE/FR) |
| `sitemap[.]xml.ts` | Sitemap dinamica (SEO) |

## Note tecniche

- `routeTree.gen.ts` è **gitignored** (generato automaticamente da TanStack Router).
- Il client Supabase legge `VITE_SUPABASE_URL` e `VITE_SUPABASE_PUBLISHABLE_KEY` (fallback su `process.env.SUPABASE_*`).
- Gating prezzi gestito lato DB con la RPC `get_products` (SECURITY DEFINER) — vedi [SUPABASE.md](./SUPABASE.md).
