# Paradigm Internal Guides

Living documentation for Paradigm staff, hosted at **https://guides.pgc.ca**.

> **Internal use only.** This repo is private. The published site is unlisted (no
> auth gate at the moment, but blocked from search engines via `_headers`,
> `robots.txt`, and per-page `<meta name="robots">`). Treat URLs as
> internal — don't link to them from any public-facing page.

---

## What's in here

| File | URL once live | Audience |
|---|---|---|
| `index.html` | https://guides.pgc.ca/ | Landing page — lists every guide |
| `using-claude.html` | https://guides.pgc.ca/using-claude | All staff — how to use Claude day-to-day |
| `claude-m365-setup.html` | https://guides.pgc.ca/claude-m365-setup | IT / admins — Claude ↔ M365 integration briefing |

---

## How to update a guide

1. Open the relevant `.html` in your editor on your Mac
2. Make changes, save
3. From this folder in Terminal:
   ```bash
   git add .
   git commit -m "Brief description of the change"
   git push
   ```
4. Cloudflare Pages picks up the push automatically. Live in ~30-60 seconds.

## How to add a new guide

1. Name the file using a clean, URL-friendly slug (lowercase, hyphens, no spaces):
   - `bluebeam-onboarding.html` (good)
   - `Bluebeam Onboarding v3.html` (bad — ugly URL)
2. Copy the `<head>` block from an existing guide so the `noindex` and `referrer`
   meta tags carry over.
3. Add a card for it in `index.html` (copy one of the existing `<a class="guide">`
   blocks and edit).
4. Commit, push, done.

---

## Internal Paradigm file-naming convention (for reference)

For files stored in SharePoint / OneDrive (NOT this repo), Paradigm uses:

```
YYMMDD_ENTITY_TYPE_DEPT_TOPIC_VERSION.ext
```

Example: `260522_PGC_BRIEF_IT_ClaudeM365Connect_v01.html`

For this repo we deliberately use URL-friendly names instead, because the
filename becomes the public URL slug. Git history preserves the version
information for us.

---

## Anti-indexing setup (why and how)

We don't want these pages showing up on Google. Three layers ensure this:

1. **`robots.txt`** — tells well-behaved crawlers to skip the entire site
2. **`_headers`** — Cloudflare Pages applies HTTP headers (`X-Robots-Tag`) to
   every response, which is the strongest signal even for non-HTML files
3. **`<meta name="robots">`** in each HTML file's `<head>` — catches direct hits

After deploying, verify by running from your laptop:

```bash
curl -I https://guides.pgc.ca/using-claude
```

You should see `x-robots-tag: noindex, nofollow, noarchive, ...` in the response.

---

## Maintainer

Deeparsh Singh Dang — `deeparsh.dang@pgc.ca`

## Hosting

- **Domain:** `guides.pgc.ca` (Cloudflare DNS, in the `pgc.ca` zone)
- **Hosting:** Cloudflare Pages (free tier, unlimited bandwidth)
- **Source:** this repo (`Paradigm-Building-Solutions-Ltd/internal-guides`)
- **Deploy:** auto on push to `main`
