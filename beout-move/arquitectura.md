---
type: Reference
title: Arquitectura de BeOut Move
description: Stack, targets, capas y gotchas de la app de pasos y rachas.
---

# Arquitectura de BeOut Move

Repo: `Be-Out/beout-move-swift-app` (local: `BEOUT_MOVE`, default `../beout-move-swift-app`).
SwiftUI, iOS 17+. Leer el `README.md` del repo para el detalle de funcionalidades.

## Stack

- **SwiftUI**, iOS 17+
- **HealthKit** — pasos, distancia, calorías
- **Supabase** — auth, sync de actividad, amigos (proyecto separado del de BeOut Block)
- **XcodeGen** — el proyecto Xcode se genera desde `project.yml`. No editar `BeOutMove.xcodeproj` a mano: cambios de bundle id, capabilities o targets van en `project.yml` y se regenera con `xcodegen generate`.
- **Swift Testing** para tests (no XCTest), carpeta `BeOutMoveTests/`.

## Targets

| Target | Qué hace |
|---|---|
| `BeOutMove` | App iOS principal |
| `BeOutMoveWidgetExtension` | Widgets de inicio y pantalla de bloqueo |
| `BeOutMoveWatch` | App standalone de watchOS (reutiliza HealthKitManager, StepsStats, Theme) |
| `BeOutMoveTests` | Unit tests |

## Capas (`BeOutMove/`)

```
App/                # entrada, AppState, RootView
Core/               # Theme, cliente Supabase, HealthKitManager, GoalManager,
                    # UnitsManager, NotificationsManager, StreakStore, StepsStats,
                    # RouteTrackingManager, RouteHistoryStore,
                    # SharedDefaults (App Group group.BeOut.beoutmove),
                    # WatchConnectivityManager, intents de Siri
Domain/             # entidades y protocolos de repositorio
Data/Supabase/      # implementaciones sobre Supabase
Presentation/       # Auth, Home, Profile, Friends, Routes, Onboarding, Components
```

## Gotchas

- El objetivo diario llega al Watch por **WatchConnectivity** (los App Groups no sincronizan entre dispositivos); pasos y racha del Watch salen de HealthKit directamente.
- Widget y app comparten estado por App Group (`SharedDefaults`); tocar `SharedDefaults` afecta a ambos.
- `StreakStore` no tiene test a propósito: el récord solo sube.
- Privacy manifest: si se usa otra API "required reason", declararla en `PrivacyInfo.xcprivacy` de la app y del widget.
- Aplican las mismas reglas de UI que en BeOut Block: componentes nativos primero, reutilizar `Theme`, soportar modo claro y oscuro.
