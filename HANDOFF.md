# YEA Skills Pathways — site handoff (2-theme build)

**Empora Ventures × YEA · pitch site + working prototype · for Emma Godwin**
_Last updated: 8 June 2026_

---

## TL;DR

The YEA pitch site + youth-tool prototype, now shipped in **two complete themes** from one
repo, deployed to Vercel, and committed to git under Logan's identity.

- **Live:** https://yea-skills-pathways.vercel.app (chooser → both versions)
- **Repo:** `~/yea-skills-pathways` → GitHub `loganhempel/yea-skills-pathways`
- **Source of truth for content:** still `~/YEA Skills Pathways Pilot/05 HTML site` (the original dark build). This repo's `dark/` is a copy of it.

### The links to send / show

| | Pitch site | Prototype |
| --- | --- | --- |
| **Light — "Fresh Field"** (recommended) | `/light/` | `/light/demo/` |
| **Dark — "Premium Dark"** (original) | `/dark/` | `/dark/demo/` |
| **Chooser** | `/` | — |

---

## What changed this session

1. **New light theme — "Fresh Field"**: warm cream `#FAF8F2` · deep ink `#19231B` ·
   fresh growth-green `#15803D` · honey-amber `#E08A2B` secondary. Chosen over the
   lime/black because it's on-theme for Food & Fibre and reads better for a
   youth/government audience.
2. **Kept the original dark** untouched as `dark/`.
3. **Root chooser page** so one deploy exposes both.
4. **Deployed to Vercel** (production) + **git history** (3 commits, authored as Logan).

### Decision locked: HTML, not React Native
Keep it HTML. The deliverable is a screen-shared / publicly-linkable pitch + web prototype.
React Native ships native apps to the App/Play stores — no public web URL without React
Native Web, and ~5× the complexity for zero pitch benefit. If YEA greenlights the build,
the *production* tool can be a responsive web app / PWA. The pitch stays static HTML.

---

## How the theming works (reusable pattern)

Both `index.html` files are **fully tokenised** with CSS custom properties in `:root`, plus
a handful of hardcoded colours. The light theme is **NOT a rewrite** — it's a single
**cascade-override block appended at the end of `<style>`** that:

1. **Redefines every `:root` token** to the light palette (later declaration wins).
2. **Overrides the ~25 selectors that hardcoded a dark-only value** — light-grey body text
   → dark; white-alpha overlays (`rgba(255,255,255,…)` blueprint grid, ghost numbers) →
   dark-alpha; the dark glass nav; the blue timeline segment → green.
3. **Recolours the inline-attributed SVG schematic strokes** with CSS
   (`.schematic .draw{stroke:var(--lime)}` beats the `stroke="#DAFF02"` presentation
   attribute — CSS always wins over SVG attributes).
4. **Swaps the lime PNG wordmark** (which clashes with green) for an inline ink+green CSS
   wordmark (`.wordmark{…}` / `EMPORA <b>VENTURES</b>`).

> The terminal card was **deliberately left dark** — a dark code block on cream is an
> intentional contrast, not a bug.

**To re-theme again** (e.g. a third palette): copy `light/`, replace just the override
block's `:root` values + the handful of selector overrides. ~40 lines of CSS, structure
untouched. This is the cheapest way to ship N looks of one site.

---

## Verifying renders with headless Chrome (gotchas learned)

`"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --screenshot=…`

- **Scroll-reveal hides content.** Sections use `opacity:0` until an IntersectionObserver
  fires on scroll — they screenshot blank. Force them visible first: inject a `<style>`
  with `.rv,.schematic,.statement,.builder-grid{opacity:1!important;transform:none!important}`.
- **`min-height:100vh` explodes with a tall window.** The hero stretches to fill an
  8000px-tall capture window and centres its content far down the page. Neutralise it
  (`.hero{min-height:auto!important}`) before a full-page tall capture.
- **Headless ignores `#anchor` scrolling** — don't rely on it to frame a section.
- **Working approach:** inject the two overrides above into a `/tmp/verify.html` copy via
  `sed`, capture one tall window (`--window-size=1440,10000`), then slice into bands with
  PIL and read each. `node --check` won't validate `.html`; isolate the `<script>` if you
  need a JS syntax check.

---

## Deploying to Vercel safely (important gotcha)

- **DO NOT** use the no-arg `deploy_to_vercel` MCP tool from this machine — it publishes the
  **whole working directory** (Logan's home folder = client files, invoices, everything).
- **DO** use the Vercel CLI scoped to the project folder. The CLI is globally authed as
  `loganhempel` (global token, not in `~/.vercel`):
  ```
  cd ~/yea-skills-pathways && npx -y vercel@latest --prod --yes
  ```
  This uploads only the folder. It auto-created the project `yea-skills-pathways` and the
  alias `yea-skills-pathways.vercel.app`. Re-run the same command to redeploy.

---

## Relevant skills (for next time)
`premium-website-design`, `frontend-design`, `design-mirror` (style-matching), `ui-ux-pro-max`
(palettes), `git-pushing`. The original build also used `schematic-animation` for the SVG
diagrams.

## Open items
1. **Branding decision still open** (from the original handoff): the PDF + Canva deck are
   still Emporom/Empora-Group cobalt-orange. The site is Empora Ventures. Decide whether to
   unify before sending Emma.
2. Pick which theme to lead with for Emma (recommendation: **light/Fresh Field**).
3. Optional: point a nicer domain/subdomain at the Vercel project.
