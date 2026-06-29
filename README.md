# RumboLabs

The umbrella site for [rumbolabs.net](https://rumbolabs.net) — an independent studio
building small, sharp software products: **Draftmark**, **PixelVault**,
**ContentVitals**, and **Joblane** (coming soon).

Single-page, fully static site served via **Cloudflare Workers Static Assets**.

## Structure

```
public/index.html   # the entire site (HTML + inline CSS/JS, no dependencies)
wrangler.jsonc      # Workers Static Assets config
package.json        # dev / deploy scripts
```

## Develop locally

```bash
npm install
npm run dev          # serves at http://localhost:8787
```

## Deploy

```bash
npm run deploy       # wrangler deploy
```

First-time setup needs a Cloudflare account: `npx wrangler login`.
After deploying, point the `rumbolabs.net` custom domain at the Worker in the
Cloudflare dashboard (Workers & Pages → rumbolabs → Settings → Domains & Routes).

## Editing content

All copy lives in `public/index.html`. The product descriptions are working
drafts inferred from the product names — swap in the real one-liners in the
`#products` section. Per-product colors are CSS variables at the top of the file
(`--draftmark`, `--pixelvault`, `--contentvitals`, `--joblane`).
