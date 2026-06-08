# YEA Skills Pathways — site + demo handoff

**Empora Ventures × YEA · pitch site + working prototype · for Emma Godwin**
_Last updated: 9 June 2026_

---

## TL;DR

A pitch site + interactive youth-tool prototype, shipped in **two themes** from one repo,
live on Vercel, committed under Logan's identity, and pushed to GitHub.

- **Live:** https://yea-skills-pathways.vercel.app  (root = chooser → both themes)
- **Repo:** `~/yea-skills-pathways`  →  GitHub **`loganhempel/skills-pathways`** (note: local dir name and repo name differ)
- **Lead with:** the **light "Fresh Field"** version. It's the flagship.

### Links to send / show

| | Pitch site | Prototype (the demo) |
| --- | --- | --- |
| **Light — "Fresh Field"** ⭐ | `/light/` | `/light/demo/` |
| **Dark — "Premium Dark"** (original) | `/dark/` | `/dark/demo/` |
| Chooser | `/` | — |

### How to show it (decided)
Don't screen-share a click-through of the demo — a laggy cursor tour kills the polish.
**Hand them the link and have them open `/light/demo/` on their own phones.** That's where
the wow is. Be present on the call to field questions (conflict-of-interest, privacy, data);
presence + self-exploration beats either alone. See "Delivery play" below.

---

## What this is

- **`<theme>/index.html`** — the pitch site: hero → opportunity → product → live prototype →
  build/architecture (animated SVG schematics) → "Built by youth" → who's building it →
  youth engagement → phases → investment → why us → contact.
- **`<theme>/demo/index.html`** — the working youth tool: home → interests → (strengths) →
  "matching you" micro-moment → ranked industries → role ladder → role detail → "your pathway".
- Plain HTML/CSS/JS. No build step, no deps. Deploy the folder as-is.

> Demo roles/skills/pay are **representative food & fibre data** for the pitch. The live tool
> would draw from the verified Muka Tangata dataset.

---

## Design system

- **Fresh Field (light):** cream `#FAF8F2` · ink `#19231B` · fresh green `#15803D`
  (`--lime` token) · honey amber `#E08A2B` · hairlines `rgba(25,35,27,.12)`.
- **Premium Dark (original):** near-black `#15140F` · off-white `#F1EDE3` · electric lime `#DAFF02`.
- Fonts: **Archivo** (display) · **Hanken Grotesk** (body) · **JetBrains Mono** (labels).
- **Brand mark:** a small "ascending pathway" glyph (nodes + a rising line, destination node
  filled) used in the wordmark + favicon. The demo home motif and SVG schematics echo it.

### How the theming works (reusable)
Both `index.html` files are fully tokenised (`:root` custom props). The light theme is **not a
rewrite** — it's a **cascade-override block appended at the end of `<style>`** that (1) redefines
every `:root` token, (2) overrides the ~25 selectors with hardcoded dark-only values, (3) recolours
inline SVG strokes via CSS (`.schematic .draw{stroke:var(--lime)}` beats `stroke="#DAFF02"` — CSS
wins over SVG presentation attributes), (4) swaps the lime PNG wordmark for an inline CSS wordmark.
To ship a third look: copy `light/`, change ~40 lines of `:root` + overrides. The terminal/code
window is **deliberately dark in both themes** (intentional contrast, not a bug).

---

## Highlights worth not breaking

- **Pitch — builder section:** the bio is deliberately humble ("being 18 genuinely helps… he's
  building for people his own age"), and the right-hand dark window is a **clear plain-language
  spec** ("what the pilot needs → what we already do"), not a cryptic fake-terminal — so a
  non-technical reader (Emma) can read it.
- **Pitch — statement:** "Built by youth. / For youth." uses `.sline{overflow:hidden}` as a
  reveal mask; it needs `line-height:1.06` + `padding-bottom:.16em` or the `y` descenders clip.
- **Demo — home screen (light, flagship):** soft drifting aurora, staggered page-load reveal,
  and a bespoke **"pathway you climb"** motif (You → Industries → *Your pathway*, glowing
  destination node) replacing the old boxy step list, plus one gradient CTA with a single load
  sheen. Built for the on-your-phone first impression. _(Light only; dark demo home is the
  earlier design.)_
- **Demo — role detail:** each section opens with a hairline divider + accent dot so the dense
  screen reads as chapters.

---

## Verifying renders with headless Chrome (gotchas)

`"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --screenshot=…`

- **Scroll-reveal hides content** (`.rv/.schematic/.statement/.builder-grid` are `opacity:0`
  until IntersectionObserver fires) → inject `<style>…{opacity:1!important;transform:none!important}`
  into a `/tmp/verify.html` copy first.
- **`min-height:100vh` heroes balloon** in a tall window → neutralise to `min-height:auto`.
- **Headless ignores `#anchor` scroll.** Capture one tall window (`--window-size=1440,10000`),
  slice into bands with PIL, read each.
- **To preview a JS-gated deep screen** (e.g. the demo role-detail `s4`): inject a `<script>`
  after the demo script that drives the real flow on load
  (`picks.add(...); rank(); openIndustry('dairy'); openRole(1);`) — the top-level `let`/`function`
  globals are reachable across script tags. Then dump computed styles into `document.title` and
  read it with `--dump-dom` to confirm CSS applied without needing to see the pixels.
- **Image-read budget is finite per session** — after many screenshot reads, the viewer hard-caps
  and rejects further images regardless of size. When that happens, fall back to computed-style /
  DOM checks and have Logan eyeball the live URL. _(The demo home redesign + role-detail dividers
  were verified functionally this way, not visually — worth a human eyeball.)_

---

## Deploying to Vercel safely (important)

- **DO NOT** use the no-arg `deploy_to_vercel` MCP tool here — it publishes the **whole working
  dir** (Logan's home folder: client files, invoices).
- **DO** use the CLI scoped to the folder (globally authed as `loganhempel`, token not in `~/.vercel`):
  ```
  cd ~/yea-skills-pathways && npx -y vercel@latest --prod --yes
  ```
  Uploads only the folder; redeploys to the same `yea-skills-pathways.vercel.app` alias.
- **Git push:** HTTPS via osxkeychain (`git push origin main`). Commits authored as
  `Logan Hempel <emporom.shop@gmail.com>` (matches his other repos → counts on his profile).

---

## PDF + Canva deck: RETIRED — do not send

The proposal PDF (`~/YEA Skills Pathways Pilot/02 Pitch (proposal PDF)/`) and Canva deck
`DAHLgmjDaBY` are **a version behind** — they still pitch the old **paid-intern model
(~$8,000 youth / ~$27,500 total)**, which contradicts the live site's lean model
(~$1,500 youth / ~$21,000 total). **Lead with the live link instead.** If a leave-behind is
ever needed, sync them to the lean model first (or regenerate from the site).

---

## Pitch audit — fixed vs still open

**Fixed & live (both themes):** pilot = 6 months (was mislabelled 8) · VUW hackathon framed as
planned/confirm-before-Phase-1 · NEETs framed as "built to reach" not guaranteed · youth costs
now sum (~$1,200 + $0–300 = ~$1,500) · ownership clarified (YEA owns tool/code/CMS, Muka Tangata
data stays theirs) · governance one-liner surfaced early · light em-dash pass.

**Still open (judgment calls — Logan to direct):**
1. Conflict-of-interest framing + a written probity plan (declared interest, recusal in writing,
   independent decision-maker). The #1 thing a governance reviewer scrutinises.
2. A privacy / data-sovereignty line (minors' data, Privacy Act 2020, Māori Data Sovereignty,
   offshore Vercel hosting).
3. Securing Muka Tangata **data-use rights** (the whole product depends on their dataset).
4. Turn the 4-vs-6 industries point into a proof point ("their Excel breaks at a 5th industry;
   our build already runs six in the prototype").

---

## Delivery play (for the meeting)

Be present, don't screen-share, make them open the link on their own phones. Presenting the
pitch isn't what Logan recuses from — the recusal is the *vendor decision*. If he genuinely
can't attend (family commitment), tell the truth + send the link + a 2-min Loom + lock a
follow-up — don't manufacture a strategic no-show in a competitive bid.

---

## Skills used (save for next time)
- **`premium-website-design`** — hero/first-impression, cognitive-load, brand-system, peak-end micro-interactions (the audit + polish lens).
- **`frontend-design`** — distinctive, non-generic craft for the demo home redesign (aurora, motif, orchestrated load).
- **`ui-ux-pro-max`** — mobile UI patterns/palettes (relevant for further demo polish).
- **`stop-slop` / `humanizer`** — copy pass (banned words, em-dash density, humble tone).
- **`marketing-psychology`** — the deal-risk audit framing.
- **`schematic-animation`** — the original pitch's animated SVG architecture diagrams.
- **`git-pushing`** — repo + push under Logan's identity.

## Open items
1. Eyeball the demo **home screen** + **role-detail dividers** on the live light URL (verified
   functionally, not visually).
2. The four open audit items above.
3. Decide whether to ever revive a synced leave-behind (PDF/deck), or stay link-only.
4. Optional: a nicer domain/subdomain on the Vercel project.
