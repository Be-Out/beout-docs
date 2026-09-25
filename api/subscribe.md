---
type: Reference
title: POST /api/subscribe
description: Guarda emails en Supabase (tabla waitlist).
---

# POST /api/subscribe

Archivo: `app/api/subscribe/route.ts`.

Guarda el email del formulario de cierre (`#cierre`) en Supabase, tabla `waitlist`.

## Request

```json
{ "email": "usuario@ejemplo.com" }
```

Valida formato y largo (máx. 254), normaliza a minúsculas.

## Responses

- `200 { success: true }` — guardado ok
- `400 { error: "Poné un email válido." }` — email inválido
- `400 { error: "Pedido inválido." }` — body no JSON
- `409 { error: "Este email ya está registrado." }` — duplicado (código Supabase `23505`)
- `500 { error: "Lista de espera no configurada..." }` — faltan env vars
- `500 { error: "Error al guardar el email." }` — error Supabase

## Ejemplo

```bash
curl -X POST http://localhost:3000/api/subscribe \
  -H "Content-Type: application/json" \
  -d '{"email":"usuario@ejemplo.com"}'
```
