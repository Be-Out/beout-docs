---
type: Guide
title: Pantallas
description: Auth, Home, Perfil, navegación y componentes de BeOut Block.
---

# Pantallas

Sin `NavigationStack` global ni onboarding. `RootView` decide: cargando sesión → splash | sin usuario → `AuthView` | con usuario → tabs **Inicio + Perfil**.

## Auth (`Auth/AuthView + AuthViewModel`)

Toggle Entrar/Crear cuenta (nombre y apellido solo en registro), email + password, validación local con errores friendly, delega en `AuthRepository`.

## Home (`Home/HomeView + AppsViewModel + AppLimitsSetupView`)

- Saludo por hora + fecha `es_AR`.
- Card "Esta semana tuviste X min / meta 120 min" con barra de progreso (`weeklyMinutes` desde Supabase).
- Si no hay permiso → prompt que abre el **picker del sistema** (`familyActivityPicker`); si hay → botón **Pausar apps**.
- Flujo: picker → recorte a 2 apps si es plan free → sheet `AppLimitsSetupView` (horas por app, 1–24) → `AppLimitsManager.setLimits`.
- Gate free: tope de 2 apps → `ProUpgradeSheet` (paywall RevenueCat).

## Perfil (`Profile/ProfileView + ProfileViewModel`)

Header con email, card de plan + "Pasar a Pro", **Tu ritmo** (minutos, racha, apps en pausa), **Contribuciones** (grilla estilo GitHub de 16 semanas), meta semanal, ajustes, gestionar suscripción (`CustomerCenterView` si es Pro), compartir y cerrar sesión. La racha se calcula de las contribuciones del App Group.

Cuando las notificaciones están habilitadas, aparece un `NavigationLink` a **NotificationSettingsView**.

## Notificaciones (`Profile/NotificationSettingsView`)

Pantalla de ajustes con cuatro secciones, cada una habilitada independientemente. Toda la configuración se persiste con `@AppStorage`.

| Tipo | Clave AppStorage | Detalle |
|---|---|---|
| Recordatorio de racha | `notif.streakReminder.enabled/hour` | Notificación diaria a la hora configurada (default 20:00) para recordar revisar la racha. |
| Límite próximo | `notif.limitApproaching.enabled/minutes` | Aviso X minutos antes de que se agote el tiempo de pantalla (5/10/15 min). Se aproxima programando a `medianoche + (minutosPermitidos − minutosAviso)`; una implementación precisa requeriría un segundo `DeviceActivityEvent`, lo cual es una limitación de Family Controls en dispositivo. |
| Resumen diario/semanal | `notif.summary.enabled/isWeekly/hour` | Notificación a la hora configurada, diaria o solo los domingos, para invitar a revisar el progreso. |
| Hitos de racha | `notif.milestones.enabled` | Dispara una sola vez al alcanzar 3, 7, 14 y 30 días de racha. Cada hito se marca en `UserDefaults` para no repetirse. |

La lógica de scheduling y cancelación vive en `NotificationManager` (ver Arquitectura). `ProfileViewModel` llama a `NotificationManager.shared.checkAndFireMilestone(streak:)` después de calcular la racha. `AppLimitsManager` llama a `NotificationManager` al guardar límites si el aviso de límite próximo está habilitado.

## Componentes (`Presentation/Components/`)

`BeOutCard`, `PrimaryButton`, `ScreenHeader`, `SectionTitle`, `StatTile`, `StatusPill`, `AppListCard`, `ContributionGraphView`, `ProUpgradeSheet`.
