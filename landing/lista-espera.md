---
type: Guide
title: Lista de espera
description: Flujo WaitlistForm → API → Supabase en la landing.
---

# Lista de espera

## Frontend (`src/components/WaitlistForm.tsx`)

Componente client reutilizado en el cierre (`#cierre`):

- Estados `idle → submitting → success`.
- Validación local con regex de email.
- `fetch POST /api/subscribe { email }`.
- Error → mensaje en español bajo el formulario. Éxito → panel animado con “Listo, ya estás en la lista”.

## API (`app/api/subscribe/route.ts`)

Ver referencia completa en [POST /api/subscribe](/api/subscribe.md). Resumen:

1. Parsea el JSON (400 si es inválido).
2. Normaliza (`trim().toLowerCase()`), valida largo ≤ 254 y formato (400 “Poné un email válido.”).
3. `getSupabase()` — 500 si faltan las env vars.
4. `insert({ email })` en tabla `waitlist` — 409 si es duplicado (`23505`), 500 ante otro error, `{ success: true }` si ok.

## Supabase (`src/lib/supabase.ts`)

`createClient(url, anonKey)` perezoso y cacheado: el build no rompe si faltan `NEXT_PUBLIC_SUPABASE_URL` / `NEXT_PUBLIC_SUPABASE_ANON_KEY`, pero el endpoint devuelve 500 sin ellas.

Tabla esperada: `waitlist(email)`. Vars de entorno: [Variables de entorno](/primeros-pasos/variables-entorno.md).
