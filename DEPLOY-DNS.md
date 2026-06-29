# Deploy, Hosting, DNS & Repository

## Repository git

Il progetto ha **due remote** allineati (branch `main`):

| Remote | URL | Ruolo |
|---|---|---|
| `origin` | https://github.com/lodragocom/clfoodweb.git | GitHub (pubblico/principale) |
| `lodrago` | https://git.lodrago.ch/salvo/clfoodweb.git | Forgejo privato (self-hosted) |

Push su entrambi:
```
git push origin main
git push lodrago main
```

> Storia: il progetto nasceva su Lovable (ora **dismesso**, tutto migrato). Prima si chiamava `sicily-table`,
> poi rinominato ovunque in **`clfoodweb`**.

## Hosting (Cloudflare)

- Worker Cloudflare **`clfoodweb`** collegato a GitHub via **Workers Builds**.
- **⏳ Stato attuale**: il Worker serve ancora un "Hello world" — manca l'**adapter/preset Cloudflare per TanStack Start**
  perché serva la vera app. Vedi [TODO.md](./TODO.md).
- URL Worker provvisorio: `https://clfoodweb.lodragosalvatore.workers.dev/`
- Account Cloudflare: quello corretto (esiste già tutto).

## Dominio

| Dato | Valore |
|---|---|
| Dominio | **cl-food.ch** |
| Registrar | **Hostpoint.ch** |
| DNS | **Cloudflare** (nameserver Hostpoint → Cloudflare) |
| Stato dominio su Resend | ✅ verificato (record DNS email propagati) |
| Collegamento dominio → Worker | ⏳ da fare dopo l'adapter Cloudflare |

## File ambiente

| File | Contenuto | Git |
|---|---|---|
| `.env` | `VITE_SUPABASE_*` (chiavi **pubbliche**) | versionato |
| `.env.local` | override locale, stesse chiavi pubbliche | **gitignored** (`*.local`) |

⚠️ Nessuna chiave **segreta** (service_role, Resend, SMTP) va nei file frontend o nei `.env`.
Vedi [CREDENZIALI.md](./CREDENZIALI.md).
