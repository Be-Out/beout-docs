---
type: Guide
title: Variables de entorno
description: Supabase y Node 24 para correr BeOut.
---

# Variables de entorno

La landing necesita estas dos variables para la lista de espera:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

## Local

Crear `.env.local` en la raíz de `beout/`:

```bash
NEXT_PUBLIC_SUPABASE_URL=https://tu-proyecto.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=tu-anon-key
```

El cliente Supabase es perezoso (`src/lib/supabase.ts` vía `getSupabase()`), así que el build no rompe si faltan, pero `/api/subscribe` devuelve 500 sin ellas.

## Producción (Workers)

En Cloudflare Dashboard → Workers → `beout` → Settings → Variables:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `NODE_VERSION=24`

Tabla esperada en Supabase: `waitlist(email)`.
