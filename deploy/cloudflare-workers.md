---
type: Guide
title: Landing en Workers
description: Cómo se despliega Be-Out/beout en Cloudflare Workers.
---

# Landing en Workers

La landing corre como Worker (`beout.<cuenta>.workers.dev`), no en Vercel ni Netlify.

- Adapter **OpenNext** (`@opennextjs/cloudflare` + `wrangler`). `next-on-pages` está deprecado.
- Archivos: `next.config.ts`, `wrangler.jsonc`, `open-next.config.ts`, `public/_headers`, `.node-version` (24).
- Scripts: `preview`, `deploy`, `upload`, `cf-typegen`, `cf:build` / `cf:deploy` (los usa el CI).
- Dashboard (Workers → `beout` → Settings → Builds): Build `npm run cf:build`, Deploy `npm run cf:deploy`, rama `main`, variables `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `NODE_VERSION=24`.
- Verificación: `next build` (`/` estática, `/api/subscribe` dinámica) y `npm run preview`.

Troubleshooting del README: si responde “Hello world”, promover la versión del build en Deployments → Version History. Si HTTPS falla con `SSL_ERROR_NO_CIPHER_OVERLAP`, re-provisionar el certificado del `workers.dev`.
