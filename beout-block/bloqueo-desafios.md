---
type: Guide
title: Bloqueo y desafíos
description: FamilyControls + DeviceActivity, estado de los desafíos y monetización.
---

# Bloqueo y desafíos

El bloqueo usa **FamilyControls + DeviceActivity + ManagedSettings**:

1. El usuario elige apps con el **picker del sistema** (`FamilyActivitySelection`).
2. `AppLimitsManager.setLimits` aplica **shields inmediatos** y programa el monitoreo (`DeviceActivityCenter`) hasta `blockedUntil`.
3. La **extensión** (`beoutDeviceActivityMonitor`) re-aplica el shield al iniciar cada intervalo y lo quita al terminar; al cerrar escribe la **contribución del día** en el App Group.
4. La app, al volver a foreground, **sincroniza** las contribuciones pendientes a Supabase.

## Desafíos de desbloqueo (estado actual)

Solo existe el **modelo** (`ChallengeType`: trivia, escritura a mano, teléfono quieto, pasos). **No hay** ejecución de desafíos ni desbloqueo parcial por minutos: hoy el desbloqueo es por **expiración** (`blockedUntil`) o "Quitar todas".

## Monetización

RevenueCat: `PurchaseManager` + `PaywallView` (free = 2 apps), `CustomerCenterView` para gestionar la suscripción. El plan vive en `profiles.plan`; un webhook lo mantiene actualizado.
