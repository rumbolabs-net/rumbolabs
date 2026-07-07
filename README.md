# RumboLabs

[![Deploy](https://github.com/facundofarias/rumbolabs/actions/workflows/deploy.yml/badge.svg)](https://github.com/facundofarias/rumbolabs/actions/workflows/deploy.yml)

**Live:** https://rumbolabs.net · Cloudflare Worker `rumbolabs` · deploy with `npm run deploy` ([details](#deploy))

The umbrella site for **[rumbolabs.net](https://rumbolabs.net)** — an independent,
bootstrapped studio in Barcelona building small, sharp, AI-native and privacy-first
software products:

| Product | Site | What it is |
|---|---|---|
| Draftmark | [draftmark.app](https://draftmark.app) | Share your thinking in markdown — write, publish, and gather feedback, with an API for people and AI agents |
| PixelVault | [pixelvault.dev](https://pixelvault.dev) | Agent-first image hosting — upload via API, get instant CDN URLs |
| ContentVitals | [contentvitals.ai](https://contentvitals.ai) | AI agents that monitor your content's "vital signs" and tell you how to fix SEO issues |
| Joblane | [joblane.ai](https://joblane.ai) | AI job-application copilot — Apply/Stretch/Skip verdicts plus tailored CV and cover letter |

Single-page, fully static site served via **Cloudflare Workers Static Assets**.
No build step, no dependencies — just one self-contained HTML file.

## Structure

```
public/index.html   # the entire site (HTML + inline CSS/JS, no dependencies)
wrangler.jsonc      # Workers Static Assets config (serves ./public)
package.json        # dev / deploy scripts
CLAUDE.md           # guidance for AI assistants working in this repo
```

## Develop locally

```bash
npm install
npm run dev          # serves at http://localhost:8787 (hot reload)
```

## Deploy

The site is deployed as a Cloudflare Worker named `rumbolabs`.

```bash
npm run deploy       # = wrangler deploy
```

**First-time setup** needs a Cloudflare account and CLI auth:

```bash
npx wrangler login   # opens a browser to authorize
npx wrangler whoami  # verify you're authenticated
```

After deploy, Wrangler prints the live URLs:

- Workers default: `https://rumbolabs.<account-subdomain>.workers.dev`
- Custom domain: **https://rumbolabs.net** (already attached)

### Custom domain

`rumbolabs.net` is attached to the Worker via **Cloudflare dashboard →
Workers & Pages → `rumbolabs` → Settings → Domains & Routes → Custom Domain**.
This is a one-time setup; subsequent `npm run deploy` runs keep it working with no
extra steps.

### Verify a deploy

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://rumbolabs.net/   # expect 200
```

## Editing content

All copy and styling live in **`public/index.html`** (single file).

- **Product cards** are in the `#products` section.
- **Per-product colors** are CSS variables near the top of the file:
  `--draftmark`, `--pixelvault`, `--contentvitals`, `--joblane`.
- **Outbound product links carry UTMs** for referral attribution:
  `utm_source=rumbolabs.net` · `utm_medium=referral` · `utm_campaign=homepage` ·
  `utm_content=hero-card|footer`. Keep this scheme consistent when adding links.

After editing, run `npm run deploy` to publish.

## License

The **source code** is released under the [MIT License](LICENSE) — feel free to learn
from it, fork it, and reuse the code.

The **RumboLabs brand is not** covered by that license. The name "RumboLabs" and its
product names (Draftmark, PixelVault, ContentVitals, Joblane), logos, `og.png`, and
marketing copy remain © RumboLabs. Please don't reuse them in a way that implies
affiliation or passes your project off as ours.
