---
type: Reference
title: Estructura del proyecto
description: Qué hay en app/, src/ y public/ de BeOut.
---

# Estructura del proyecto

```
app/
  layout.tsx                # layout raíz + metadata
  page.tsx                  # armado de la landing: Nav, Hero, secciones, Footer
  privacidad/page.tsx       # aviso de privacidad de la lista de espera
  terminos/page.tsx         # términos simples
  api/subscribe/route.ts    # POST /api/subscribe: guarda el email en Supabase (waitlist)
src/
  components/
    ComoFuncionaPinned.tsx  # sección "Cómo funciona" con scroll pineado (GSAP)
    Reveal.tsx              # wrapper que anima hijos al entrar en viewport
    SmoothScroll.tsx        # setup de Lenis sincronizado con GSAP
    icons.tsx               # íconos SVG propios
    ui/                     # componentes shadcn/ui
  lib/
    supabase.ts             # cliente Supabase perezoso vía getSupabase()
    utils.ts                # helper cn() (clsx + tailwind-merge)
  index.css                 # theming y paleta tierra/marrón
  App.css                   # animaciones de reveal del hero
public/                     # favicon, marca e imágenes
public/app/                 # capturas reales de la app
BeOut_landing_brief.md      # brief de diseño y copy
```

Paleta: tonos tierra/marrón (`--cream`, `--sand`, `--stone`, `--moss`, `--forest`, `--bark` en `src/index.css`). No reemplazar sin pedido explícito.
