---
type: Guide
title: Instalación
description: Requisitos, install y scripts npm de BeOut.
---

# Instalación

Repo: [Be-Out/beout](https://github.com/Be-Out/beout) (rama `main`).

## Requisitos

- Node.js 24 (ver `.node-version` del repo)
- npm
- Cuenta Supabase (para la lista de espera)

## Pasos

```bash
git clone https://github.com/Be-Out/beout.git
cd beout
npm install
npm run dev
```

Abrí `http://localhost:3000`.

## Scripts

```bash
npm run dev       # desarrollo (Next.js)
npm run build     # build de producción
npm run start     # sirve el build localmente
npm run lint      # ESLint
npm run preview   # build + serve en runtime Cloudflare Workers (OpenNext)
```

Ver también: [Variables de entorno](/primeros-pasos/variables-entorno.md).
