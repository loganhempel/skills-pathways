# YEA — Youth Skills Pathways

Pitch site + working prototype for **Empora Ventures'** proposal to design and build
YEA's (Youth Employment Aotearoa) Food & Fibre Skills Pathways digital tool.

Two complete, self-contained static builds of the same proposal:

| Version | Theme | Path | Live |
| --- | --- | --- | --- |
| **Dark** | "Premium dark" — near-black + electric lime (original Empora Ventures look) | [`/dark`](./dark) | _see Vercel_ |
| **Light** | "Fresh Field" — warm cream + deep ink + fresh growth-green (on-theme for Food & Fibre) | [`/light`](./light) | _see Vercel_ |

Each version contains:

- `index.html` — the pitch site (hero → opportunity → product → live prototype → build/architecture → youth engagement → phases → investment → why us → contact)
- `demo/index.html` — the working youth tool prototype (interests → match micro-moment → ranked industries → role ladder → "your pathway" summary)

## Stack

Plain HTML/CSS/JS — no build step, no dependencies. Fonts via Google Fonts.
Fully tokenised CSS variables; the light theme is a cascade override layered on the
original, so both versions stay in sync structurally.

> Demo data (roles, skills, pay) is **representative food & fibre data** for the pitch.
> The live tool would draw from the verified Muka Tangata dataset.

## Run locally

Open `dark/index.html` or `light/index.html` in a browser. The prototype is linked
from each pitch site and also embedded as a live phone frame.

---

Empora Ventures · Logan Hempel · Wellington, Aotearoa
