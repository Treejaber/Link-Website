# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single static HTML page for **Link** (business entity: LinkCalendar, LLC) — an iOS app that lets you share your whereabouts with chosen friends, see who's nearby, and make plans in a few taps. The site's current purpose is narrow: it exists to give **Apple Developer Program enrollment** a legitimate, live web presence. It is intentionally minimal — not a marketing/conversion page — so keep additions concise unless asked to expand scope.

Live at **https://mylinkcal.com** (and `https://www.mylinkcal.com`, which redirects to the apex), hosted on GitHub Pages from [Treejaber/Link-Website](https://github.com/Treejaber/Link-Website).

## Commands

There is no build, package manager, lint, or test setup — `index.html` is the entire site: a single file with inline `<style>` and no JS.

Local dev server:
```
python3 -m http.server 8743 --directory .
```
Then open `http://localhost:8743`. This is also wired up as a Claude Code preview config at `.claude/launch.json` (gitignored — local tooling only, not part of the deployed site).

## Architecture

- **`index.html`** — the whole site. Structure: `<header>` (wordmark + contact email) → `<main>` (vertically-centered `.hero` section: headline, one-paragraph pitch, dev-status line) → `<footer>` (copyright + contact email). No JS, no external assets besides two Google Fonts (`Instrument Serif` for headings/serif accents, `Instrument Sans` for body text).
- **Layout**: `body` is a `min-height:100dvh` flex column with `header`/`footer` fixed-size (`flex-shrink:0`) and `main` as the flexible middle (`flex:1 0 auto`) that vertically centers `.hero`. This is a deliberate sticky-footer pattern — it keeps the footer pinned to the viewport bottom instead of leaving a large blank gap on tall screens, since the page has very little content. Mobile-considerate but currently reviewed primarily in desktop viewport. Fluid type via `clamp()`; single `.wrap` container class (`max-width:68rem`) used for both header and content alignment.
- **Typographic hierarchy**: the `.wordmark` ("Link" in the header) is deliberately sized to be the single largest text element on the page at every viewport width — `clamp(2.9rem,8vw,5.25rem)`, strictly exceeding the hero `h1`'s `clamp(2.6rem,7vw,4.6rem)` on all three clamp parameters (min/vw-factor/max). If either clamp is changed, keep this ordering intact rather than letting the headline become dominant again.
- **Theme**: strict black & white (`--ink:#000`, `--paper:#fff`, `--quiet:#6B6B6B` for secondary text). All colors are CSS custom properties in `:root` — reuse them rather than hardcoding hex values.
- Branding used throughout: **LinkCalendar, LLC**, contact `contact@mylinkcal.com`.

## Conventions

- This page has been deliberately trimmed down over several iterations (an illustrated "connected dots" map, then a calendar graphic, then a prose pull-quote, then a "How it works" section and a dark CTA band were all added and later removed) to stay minimal for its current sole purpose. Don't re-add sections/illustrations without being asked — default to concision.
- No survey/waitlist links belong on this page — survey distribution is handled separately for internal research purposes, by explicit product-owner decision.
- No privacy policy page yet — deferred until actual App Store submission (distinct from developer program enrollment, which doesn't require one).
- The repo is under git (`main` branch, pushed to `origin` = `Treejaber/Link-Website` on GitHub). Use normal git history (`git log`, `git show <hash>:index.html`) to recover earlier versions rather than keeping hand-maintained backup copies in this file.

## Deployment

GitHub Pages, deployed from the `main` branch root (repo Settings → Pages → "Deploy from a branch"). Custom domain wiring:
- `CNAME` file at repo root contains `mylinkcal.com` — required by GitHub Pages to bind the custom domain, and must stay in sync with whatever domain is configured in Settings → Pages.
- DNS is managed in **Porkbun** (Cloudflare-backed DNS panel), not GitHub: apex `mylinkcal.com` has four `A` records pointing at GitHub Pages' IPs (`185.199.108.153`/`.109`/`.110`/`.111.153`); `www` is a `CNAME` to `treejaber.github.io`. HTTPS is enforced.
- The same Porkbun DNS zone also carries `MX`/`TXT` (DKIM/SPF/site-verification) records for Google Workspace email on `contact@mylinkcal.com` — do not touch those when editing site-related DNS.

## Open items for a future session

1. **Privacy policy page** — Apple requires a privacy policy URL for actual App Store *submission* (distinct from developer program enrollment, which is already satisfied by the live site). Not needed yet, but will be before shipping. No page exists for this currently.
2. **Business address** — `index.html` has a commented-out placeholder in the footer (`<!-- Optionally add a business address line here for verification purposes -->`). Apple's enrollment/verification process may want a physical address on the site — add if requested.
3. **App Store link** — once the app is live, add a real App Store badge/link to `index.html`.
