# Paradigm Internal Guides

Living documentation for Paradigm staff, published at **https://guides.pgc.ca**.

This `README.md` is **the canonical operating manual** for this repository. It is
written for two audiences in equal measure:

1. **Humans** who maintain this repo (currently Deeparsh)
2. **AI agents** (Claude Opus 4.7 and successors) invoked inside this folder to
   add, edit, or delete guides on the maintainer's behalf

The conventions below are not suggestions — they are the contract. If you are an
agent reading this, follow every rule. If you are a human and want to change a
rule, edit this README first, then propagate the change through every existing
file before committing.

---

## ⚠ Content rules (read first, every time)

This repo is **public on GitHub** (free org plan only supports Pages from public
repos). The published site at `guides.pgc.ca` is also publicly accessible but
**anti-indexed** so it won't appear on Google or in AI search results.

**Implications:**

- Source HTML is visible to anyone who finds the repo at github.com
- Published guides are reachable by anyone with the URL
- **Treat every file in this repo as if a stranger could read it — because they can**

### Never commit to this repo

- Passwords, API keys, tokens, client secrets, certificates
- Internal IP addresses, server hostnames, machine network details
- Employee personal info or contact details beyond public role context
- Pricing, contract terms, financial data
- Detailed infrastructure / operational runbooks
- Any leaked-credential reference numbers, internal ticket IDs, or vendor account numbers

### Safe to commit

- Generic "how to use X" guides for staff
- Onboarding info that any new employee would learn anyway
- Public product/vendor documentation summaries
- Brand-level descriptions of how Paradigm uses tools
- Step-by-step procedures that don't reveal infrastructure topology

### When in doubt → do not commit

Put the content in `Paradigm-Master/` (private) instead, and link out from the
guide to a SharePoint location that requires M365 sign-in.

---

## ✍ Writing style (mandatory for every guide)

These rules are not optional. They apply to all visible text in every guide.

- **Never use em dashes (`—`) or en dashes (`–`) anywhere in guide text.** This
  includes body copy, headings, list items, captions, and callouts. Use a
  period, comma, colon, or parentheses instead. The title-tag suffix uses a
  colon: `<title>Title: Paradigm Internal Guide</title>` (not an em dash).
- **Write simply. No fluff.** Cut filler and self-congratulatory phrasing.
  Banned words/phrases: "no-jargon", "seamless(ly)", "simply", "just",
  "effortless", "two-minute walkthrough", "in no time", "don't worry". State
  what to do, plainly.
- **Short sentences.** One instruction per sentence where possible. Prefer
  active voice ("Click Add" not "the Add button should be clicked").
- **Assume zero technical knowledge** for end-user (`all-staff`) guides: name
  exactly what to click, where it is on screen, and what will happen next.
- **Name things exactly.** Use the real network name, button label, or menu
  item (e.g. "Paradigm Corporate Wi-Fi", `Printers & scanners`), not vague
  references like "the office network" or "the settings".

---

## Architecture (so you know what you're touching)

```
Local Mac folder
   /Users/deeparsh/Documents/Deeparsh/Paradigm Projects/InternalGuides/
       │
       │  git push
       ▼
GitHub repo (public)
   Paradigm-Building-Solutions-Ltd/internal-guides
       │
       │  GitHub Pages auto-build on push to main
       ▼
GitHub Pages serving from main branch / root
   paradigm-building-solutions-ltd.github.io/internal-guides
       │
       │  CNAME file in repo → guides.pgc.ca
       │  Cloudflare DNS: guides CNAME paradigm-building-solutions-ltd.github.io (gray cloud)
       ▼
Public URL
   https://guides.pgc.ca/
   https://guides.pgc.ca/<slug>     ← one URL per guide
```

**Deploy time:** 30 sec – 2 min after every `git push` to `main`.
**No build step.** Raw HTML/CSS/JS served exactly as committed.
**SSL:** auto-managed by GitHub Pages via Let's Encrypt.

---

## Repo file inventory

| File | Purpose | Editable? |
|---|---|---|
| `index.html` | Landing page with search + filters + guide cards | YES — when adding/removing/renaming guides |
| `using-claude.html` | Guide: How to Use Claude at PGC (all staff) | YES |
| `claude-m365-setup.html` | Guide: Connecting Claude to Microsoft 365 (IT/admins) | YES |
| `_headers` | (deleted — not used; GH Pages ignores) | N/A |
| `CNAME` | Tells GitHub Pages the custom domain is `guides.pgc.ca` | NO — never modify |
| `.nojekyll` | Empty file telling GH Pages to skip Jekyll processing | NO — never modify |
| `robots.txt` | Tells search engines and AI crawlers to skip the entire site | Add new AI crawler User-agents as you become aware of them |
| `.gitignore` | Standard junk patterns (.DS_Store, drafts, editor files) | Rarely |
| `README.md` | This file | When conventions change |

All HTML guide files at the repo root become URLs at `guides.pgc.ca/<filename-without-.html>`.

---

## File-naming convention (strict)

Every published guide is a single `.html` file at the repo root.

### Filename rules

| Rule | Why |
|---|---|
| All lowercase | URL case-sensitivity ambiguity is removed |
| Hyphens, never spaces or underscores | Google explicitly recommends hyphens; spaces become ugly `%20` in URLs |
| No special characters (`&`, `?`, `é`, etc.) | URL-encoding makes them illegible |
| Short but descriptive — 3 to 5 words max | Memorable URL; people will type these by hand |
| Always end with `.html` extension | GitHub Pages auto-strips it from the URL anyway |
| Avoid dates or version numbers in the filename | Git history preserves versioning; URLs should be stable |

### Examples

| ✓ Good | ✗ Bad |
|---|---|
| `bluebeam-onboarding.html` | `Bluebeam Onboarding v3.html` |
| `using-claude.html` | `claude_guide_v3.html` |
| `slack-best-practices.html` | `260615_PGC_GUIDE_IT_SlackBP_v01.html` |
| `password-reset.html` | `password-reset-procedure-final.html` |

> **Note on the Paradigm internal naming convention:** for files in
> SharePoint/OneDrive, Paradigm uses `YYMMDD_ENTITY_TYPE_DEPT_TOPIC_VERSION.ext`.
> That convention does NOT apply here because the filename becomes the public URL
> slug. Use URL-friendly slugs instead. Git history (`git log --follow <file>`)
> preserves all version information.

---

## Required HTML structure for every new guide

Every guide MUST have these elements in the `<head>`, in this order:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="robots" content="noindex, nofollow, noarchive, nosnippet, noimageindex">
<meta name="referrer" content="no-referrer">
<title>[Descriptive title]: Paradigm Internal Guide</title>
```

The two `<meta>` tags after viewport are non-negotiable anti-indexing controls.
Forgetting them is a hard error — search engines could pick up the guide.

### Body styling baseline

All guides use the **Catamaran** Google Font. Always preload it:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Catamaran:wght@200;300;400;500;600;700;800;900&display=swap" rel="stylesheet">
```

### Brand color palette (use CSS variables for consistency)

```css
:root {
  --paper: #F2EEE5;       /* main background */
  --paper-deep: #EAE4D6;  /* secondary background */
  --paper-card: #FBF8F1;  /* card background */
  --ink: #161513;         /* main text */
  --ink-soft: #4A453D;    /* secondary text */
  --ink-faint: #8A8378;   /* metadata text */
  --red: #C8202C;         /* primary accent — Paradigm red */
  --red-deep: #962027;    /* darker red */
  --red-tint: rgba(200, 32, 44, 0.08);  /* red tint background */
  --gold: #B68A35;
  --green: #2F4F36;
}
```

Existing guides (`using-claude.html` and `claude-m365-setup.html`) are good
references for typography, spacing, paper grain texture, and architectural grid
backgrounds. Match their visual sophistication.

### Recommended structure for a new guide

```
1. <head>  with all required meta tags
2. Top bar with brand mark + page title (fixed position)
3. Hero section: eyebrow + title + lede + meta blocks
4. TL;DR block (dark ink background, red accent) — optional but encouraged
5. Numbered chapters / sections with chapter headers
6. Body content with body-grid layout (margin + main + margin)
7. Pull quotes, define blocks, method cards as needed
8. Footer with brand mark + tagline + maintainer info
9. <script> for scroll animations / progress bar
```

---

## Tag taxonomy (canonical list — update this section when adding tags)

Tags drive the filter chips on `index.html`. They are written **lowercase, hyphenated, single-word-ish**, separated by spaces in the `data-tags` HTML attribute.

### Current tag vocabulary

| Tag | Category | When to use |
|---|---|---|
| `all-staff` | Audience | Guide is for any employee at any of the PGC companies |
| `it` | Audience | Guide is for IT staff, admins, or technically-inclined readers |
| `management` | Audience | Guide is for executives, plant managers, supervisors |
| `field` | Audience | Guide is for site supervisors, foremen, field workers |
| `ai` | Topic | Anything about Claude, ChatGPT, AI usage, prompts |
| `microsoft-365` | Topic | Outlook, Teams, SharePoint, OneDrive, Office apps |
| `setup` | Topic | Initial install or configuration of any tool |
| `policy` | Topic | Company policies, conduct, expectations |
| `security` | Topic | Passwords, MFA, phishing, secure handling of data |
| `onboarding` | Topic | New-hire material |
| `troubleshooting` | Topic | "When X breaks, do Y" runbooks |
| `tools` | Topic | Software tooling guides |

### Rules

- Each guide must have **at least one audience tag** (`all-staff`, `it`, `management`, or `field`)
- Each guide should have **at least one topic tag** but can have multiple
- If you need a new tag, add it both to this table AND to the filter chips in `index.html` (see "Adding a filter chip" below)
- Use the most-specific existing tag before inventing a new one

---

## How to add a new guide (step-by-step procedure)

This is the complete procedure for an AI agent or human to follow when adding a
new guide. Every step is mandatory unless explicitly marked optional.

### Step 1 — Decide the slug and tags

- Slug = filename without `.html`, all lowercase, hyphenated
- Tags = pick from the canonical list above (at least 1 audience + 1 topic)

### Step 2 — Create the HTML file

```bash
cd "/Users/deeparsh/Documents/Deeparsh/Paradigm Projects/InternalGuides"
cp using-claude.html <new-slug>.html
```

(Starting from an existing guide preserves the `<head>` block with all required
anti-indexing tags and the brand fonts/colors. Do not write a new guide from
scratch — always copy.)

### Step 3 — Edit the new file

In the new `.html`:
- Update `<title>` to a descriptive name ending with `: Paradigm Internal Guide`
- Update the eyebrow / hero / lede to match the new topic
- Replace all body content
- Update any meta blocks (audience, last-updated, etc.)
- Update the footer maintainer info if needed
- **Never remove or modify the `<meta name="robots">` and `<meta name="referrer">` tags**

### Step 4 — Add a card to `index.html`

Open `index.html`. Find the `<div class="guides" id="guides">` section.
Inside that div, copy an existing `<a class="guide">` block. The structure:

```html
<a href="/<new-slug>"
   class="guide"
   data-title="<exact same title as in the .html file>"
   data-desc="<one-line description — appears below the title>"
   data-tags="<space-separated tags from the canonical list>">
  <div class="guide-num">0<span class="slash">/</span><NEW-NUMBER></div>
  <div class="guide-body">
    <span class="guide-eyebrow">For <audience description></span>
    <h2 class="guide-title"><same title></h2>
    <p class="guide-desc">
      <description — can be longer here than data-desc; this is what users see>
    </p>
    <div class="guide-tags">
      <span class="guide-tag audience">For <audience></span>
      <span class="guide-tag topic"><topic 1></span>
      <span class="guide-tag topic"><topic 2></span>
    </div>
  </div>
  <div class="guide-side">
    <div class="guide-meta">Updated <span class="red"><Month Year></span></div>
    <div class="guide-meta">~ <N> min read</div>
    <div class="guide-arrow"><span>Read guide</span><span class="arrow">→</span></div>
  </div>
</a>
```

**Key requirements:**
- `href` = `/` + slug (no `.html` extension — Pages auto-resolves)
- `data-title` / `data-desc` / `data-tags` drive search and filtering — they MUST be present
- Guide number = next sequential (existing are `01`, `02` → new is `03`, etc.)
- `data-tags` and visible `<span class="guide-tag">` should match each other
- Audience tag uses the visible label "For all staff" or "For IT / admins" but the `data-tags` attribute uses the slug form (`all-staff`, `it`)

### Step 5 — Add a filter chip (only if introducing a NEW tag)

If your new guide uses a tag not yet in the filter chips, add a chip.

In `index.html`, find the `<div class="filters-wrap" id="filters">` section.
After the existing chips, add:

```html
<button class="chip" data-filter="<new-tag-slug>"><Display Label></button>
```

The `data-filter` value MUST match the lowercase-hyphenated tag exactly as
written in `data-tags` on the guide cards. The display label can be any
human-readable text (e.g., `For management`, `Security`).

**Also update the tag taxonomy table in this README.**

### Step 6 — Validate before commit

Run through this checklist mentally:

- [ ] New `.html` file has the four required `<meta>` tags in `<head>`
- [ ] Filename is lowercase, hyphenated, ends in `.html`
- [ ] `<title>` ends with `: Paradigm Internal Guide`
- [ ] No em dashes (`—`) or en dashes (`–`) anywhere in the guide text (see Writing style)
- [ ] Content contains no secrets, no internal IPs, no leaked credentials
- [ ] A matching `<a class="guide">` card exists in `index.html`
- [ ] `data-tags` on the card lists every applicable canonical tag
- [ ] Visible `<span class="guide-tag">` items on the card match `data-tags`
- [ ] Guide number on the card is sequential
- [ ] If a new tag was introduced, the filter chip was added AND the tag was added to the README taxonomy

### Step 7 — Commit and push

```bash
git add .
git status   # confirm only intended files are staged
git commit -m "Add <guide title> guide"
git push
```

Cloudflare DNS already points `guides.pgc.ca` at GitHub Pages. Within 30-90
seconds of the push, the new guide is live.

### Step 8 — Verify deployment

```bash
# Wait 60-90 seconds after push, then:
curl -I https://guides.pgc.ca/<new-slug>
# Expected: HTTP/2 200

# Verify anti-indexing tag landed:
curl -s https://guides.pgc.ca/<new-slug> | grep -i robots
# Expected: <meta name="robots" content="noindex, nofollow, noarchive, ...">

# Open the landing page in browser, confirm:
# - New card appears
# - Search for words in the title — guide shows up
# - Click each filter chip your guide's tags belong to — guide shows up
```

---

## How to update an existing guide

1. Open the `.html` in editor
2. Edit content
3. If the change is substantive, update the `data-desc` and possibly `data-tags`
   on the matching card in `index.html` so search keeps working
4. If the title changes meaningfully, also update `data-title` AND the visible
   `<h2 class="guide-title">` on the card — they must match
5. Commit + push with a descriptive message

```bash
git add .
git commit -m "Update <guide name>: <what changed>"
git push
```

---

## How to delete a guide

1. Delete the `.html` file:
   ```bash
   git rm <slug>.html
   ```
2. Remove the matching `<a class="guide">` card from `index.html`
3. **Re-number the remaining cards** — guides 01, 02, 03 should stay sequential
   with no gaps (the number is cosmetic but the convention is "no gaps")
4. If the deleted guide was the only one using a particular tag, also:
   - Remove the now-unused filter chip from `index.html`
   - Remove the tag from this README's taxonomy table
5. Commit + push:
   ```bash
   git add .
   git commit -m "Remove <guide name> guide"
   git push
   ```

---

## Commit message convention

| Action | Format | Example |
|---|---|---|
| Add new guide | `Add <title> guide` | `Add Bluebeam onboarding guide` |
| Update content | `Update <title>: <what changed>` | `Update Claude guide: added Excel section` |
| Fix typo / formatting | `Fix typo in <title>` | `Fix typo in M365 setup guide` |
| Delete guide | `Remove <title> guide` | `Remove deprecated Slack guide` |
| Site-level change | `<verb> <area>: <detail>` | `Update README: new tag taxonomy` |
| Multi-change | Use multi-line body | (see below) |

Multi-line commit body (when changes span multiple guides):

```
Brief one-line summary

- Bullet describing change 1
- Bullet describing change 2
- Bullet describing change 3
```

Never use these in commit messages: `Co-Authored-By:`, AI attribution lines,
emoji, or "🤖 Generated with..." footers.

---

## Anti-indexing setup (do not remove)

Two layers ensure guides don't show on Google / Bing / AI search:

1. **`robots.txt`** at the site root — tells well-behaved crawlers (Google,
   Bing, DuckDuckGo, GPTBot, ClaudeBot, CCBot, Google-Extended, PerplexityBot)
   to skip everything
2. **`<meta name="robots">`** in each HTML `<head>` — direct page-level signal

> GitHub Pages does not support setting custom HTTP headers (no `X-Robots-Tag`).
> The two layers above are sufficient for major search engines and AI crawlers.

### Verifying anti-indexing after a deploy

```bash
curl -s https://guides.pgc.ca/<any-slug> | grep -i robots
```

Should output:
```html
<meta name="robots" content="noindex, nofollow, noarchive, nosnippet, noimageindex">
```

If you ever see search-engine traffic appearing in any analytics, the most
likely cause is a missing or modified `<meta name="robots">` tag in one of the
guide files. Re-add it.

### Adding a new AI crawler to robots.txt

If a new AI training crawler becomes known (e.g., new model from new vendor),
add an explicit block in `robots.txt`:

```
User-agent: NewCrawlerBot
Disallow: /
```

The wildcard `User-agent: *` rule covers them already, but explicit named
blocks are stronger signals.

---

## Hosting setup (canonical record)

| Item | Value |
|---|---|
| **Domain** | `guides.pgc.ca` |
| **DNS record** | CNAME → `paradigm-building-solutions-ltd.github.io` |
| **DNS proxy** | Cloudflare DNS only (gray cloud) — orange cloud would break GitHub's SSL |
| **DNS zone** | `pgc.ca` (managed in Cloudflare under `it@pgc.ca`) |
| **Hosting** | GitHub Pages |
| **Source branch** | `main` |
| **Source folder** | `/` (root) |
| **SSL** | Let's Encrypt cert auto-managed by GitHub Pages |
| **Enforce HTTPS** | Enabled |
| **Repo** | `Paradigm-Building-Solutions-Ltd/internal-guides` |
| **Repo visibility** | Public (free plan requirement for GitHub Pages) |
| **Deploy trigger** | Push to `main` |
| **Deploy time** | 30 sec – 2 min after push |
| **Local clone** | `/Users/deeparsh/Documents/Deeparsh/Paradigm Projects/InternalGuides/` |
| **Git auth** | `gh` CLI via macOS Keychain (set up via `gh auth login`) |

### Files that should NEVER be modified

These exist for hosting plumbing and changing them will break the site:

- `CNAME` — contains exactly `guides.pgc.ca\n`. Changing it breaks custom domain.
- `.nojekyll` — empty file. Removing it makes GH Pages try Jekyll on the site.

### Files that may rarely need modifying

- `robots.txt` — only to add new AI crawler User-agents
- `.gitignore` — only if new draft-file conventions emerge

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Push rejected with auth error | `gh` CLI session expired | `gh auth login` again |
| Site shows 404 after push | Deploy still building OR slug typo | Wait 2 min; if persistent, check Actions tab on GitHub for build errors |
| Site shows old content | Browser cache | Hard refresh: `Cmd+Shift+R` (Mac) or `Ctrl+Shift+R` (Windows) |
| Custom domain check failing in GitHub Settings → Pages | Cloudflare cloud is orange | Switch to gray (DNS only) in Cloudflare DNS settings |
| SSL warning in browser | Cert hasn't issued yet | Wait 5-30 min after DNS check passed; `openssl s_client -servername guides.pgc.ca -connect guides.pgc.ca:443` to check current cert subject |
| Search/filter not working on landing page | JS error or malformed card attributes | Open browser console; check `data-tags` attribute syntax (lowercase, hyphenated, space-separated, no quotes inside) |
| Card visible but search doesn't find guide | `data-title` or `data-desc` missing on card | Re-check card in `index.html` matches the schema in "Adding a card" above |

---

## Maintainer

**Deeparsh Singh Dang**
IT &amp; AI Coordinator, Paradigm Group of Companies
deeparsh.dang@pgc.ca

## Related repositories (for context, not modification from this folder)

- `Paradigm-Master/` (private, local-only) — the comprehensive internal
  knowledge base. Sensitive infrastructure / operational content lives there,
  NOT here.
- Future agents working in this folder: do NOT cross-reference paths inside
  `Paradigm-Master/` in any guide. That folder is private; URLs and paths from
  it must never appear in public guides.
