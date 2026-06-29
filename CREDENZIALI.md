# Inventario credenziali

> 🔒 **Questo file NON contiene i valori segreti.** È una mappa: per ogni credenziale trovi
> a cosa serve, **dove è salvata** e come rigenerarla. I valori veri restano nei rispettivi servizi
> (Supabase, Resend, Cloudflare…). Se vuoi tenerne copia in locale, usa il file `segreti.local`
> (escluso da git tramite `.gitignore`) — compilandolo **tu**.

## Chiavi PUBBLICHE (ok in repo)

| Nome | Dove | Note |
|---|---|---|
| `VITE_SUPABASE_URL` | `.env` / `.env.local` | `https://zgnyjbxvxoupdaeuohtm.supabase.co` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` (anon) | `.env` / `.env.local` | Chiave anon JWT — pubblica per definizione, protetta da RLS |

## Chiavi SEGRETE (MAI in repo, MAI nel frontend)

| Nome | A cosa serve | Dove è salvata (fonte di verità) | Come rigenerarla |
|---|---|---|---|
| **Supabase `service_role` key** | Accesso admin al DB (bypassa RLS) | Supabase → Project Settings → API. Iniettata in automatico nelle Edge Functions | Supabase → Settings → API → "Reset" / rigenera |
| **Resend API key** (`re_...`) | Invio email (SMTP auth **e** funzione ordini) | 1) Supabase → Auth → SMTP (password) · 2) Supabase → Edge Functions → Secrets (`RESEND_API_KEY`) | Resend → API Keys → Create / Revoke |
| **Supabase DB password** | Accesso diretto Postgres | Supabase → Project Settings → Database | Supabase → Settings → Database → Reset password |
| **Cloudflare API token** | Deploy/gestione Worker e DNS | Cloudflare → My Profile → API Tokens | Cloudflare → API Tokens → Roll/Create |
| **Hostpoint login** | Gestione dominio cl-food.ch | Account Hostpoint.ch | Reset da Hostpoint |
| **GitHub** (`origin`) | Repo codice | Account GitHub `lodragocom` | — |
| **Forgejo git.lodrago.ch** (`lodrago`) | Repo privato | Server self-hosted | — |
| **Supabase dashboard token** | Usato per gestire config via Management API | `localStorage['supabase.dashboard.auth.token']` di una sessione loggata | Si rinnova al login dashboard |

## Account chiave

| Servizio | Account / identità |
|---|---|
| Admin del sito (ruolo admin auto) | **info@cl-food.ch** |
| Email aziendale / notifiche ordini | info@cl-food.ch |
| Mittente email sistema | noreply@cl-food.ch |

## Regole d'oro

1. I valori segreti **non si scrivono** in file versionati.
2. Nel frontend solo chiavi `VITE_*` (pubbliche).
3. La `service_role` la usano solo le Edge Functions (la ricevono in automatico).
4. Se una chiave finisce per sbaglio in git → **rigenerala subito** (revoke + nuova).
