# Paradigm Internal Guides

Living documentation for Paradigm staff, published at **https://guides.pgc.ca**.

---

## ⚠ Content rules (read first)

This repo is **public on GitHub** (we're on the free org plan, which only supports
GitHub Pages from public repos). That means:

- **Source HTML is visible to anyone** who finds the repo at github.com
- **Published guides at guides.pgc.ca are anti-indexed** (won't show on Google) but
  reachable by anyone who has the URL
- **Treat every file here as if a stranger could read it** — because they can

**Never commit to this repo:**
- Passwords, API keys, tokens, client secrets, certificates
- Internal IP addresses, server hostnames, machine network details
- Employee personal info, contact details beyond public role context
- Pricing, contract terms, financial info
- Detailed infrastructure / operational runbooks (those belong in the
  private `Paradigm-Master` notes)

**Safe to commit:**
- Generic "how to use X" guides for staff
- Onboarding info that any new employee would learn anyway
- Public product/vendor documentation links
- Brand-level descriptions of how Paradigm uses tools

If you're unsure whether something is safe → don't commit it. Put it in
`Paradigm-Master/` instead (private), and link out from the guide to a
SharePoint location that requires M365 sign-in.

---

## What's in here

| File | URL once live | Audience |
|---|---|---|
| `index.html` | https://guides.pgc.ca/ | Landing page with search + filters |
| `using-claude.html` | https://guides.pgc.ca/using-claude | All staff — Claude day-to-day usage |
| `claude-m365-setup.html` | https://guides.pgc.ca/claude-m365-setup | IT / admins — Claude ↔ M365 integration |

Supporting files:

| File | What it does |
|---|---|
| `CNAME` | Tells GitHub Pages to serve at `guides.pgc.ca` |
| `.nojekyll` | Disables Jekyll processing — files served exactly as-written |
| `robots.txt` | Tells search engines and AI crawlers to skip this site |
| `.gitignore` | Files git should never track (.DS_Store, drafts, etc.) |

---

## How to update an existing guide

1. Open the relevant `.html` in your editor on your Mac
2. Make your changes, save
3. From this folder in Terminal:
   ```bash
   git add .
   git commit -m "Brief description of what changed"
   git push
   ```
4. GitHub Pages rebuilds automatically. Live in ~1-2 minutes at guides.pgc.ca.

## How to add a new guide

1. **Pick a URL-friendly filename** — all lowercase, hyphens not spaces, no special chars:
   - `bluebeam-onboarding.html` ✓
   - `Bluebeam Onboarding v3.html` ✗ (becomes ugly URL)
2. **Copy an existing guide as a starting point** to inherit the `<head>` block
   with the `noindex` and `referrer` meta tags:
   ```bash
   cp using-claude.html bluebeam-onboarding.html
   ```
3. Edit the new file — change the `<title>`, content, etc.
4. **Add a card for it in `index.html`** — copy one of the existing `<a class="guide">`
   blocks, change the `href`, title, description, and `data-tags`.
5. **Update `data-tags`** with relevant tags. Current tag vocabulary:
   - Audience: `all-staff`, `it`
   - Topic: `ai`, `microsoft-365`, `setup`
   - (Add new tags as needed — also add a matching filter chip in `index.html`'s
     `.filters` section)
6. Commit, push.

---

## Search + filters on the landing page

`index.html` has client-side search and filter chips:

- **Search box** — instant filter as you type; matches title, description, or tag
- **Filter chips** — click to toggle; multiple can be active (OR logic — card matches if ANY selected filter matches)
- **Keyboard:** press `/` anywhere to focus the search box, `Esc` to blur
- **Reset filters** button appears when any search or filter is active

The search/filter logic is pure JavaScript in the `<script>` block at the
bottom of `index.html`. No build step, no dependencies.

---

## Anti-indexing setup

Two layers ensure these guides don't show on Google / Bing / AI search:

1. **`robots.txt`** at the site root — tells well-behaved crawlers (including
   GPTBot, ClaudeBot, CCBot, Google-Extended, PerplexityBot) to skip everything
2. **`<meta name="robots">`** in each HTML `<head>` — direct page-level signal

> Note: GitHub Pages does not support setting custom HTTP headers (no
> `X-Robots-Tag`). The two layers above are sufficient for major search engines
> and AI crawlers, but the HTTP-header signal would be slightly stronger. If we
> ever need belt-and-suspenders, we'd front this site with Cloudflare proxy
> (orange cloud) and add the header via a Transform Rule.

After deploying, verify with:

```bash
curl -s https://guides.pgc.ca/using-claude | grep -i robots
```

Should output the `<meta name="robots" ...>` line confirming the signal is in place.

You can also submit to Google Search Console and watch the Coverage report
flag pages as "Excluded by 'noindex' tag" — that's the confirmation Google saw
and obeyed.

---

## Internal Paradigm file-naming convention (for reference)

For files stored in **SharePoint / OneDrive** (not this repo), Paradigm uses:

```
YYMMDD_ENTITY_TYPE_DEPT_TOPIC_VERSION.ext
```

Example: `260522_PGC_BRIEF_IT_ClaudeM365Connect_v01.html`

For **this repo** we deliberately use URL-friendly slugs instead, because the
filename becomes the public URL path. Git history (`git log --follow <file>`)
preserves the version information for us.

---

## Hosting setup

- **Domain:** `guides.pgc.ca` (Cloudflare DNS, in the `pgc.ca` zone, gray-cloud
  proxy OFF so GitHub Pages can manage the SSL cert)
- **Hosting:** GitHub Pages, served from `main` branch root
- **Source:** this repo (`Paradigm-Building-Solutions-Ltd/internal-guides`)
- **Deploy:** auto on push to `main` (1-2 min)
- **SSL:** Let's Encrypt cert auto-managed by GitHub Pages

## Maintainer

Deeparsh Singh Dang — `deeparsh.dang@pgc.ca`
