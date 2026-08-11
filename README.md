# Mountain Speaker

Christian confessions/declarations app. "We speak TO the mountain, not ABOUT the mountain" (Mark 11:23).

## Current state

This is a **static, installable PWA** — no build step, no framework, no backend. It works as-is when served over HTTP(S):

- `index.html` — the entire app (markup, styles, and JS in one file)
- `manifest.json` — PWA manifest (installable, standalone display, theme colors)
- `sw.js` — service worker, cache-first app shell for offline use
- `icons/` — app icons (192, 512, apple-touch-icon, favicon) — placeholder mountain mark on navy/blue, replace with real branding art before shipping

Content model: 18 "mountains" (circumstances) — poverty, offense, pride, doubt, loss, healing, patience, fear, anger, stress, love, peace, marriage, family, complacency, regret/the past, debt, identity — each with 5 confessions and 5 declarations. Every entry cites a real KJV reference and shows the actual KJV verse text (public domain, safe to reproduce verbatim). **Only KJV is used** — every other common translation (NKJV, NLT, TLB, AMP, AMPC, MEV, ERV, ESV) is copyrighted by its publisher and was deliberately left out; don't add translation-switching back in without a license or publisher API (ESV has a free non-commercial API; others require licensing).

Favorites persist via `localStorage` (wrapped in try/catch since some embedded/sandboxed webviews block storage access).

## Why it's not installable yet, as-is

PWAs require **HTTPS + being served from a real origin** — `file://` won't register a service worker or show an install prompt. To test installability locally:

```bash
npx serve .
# or
python3 -m http.server 8080
```
Then open the served URL (not the raw file) in Chrome/Edge/Safari.

## Suggested next steps for Claude Code

1. **Verify PWA install** — serve locally, confirm manifest + service worker register cleanly (check DevTools → Application tab), confirm "Add to Home Screen" prompts on Android/Chrome and works via Safari's Share sheet on iOS.
2. **Real icon art** — swap `/icons/*.png` placeholders for real branding (the current ones are programmatically generated, not designed).
3. **Decide on a host** — Vercel/Netlify/Cloudflare Pages all work for a static PWA with zero config.
4. **Optional: move off inline everything** — currently HTML/CSS/JS are one file for portability. Fine for a PWA this size, but if the content list keeps growing, consider splitting `content.js` out as a separate data file and/or moving to a lightweight bundler (Vite) — ask if you want that refactor.
5. **Push notifications for daily confessions** (if wanted later) require a backend (e.g. web push via a server, or a service like OneSignal) — not included yet, since it needs infrastructure decisions.
6. **Bible translations**: if licensing is sorted later, ESV's API is the easiest legal path to add a second translation without violating copyright.
