---
type: Guide
title: Esta docs en Pages
description: Cómo publicar esta documentación en Cloudflare Pages.
---

# Esta docs en Pages

Esta documentación (`beout-docs`, docmd) se publica como sitio estático en **Cloudflare Pages**, separada de la landing (que vive en Workers).

## 1. Crear el repo

```bash
cd ~/beout-docs
git init -b main
git add .
git commit -m "docs: inicial beout-docs con docmd"
gh repo create Be-Out/beout-docs --public --source=. --push
```

Si preferís monorepo, podés poner esta carpeta como `docs/` dentro de `beout` y usar **Root directory = `docs`** en Pages.

## 2. Conectar Pages

1. Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
2. Elegí `Be-Out/beout-docs`, rama `main`.
3. Build settings:
   - **Framework preset:** `None`
   - **Build command:** `npm run build`
   - **Build output directory:** `site`
   - **Root directory:** `/` (o `docs` si es monorepo)
   - **Node version:** `20` (Variable `NODE_VERSION=20` o `.node-version` con `20`)
4. **Save and Deploy**.

Cada push a `main` redespliega solo.

## 3. Por qué `_redirects`

`docmd.config.json` usa `"spa": true`, así que la navegación es client-side. El archivo `_redirects` en la raíz:

```
/* /index.html 200
```

se copia a `site/_redirects` en `npm run build` (`cp -f _redirects site/_redirects`) para que Pages resuelva todas las rutas al `index.html`.

## 4. Dominio propio (opcional)

Pages → tu proyecto → **Custom domains** → `docs.beout.app` (o el dominio que uses):

1. Agregalo y seguí el asistente (crea el CNAME solo).
2. Cambiá `"url"` en `docmd.config.json` a `https://docs.beout.app`.
3. Rebuild para regenerar `sitemap.xml` y `robots.txt`.

## 5. Flujo de trabajo

```bash
npm run dev    # http://localhost:3000 con hot reload
npm run build  # genera site/ + okf bundle
```

Reglas (igual que Mandioca): todo `.md` no reservado lleva frontmatter con `type` (`Guide` / `Reference` / `Concept`), H1 primero, enlaces internos con rutas absolutas del bundle (`/api/subscribe.md`), imágenes en `assets/images/<doc>/`.
