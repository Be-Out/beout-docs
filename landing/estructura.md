---
type: Reference
title: Estructura y stack de la landing
description: Next.js, componentes, assets y archivos de la landing BeOut.
---

# Estructura y stack de la landing

Repo: [Be-Out/beout](https://github.com/Be-Out/beout) (local: `BEOUT_LANDING`, default `../beout-landing`).

## Stack

- **Next.js 16** (App Router) + **React 19** + **TypeScript**
- **Tailwind CSS v4** + tokens propios en `src/index.css`
- **GSAP + ScrollTrigger** (reveals, sección pineada), **Lenis** (smooth scroll), **Motion** (micro-interacciones)
- **shadcn/ui** sobre `@base-ui/react` (`src/components/ui/button.tsx`)
- **Supabase** (`@supabase/supabase-js`) para la lista de espera
- **OpenNext** (`@opennextjs/cloudflare` + `wrangler`) para el deploy en Workers

## Árbol

```
app/
  layout.tsx              # fuentes Lora + Work Sans, metadata OG/Twitter, JSON-LD
  page.tsx                # landing single-page ('use client'): Nav → Hero → … → Footer
  privacidad/page.tsx     # aviso de privacidad de la lista de espera
  terminos/page.tsx       # términos simples
  not-found.tsx           # 404 en tono de marca
  robots.ts / sitemap.ts  # SEO, usan siteUrl
  api/subscribe/route.ts  # POST /api/subscribe (ver lista de espera)
src/
  components/
    ComoFuncionaPinned.tsx  # "Cómo funciona" pineada con timeline GSAP
    Reveal.tsx              # fade-up al entrar en viewport
    SmoothScroll.tsx        # Lenis sincronizado con ScrollTrigger
    WaitlistForm.tsx        # formulario de email de la lista de espera
    icons.tsx               # íconos SVG propios (sol, pasos, lápiz, quiz, teléfono, flecha)
    ui/button.tsx           # botón shadcn con variants
  lib/
    site.ts                 # siteUrl (env o workers.dev)
    supabase.ts             # cliente lazy vía getSupabase()
    utils.ts                # cn() (clsx + tailwind-merge)
  index.css                 # paleta tierra + fuentes + base shadcn
  App.css                   # animación .reveal del hero
public/
  favicon.svg, apple-touch-icon.png, og-image.png (1200×630)
  beout-icono.webp (nav/hero), beout-marca.webp (footer/éxito)
  app/*.webp                # 4 capturas reales de la app iOS
```

## Config de deploy

- `next.config.ts` (`images.unoptimized: true`), `open-next.config.ts`, `wrangler.jsonc`
- `public/_headers` (cache largo para `/_next/static/*`)
- Scripts: `preview`, `deploy`, `cf:build` / `cf:deploy` (los usa el CI)

Ver [Landing en Workers](/deploy/cloudflare-workers.md) y [Secciones de la página](/landing/secciones.md).
