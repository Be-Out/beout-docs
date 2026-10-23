---
type: Reference
title: Supabase de BeOut Move
description: Tablas, migraciones y Edge Functions del proyecto Supabase de BeOut Move.
---

# Supabase de BeOut Move

BeOut Move usa un proyecto Supabase **separado** del de BeOut Block:

- Proyecto: `wntgwkhuvmalagdakmpg`
- MCP: `supabase-move` (no usar el MCP `supabase`, que es el de BeOut Block)

## Migraciones (`supabase/migrations/`)

| Archivo | Qué crea |
|---|---|
| `0001_profiles` | Tabla de perfiles de usuario |
| `0002_sync_and_friends` | Sync de actividad y sistema de amigos |
| `0003_routes` | Recorridos guardados |

Cambios de esquema: siempre como archivo nuevo en `supabase/migrations/`; no aplicar en la base sin confirmación explícita.

## Edge Functions (`supabase/functions/`)

- **delete-account** — elimina la cuenta del usuario y sus datos asociados.

## Reglas

- Todas las tablas con datos de usuario deben tener RLS.
- La `service_role` key nunca va en la app; la publishable key (anon key) es pública por diseño.
