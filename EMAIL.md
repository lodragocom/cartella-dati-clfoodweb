# Sistema Email CL-FOOD

Provider: **Resend** (sia SMTP per le email di sistema, sia API HTTP per le email ordini).
Dominio mittente: **cl-food.ch** (✅ verificato su Resend, DNS su Cloudflare).

---

## 1) Email di sistema / auth (6 template brandizzati)

Configurati in **Supabase → Authentication → Emails → Templates**. Tutti con logo `cl-food lombardo`
(verde `#4F5D2A` / oro `#E0A52C`), pulsante e footer aziendale.

| Template | Campo subject | Subject impostato | Variabile link |
|---|---|---|---|
| Conferma registrazione | `mailer_subjects_confirmation` | "Conferma la tua registrazione — CL-FOOD" | `{{ .ConfirmationURL }}` |
| Magic link / accesso | `mailer_subjects_magic_link` | "Il tuo link di accesso — CL-FOOD" | `{{ .ConfirmationURL }}` |
| Reset password | `mailer_subjects_recovery` | "Reimposta la tua password — CL-FOOD" | `{{ .ConfirmationURL }}` |
| Cambio email | `mailer_subjects_email_change` | "Conferma il tuo nuovo indirizzo email — CL-FOOD" | `{{ .ConfirmationURL }}` |
| Invito | `mailer_subjects_invite` | "Sei stato invitato su CL-FOOD" | `{{ .ConfirmationURL }}` |
| Codice verifica | `mailer_subjects_reauthentication` | "Il tuo codice di verifica — CL-FOOD" | `{{ .Token }}` |

Il sorgente HTML dei template è anche in `email-templates-supabase.md` (root del progetto).

### Metodo per aggiornarli senza UI
I template si aggiornano via **Management API** con il token di sessione della dashboard:
- Token: `JSON.parse(localStorage.getItem('supabase.dashboard.auth.token')).access_token` (da una scheda supabase.com).
- `PATCH https://api.supabase.com/v1/projects/zgnyjbxvxoupdaeuohtm/config/auth`
  con header `Authorization: Bearer <token>`, body = JSON dei campi
  `mailer_subjects_<nome>` e `mailer_templates_<nome>_content`.
- (Utile perché l'editor Monaco della dashboard si renderizza solo se la scheda è in primo piano.)

---

## 2) SMTP Resend (consegna email di sistema)

**Supabase → Authentication → Emails → SMTP Settings** (✅ già configurato):

| Campo | Valore |
|---|---|
| Host | `smtp.resend.com` |
| Port | `465` |
| Username | `resend` |
| Password | la **API key Resend** (`re_...`) — SEGRETA, vedi CREDENZIALI.md |
| Sender email | `noreply@cl-food.ch` |
| Sender name | `CL-FOOD` |

---

## 3) Email transazionali ordini/preventivi (Edge Function)

Funzione **`send-order-email`** (`supabase/functions/send-order-email/index.ts`), ✅ deployata e ATTIVA (v1).

**Cosa fa**: alla conferma di un ordine (`checkout.tsx` la invoca in modo non bloccante) invia 2 email via Resend:
1. **Notifica interna** → `ORDER_NOTIFY_EMAIL` (info@cl-food.ch)
2. **Conferma al cliente** → email del profilo

Mittente = `ORDER_FROM_EMAIL`. Legge l'ordine dalla tabella `orders` con la service_role (auto-iniettata).

### Secret / variabili della funzione (Supabase → Edge Functions → Secrets)

| Nome | Tipo | Valore / nota |
|---|---|---|
| `RESEND_API_KEY` | 🔒 SEGRETO | API key Resend (stessa dell'SMTP). Senza, la funzione è attiva ma non invia |
| `ORDER_FROM_EMAIL` | config | `CL-FOOD <noreply@cl-food.ch>` |
| `ORDER_NOTIFY_EMAIL` | config | `info@cl-food.ch` |
| `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, ecc. | auto | iniettati da Supabase |

### Deploy della funzione (via Management API)
`POST https://api.supabase.com/v1/projects/zgnyjbxvxoupdaeuohtm/functions/deploy?slug=send-order-email`
multipart con part `metadata` (JSON `{entrypoint_path:"index.ts", name, verify_jwt:false}`) + part `file` (index.ts).
In alternativa: `npx supabase functions deploy send-order-email --project-ref zgnyjbxvxoupdaeuohtm`.

---

## Test eseguito (17 giugno 2026) ✅

Creato un ordine di test sull'utente info@cl-food.ch → invocata la funzione →
risposta `{"ok":true}` (Resend ha accettato entrambe le email) → ordine di test cancellato.
Esito: **sistema email pienamente funzionante**.
