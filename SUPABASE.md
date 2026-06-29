# Supabase

## Progetto

| Dato | Valore |
|---|---|
| Project Ref | `zgnyjbxvxoupdaeuohtm` |
| URL | `https://zgnyjbxvxoupdaeuohtm.supabase.co` |
| Dashboard | https://supabase.com/dashboard/project/zgnyjbxvxoupdaeuohtm |
| Org | lodragocom's Org |
| Anon key (pubblica) | in `.env` come `VITE_SUPABASE_PUBLISHABLE_KEY` — è pubblica per definizione |
| service_role key (SEGRETA) | ⚠️ MAI nel frontend / mai in repo — vedi [CREDENZIALI.md](./CREDENZIALI.md) |

> Nota: esisteva un progetto sbagliato `diwynckrjwsjnlmowxbi` (vecchio/residuo). Quello **corretto** è `zgnyjbxvxoupdaeuohtm`.

## Tabelle (schema `public`)

| Tabella | Scopo |
|---|---|
| `categories` | 12 categorie prodotto |
| `products` | 60 prodotti reali (IT/DE/FR, prezzi privato/azienda, `image_url='/products/<slug>.jpg'`) |
| `profiles` | profilo utente (account_type, full_name, email, company_name, vat_number) |
| `orders` | ordini/preventivi (vedi colonne sotto) |
| `user_roles` | ruoli (enum `app_role`: admin, user) |

### Colonne `orders` (principali)
`id` (uuid, default gen_random_uuid) · `user_id` (uuid, **NOT NULL**, FK auth.users) ·
`account_type` (text) · `status` (text, default `preventivo`) · `items` (jsonb) ·
`subtotal` (numeric) · `currency` (text, default `CHF`) · `company_name` · `vat_number` ·
`notes` · `order_number` · `payment_method` · `created_at`.

## Funzioni / RPC (`public`)

| Funzione | Scopo |
|---|---|
| `get_products` | **Gating prezzi** — SECURITY DEFINER, restituisce i prodotti con prezzi solo se l'utente è autenticato e secondo tipo account |
| `handle_new_user` | Trigger su `auth.users` insert → crea `profiles`; assegna ruolo **admin SOLO a `info@cl-food.ch`**; `exception when others then return new` (non blocca mai la registrazione) |
| `has_role(uuid, app_role)` / `is_admin()` | Helper ruoli (SECURITY DEFINER, evitano ricorsione RLS) |
| `generate_order_number` | Genera numero ordine progressivo |
| `update_orders_updated_at` | Trigger updated_at |

## Trigger

- `on_auth_user_created` → `handle_new_user()` (after insert su `auth.users`).

## RLS (Row Level Security)

- `user_roles`: utente legge i propri ruoli; admin legge/scrive tutti.
- `products` / `categories`: admin (via `is_admin()`) gestisce in scrittura; lettura prezzi via `get_products`.
- Admin assegnato automaticamente solo a **info@cl-food.ch**.

## Storage

| Bucket | Pubblico |
|---|---|
| `product-images` | No (privato) |

## Edge Functions

| Funzione | Stato | Note |
|---|---|---|
| `send-order-email` | ✅ ACTIVE (v1) | `verify_jwt = false`. Invia 2 email via Resend alla conferma ordine. Dettagli in [EMAIL.md](./EMAIL.md) |

## Auth — impostazioni note

- **Conferma email**: disattivata (`mailer_autoconfirm = true`) → registrazione immediata.
- Metadata registrazione: `account_type`, `company_name`, `vat_number` (in `raw_user_meta_data`).
- SMTP custom abilitato (Resend) — vedi [EMAIL.md](./EMAIL.md).
- Admin attuale: **info@cl-food.ch** (user id `adc3199a-c666-4cfb-bf17-9281e10c52bd`).

## Come aggiornare lo schema

Lo schema completo riproducibile è in `supabase-setup.sql` (root del progetto): tabelle, RPC, trigger,
policy, bucket + 12 categorie + 60 prodotti. Le 7 migration sono in `supabase/migrations/`.
