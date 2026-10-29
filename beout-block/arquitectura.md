---
type: Reference
title: Arquitectura de BeOut Block
description: Capas Clean + MVVM, managers Core, targets Xcode y requisitos de build.
---

# Arquitectura de BeOut Block

Repo: `Be-Out/beout-swift-app` (local: `BEOUT_BLOCK`, default `../beout-block`).
Proyecto Xcode `beout.xcodeproj`, iOS 26.0 mínimo, Swift 5 language mode, SwiftUI.
Notas de producto y decisiones recientes en `app.md` en la raíz del repo.

## Capas

```
beout/
  beoutApp.swift            # entry: crea AppState, configura PurchaseManager, sync al foreground
  App/                      # AppState (@MainActor, sesión, sync) + RootView (navegación raíz)
  Core/                     # singletons transversales (ver abajo)
  Domain/                   # entidades puras + AuthRepositoryProtocol
  Data/                     # adaptadores Supabase + Mock/MockData para UI
  Presentation/             # Auth, Home, Profile (+ ViewModels) + Components/
  Assets.xcassets / beout.entitlements
beoutDeviceActivityMonitor/ # extensión DeviceActivityMonitor (comunica vía App Group)
supabase/migrations/        # SQL versionado
beoutTests / beoutUITests
```

Patrón: **MVVM** con `ObservableObject` + `@Published`, ViewModels `@MainActor`, async/await. Los repositorios hablan con Supabase vía el cliente global `supabase` (`Core/Supabase.swift`).

## Core (singletons)

- **ScreenTimeManager** — permiso FamilyControls y shields manuales (`applyShields()` / `removeShields()` sobre `ManagedSettingsStore`), persiste el `FamilyActivitySelection` en el App Group.
- **AppLimitsManager** — bloqueos temporarios: `setLimits(_)` pone shields, programa el desbloqueo y abre sesión; `removeAllLimits()` limpia y cierra sesión.
- **PurchaseManager** — wrapper mínimo de RevenueCat: `isPro` espeja `profiles.plan`, `restorePurchases()`.
- **Supabase** — cliente global (`SupabaseClient`) al proyecto `fygzwsegogzqgerjazet`.
- **NotificationManager** — gestiona el ciclo de vida de las cuatro notificaciones configurables (recordatorio de racha, límite próximo, resumen diario/semanal, hitos de racha): programa, cancela y valida permisos con `UNUserNotificationCenter`. Expone `checkAndFireMilestone(streak:)` para hitos y `scheduleAll()` / `cancelAll()` para reconstruir el calendario al cambiar ajustes.
- **Theme** — design system: colores tierra + tipografía rounded.

## Targets y capabilities

| Target | Qué hace |
|---|---|
| `beout` | App principal (bundle `BeOut.beout`) |
| `beoutDeviceActivityMonitor` | Extensión `DeviceActivityMonitor`: recibe eventos de umbral por app y registra en el App Group qué apps quedaron dentro del límite |
| `beoutShieldConfiguration` | Personaliza la pantalla de bloqueo (shield) |
| `beoutShieldAction` | Maneja los botones del shield |
| `beoutTests` / `beoutUITests` | Tests (casi vacíos) |

Capabilities: **Family Controls** + **App Group** `group.BeOut.beout` (en app y extensión). Es el único canal entre ambas: JSON en el grupo compartido, sin tipos Swift compartidos. Las extensiones no pueden usar Supabase ni red de forma confiable; escriben local y la app sincroniza al volver a primer plano.

## Requisitos para compilar

- Xcode con iOS 26 SDK.
- El Simulador compila y corre hasta el login, pero FamilyControls/DeviceActivity **no funcionan** ahí: bloqueo real solo en **iPhone físico**.
- Requiere **Apple Developer Program pago + entitlement de Family Controls aprobado**.
- Build de verificación: `xcodebuild -project beout.xcodeproj -scheme beout -destination 'platform=iOS Simulator,name=iPhone 17' build`.

## Gotchas

- **Family Controls no corre en el Simulador.** Bloqueo y eventos de `DeviceActivity` solo se prueban en iPhone físico.
- `ApplicationToken` es opaco: no se puede saber el nombre de la app en código. Por eso `AppLimit` usa un UUID propio; para mostrar la app usar `Label(token)` de FamilyControls.
- Las extensiones tienen límites de memoria muy bajos (~6 MB en DeviceActivityMonitor): nada de dependencias pesadas ahí.
- La publishable key de Supabase está hardcodeada en `Core/Supabase.swift` (es pública por diseño; la seguridad depende de RLS). Nunca poner la `service_role` key en la app.
