---
type: Reference
title: Dominio y backend
description: Entidades, repositorios y tablas Supabase de la app iOS.
---

# Dominio y backend

## Entidades (`Domain/Entities/`)

- **AppLimit** — límite diario por app: `id`, `tokenData` (token de FamilyControls serializado; no guarda el nombre real por privacidad), `blockHours`, `blockedUntil` (+ `remainingText`).
- **BlockedApp** (legacy/mock) — `id`, `name`, `blockedUntil`, `challenge: ChallengeType`.
- **ChallengeType** — `quiz` (Trivia), `handwrite` (Escritura), `phoneDown` (Teléfono quieto), `steps` (Pasos), cada uno con minutos ganados (15/20/10/30).
- **UserProfile** (mock) — `email`, `plan` (free/pro), `totalMinutes`, `streak`, `weeklyGoal`.
- **AuthUser** — `id`, `email`, `first_name`, `last_name`, `isPro` (vía `AuthRepositoryProtocol`).

## Repositorios (`Data/`)

- **SupabaseAuthRepository** — signUp/signIn/signOut con email+pass; lee `profiles` para armar el `AuthUser` (`isPro = plan == "pro"`).
- **BlockSessionRepository** — sesiones de bloqueo (`startSession` / `endSession` / `closeOrphanedSessions`, `weeklyMinutes()`, `hasActiveSession()`).
- **ContributionsRepository** — lee las contribuciones del App Group y hace `upsert` a Supabase (`onConflict=user_id,app_limit_id,date`).
- **MockData** — datos falsos para desarrollar la UI sin backend.

## Tablas Supabase (proyecto `fygzwsegogzqgerjazet`)

- **profiles** (`id`, `first_name`, `last_name`, `plan`) — el plan define `isPro`. Sin migración en el repo.
- **block_sessions** (`id`, `started_at`, `ended_at`) — minutos por semana. Sin migración en el repo.
- **daily_contributions** (`user_id`, `app_limit_id`, `date`, `created_at`, PK compuesta) — racha estilo GitHub: 1 contribución por app y día si no excede el límite. Migración `0001_daily_contributions.sql` + RLS (cada usuario gestiona las suyas).
