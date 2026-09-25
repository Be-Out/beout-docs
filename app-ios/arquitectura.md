---
type: Reference
title: Arquitectura de la app iOS
description: Capas Clean + MVVM, managers Core, targets Xcode y requisitos de build.
---

# Arquitectura de la app iOS

Código local: `Escritorio/allProject/beout-swift/beout-swift-app`. Swift/SwiftUI, Xcode, patrón **Clean-ish + MVVM por pantalla** (sin framework de DI).

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
supabase/migrations/        # SQL versionado (0001_daily_contributions)
beoutTests / beoutUITests
```

## Core (singletons)

- **ScreenTimeManager** — permiso FamilyControls y shields manuales (`applyShields()` / `removeShields()` sobre `ManagedSettingsStore`), persiste el `FamilyActivitySelection` en el App Group.
- **AppLimitsManager** — bloqueos temporarios: `setLimits(_)` pone shields, programa el desbloqueo y abre sesión; `removeAllLimits()` limpia y cierra sesión.
- **PurchaseManager** — wrapper mínimo de RevenueCat: `isPro` espeja `profiles.plan`, `restorePurchases()`.
- **Supabase** — cliente global (`SupabaseClient`) al proyecto `fygzwsegogzqgerjazet`.
- **Theme** — design system: colores tierra + tipografía rounded.

## Targets y capabilities

Targets: `beout` (app, bundle `BeOut.beout`), `beoutDeviceActivityMonitor` (extensión `com.apple.deviceactivity.monitor-extension`), `beoutTests`, `beoutUITests`. `MARKETING_VERSION 1.0`, deployment target iOS 26.

Capabilities: **Family Controls** + **App Group** `group.BeOut.beout` (en app y extensión; es el único canal entre ambas: JSON en el grupo compartido, sin tipos Swift compartidos).

## Requisitos para compilar

- Xcode con iOS 26 SDK.
- El Simulador compila y corre hasta el login, pero FamilyControls/DeviceActivity **no funcionan** ahí: bloqueo real solo en **iPhone físico**.
- Requiere **Apple Developer Program pago + entitlement de Family Controls aprobado** (con cuenta gratuita falla el provisioning).
