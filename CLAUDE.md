# CLAUDE.md

Guidance for AI assistants working in this repository.

## What this is

The **RumboLabs umbrella site** — a single-page, static marketing site for
[rumbolabs.net](https://rumbolabs.net) that introduces the studio and links to its
five products. Live in production.

- **Studio identity:** independent, **bootstrapped (no VC)**, based in **Barcelona,
  Spain**, **AI-native but privacy-first** ("your data is yours, never the product"),
  grows organically.
- **Products:** Draftmark (draftmark.app), PixelVault (pixelvault.dev),
  ContentVitals (contentvitals.ai), Joblane (joblane.ai), Pecia (pecia.app).

## Architecture (read before editing)

- The **entire site is one file: `public/index.html`** — HTML + inline `<style>` +
  inline `<script>`. There is **no build step, no framework, no dependencies** in the
  page itself. Do not introduce a bundler, CSS framework, or JS library unless
  explicitly asked.
- Served via **Cloudflare Workers Static Assets** (`wrangler.jsonc` points `assets`
  at `./public`). There is **no Worker script** — it's pure static asset serving.
- `package.json` only carries `wrangler` as a devDependency and the `dev`/`deploy`
  scripts.

## Conventions

- **Per-product color** is a CSS variable (`--draftmark`, `--pixelvault`,
  `--contentvitals`, `--joblane`, `--pecia`) and each card sets `style="--c: var(--<product>)"`.
  Reuse this pattern; don't hardcode hex values in markup.
- **Outbound product links must carry UTMs** for referral attribution:
  `?utm_source=rumbolabs.net&utm_medium=referral&utm_campaign=homepage&utm_content=hero-card`
  (use `utm_content=footer` for footer links). Keep this consistent for any new link.
- **Scroll-reveal is progressive enhancement:** `.reveal` elements are only hidden
  (`opacity:0`) under the `.js` class, which an inline `<head>` script adds. Content
  must remain visible if JS is disabled — preserve this when touching reveal logic.
- **Motion respects `prefers-reduced-motion`** — keep that media query intact when
  adding animations.
- Product copy reflects each product's **actual** positioning (verified from the live
  sites). If asked to change copy, verify against the real site rather than inventing.

## Develop & deploy

```bash
npm install
npm run dev      # local preview at http://localhost:8787 (hot reload)
npm run deploy   # publish to Cloudflare Workers (worker name: rumbolabs)
```

- First-time auth: `npx wrangler login`; verify with `npx wrangler whoami`.
- `rumbolabs.net` is already attached as a custom domain in the Cloudflare dashboard;
  redeploys keep it working with no extra steps.
- **Verify after deploy:** `curl -s -o /dev/null -w "%{http_code}\n" https://rumbolabs.net/`
  should return `200`.

## Don'ts

- Don't add a build step, framework, or runtime dependencies to the page.
- Don't commit `node_modules/`, `.wrangler/`, or local screenshot output (`.preview/`)
  — all gitignored.
- Don't deploy or commit unless asked.
