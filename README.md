---
type: Guide
title: Guía de trabajo
description: Cómo editar y mantener esta documentación localmente.
---

# Guía de trabajo

Documentación técnica de **BeOut** con [docmd](https://docmd.io). Repo principal: [Be-Out/beout](https://github.com/Be-Out/beout).

## Requisitos

- Node.js 20+ (ver `.node-version`)
- npm

## Instalación

```bash
npm install
```

## Desarrollo

```bash
npm run dev
```

Levanta en `http://localhost:3000` con recarga en caliente.

## Editar documentos

Cada documento es Markdown con frontmatter YAML (especificación [OKF v0.1](https://okf.md/spec)):

```markdown
---
type: Guide
title: Nombre del documento
description: Resumen en una línea.
---
```

- **`type` obligatorio:** `Guide`, `Reference`, `Concept`.
- **`title` y `description`** recomendados.
- H1 como primer encabezado.
- **Imágenes:** `assets/images/<documento>/` referenciadas como `/assets/images/<documento>/imagen.png`.
- **Enlaces:** rutas absolutas del bundle, ej. `/api/subscribe.md`.
- Secciones nuevas: crear `nueva-seccion/index.md` (sin frontmatter, lista de links), registrar en `navigation` de `docmd.config.json`.

## Build

```bash
npm run build
```

Genera `site/` (+ copia `_redirects` para Pages SPA). Ver `site/okf/_meta/lint-report.txt`.

## Deploy

Ver [Esta docs en Pages](/deploy/cloudflare-pages-docs.md).
