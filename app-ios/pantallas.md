---
type: Guide
title: Pantallas
description: Auth, Home, Perfil, navegación y componentes de la app iOS.
---

# Pantallas

Sin `NavigationStack` global ni onboarding. `RootView` decide: cargando sesión → splash | sin usuario → `AuthView` | con usuario → tabs **Inicio + Perfil**.

## Auth (`Auth/AuthView + AuthViewModel`)

Toggle Entrar/Crear cuenta (nombre y apellido solo en registro), email + password, validación local con errores friendly, delega en `AuthRepository`.

## Home (`Home/HomeView + AppsViewModel + AppLimitsSetupView`)

- Saludo por hora + fecha `es_AR`.
- Card “Esta semana tuviste X min / meta 120 min” con barra de progreso (`weeklyMinutes` desde Supabase).
- Si no hay permiso → prompt que abre el **picker del sistema** (`familyActivityPicker`); si hay → botón **Pausar apps**.
- Flujo: picker → recorte a 2 apps si es plan free → sheet `AppLimitsSetupView` (horas por app, 1–24) → `AppLimitsManager.setLimits`.
- Gate free: tope de 2 apps → `ProUpgradeSheet` (paywall RevenueCat).

## Perfil (`Profile/ProfileView + ProfileViewModel`)

Header con email, card de plan + “Pasar a Pro”, **Tu ritmo** (minutos, racha, apps en pausa), **Contribuciones** (grilla estilo GitHub de 16 semanas), meta semanal, ajustes, gestionar suscripción (`CustomerCenterView` si es Pro), compartir y cerrar sesión. La racha se calcula de las contribuciones del App Group.

## Componentes (`Presentation/Components/`)

`BeOutCard`, `PrimaryButton`, `ScreenHeader`, `SectionTitle`, `StatTile`, `StatusPill`, `AppListCard`, `ContributionGraphView`, `ProUpgradeSheet`.
