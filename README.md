# prezzers.dev

My personal site and blog, built with [Astro](https://astro.build) and self-hosted on my [homelab](https://github.com/prezzers/homelab).

## Writing a post

Add a Markdown file to `src/content/blog/`:

```md
---
title: 'Post title'
description: 'One-line summary for previews and search.'
pubDate: '2026-11-20'
---

Write here.
```

Push to `main` and Coolify rebuilds and deploys the site.

## Running locally

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # production build into dist/
```

## How it's deployed

- A multi-stage `Dockerfile` builds the static site with Node, then serves `dist/` with nginx on port 80.
- Coolify (on my homelab) builds it from this repo on every push to `main`.
- Cloudflare Tunnel serves it at https://prezzers.dev with no open ports on my router.
