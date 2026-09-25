---
type: Guide
title: Secciones de la página
description: Nav, Hero, problema, cómo funciona, desafíos, planes, cierre, FAQ y footer.
---

# Secciones de la página

Todo se compone en `app/page.tsx` (`App()`), en este orden:

1. **Nav** — header pill sticky con logo + CTA “Sumate” (`#cierre`).
2. **Hero** — badge “Para iPhone”, H1 “No te prohibimos el celular. Le ponemos un poco de *fricción*.”, CTA primario `#cierre` + secundario `#como-funciona`.
3. **Problema** — fondo `sand`: scroll automático y límites nativos que se saltean fácil.
4. **Cómo funciona** (`#como-funciona`) — sección pineada a pantalla completa (GSAP), 4 pasos con capturas reales de `public/app/*.webp`: elegís apps → se bloquean con pantalla nativa iOS → desafío si querés entrar antes → desbloqueo temporal.
5. **Desafíos** (`#desafios`) — 4 cards: quiz, escribir frase a mano, teléfono boca abajo, caminar pasos.
6. **Por qué es distinto** — bloque oscuro: nativos = “un botón para saltearlos” vs BeOut = “desafío breve que te hace parar”.
7. **Planes** (`#planes`) — Gratis ($0, 2 apps) vs Completo (todas las apps/desafíos, mensual/anual/lifetime).
8. **Cierre** (`#cierre`) — “Cuando quieras, probá *BeOut*.” + `<WaitlistForm/>`.
9. **FAQ** (`#faq`) — 4 `<details>`: ¿se puede saltear?, ¿el desafío es siempre igual?, ¿desbloquear todo?, ¿cuántas apps gratis?
10. **Footer** — marca, columnas Producto/Contacto, links a `/privacidad` y `/terminos`.

Detalles de tono, paleta y copy: [Brief de landing](/diseno/brief-landing.md).
