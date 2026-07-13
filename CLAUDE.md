# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single static HTML page for **Link** (business entity: LinkCalendar, LLC) — an iOS app that lets you share your whereabouts with chosen friends, see who's nearby, and make plans in a few taps. The site's current purpose is narrow: it exists to give **Apple Developer Program enrollment** a legitimate, live web presence. It is intentionally minimal — not a marketing/conversion page — so keep additions concise unless asked to expand scope.

Live at **https://mylinkcal.com** (and `https://www.mylinkcal.com`, which redirects to the apex), hosted on GitHub Pages from [Treejaber/Link-Website](https://github.com/Treejaber/Link-Website).

## Current status / handoff

Apple Developer Program support rejected the enrollment (case #102939453646, enrollment ID MF33HFH6SU) because the site "contains minimal content." In response, across the last several sessions the page went from plain text-only to text + a real product screenshot:

- Replaced the old marketing headline/pitch/status-line copy with one short, plain paragraph stating what's being built (no more `<h1>`, no more "Currently in development" status line).
- Added a cropped screenshot of the app prototype (`assets/screenshot-whos-around.png`, 905×2033px, transparent background, rounded corners baked into the image) — see **Architecture** below for how it's laid out and where it came from.
- Iterated on copy weight/size and screenshot size per direct feedback: paragraph is now bold black (`font-weight:700`, `color:var(--ink)`) and sized up (`clamp(1.3rem,2.2vw,1.6rem)`); screenshot display width bumped to `clamp(15rem,38vw,24rem)`.
- Fixed a layout bug where vertically-centering `.hero` in `main` left a large empty gap under the header on wide/short viewports — `main` now top-aligns (`justify-content:flex-start`) instead.

All of the above is committed and pushed to `main` / live on mylinkcal.com. **Not yet done / unconfirmed:** whether the user has resubmitted the enrollment with Apple, or whether it's been accepted — that's outside this repo and hasn't been reported back in a session yet.

Loose end: `link-screen-1-today.png` sits untracked in the repo root — it's the original, low-resolution raw screenshot paste from early in this work (superseded by the high-res crop in `assets/`), not referenced anywhere in `index.html`. It's harmless but undecided whether to delete it or leave it; ask the user if it comes up.

## Commands

There is no build, package manager, lint, or test setup — `index.html` is the entire site: a single file with inline `<style>` and no JS.

Local dev server:
```
python3 -m http.server 8743 --directory .
```
Then open `http://localhost:8743`. This is also wired up as a Claude Code preview config at `.claude/launch.json` (gitignored — local tooling only, not part of the deployed site).

## Architecture

- **`index.html`** — the whole site. Structure: `<header>` (wordmark + contact email) → `<main>` (`.hero` section) → `<footer>` (copyright + contact email). No JS, no external assets besides two Google Fonts (`Instrument Serif` for the wordmark/serif accents, `Instrument Sans` weights 400/500/700 for body text) and one local image.
- **Hero layout**: `.hero .wrap` is a flex column (image below text) below the `56rem` breakpoint, and a flex row (`.hero-text` on the left, `.hero-shot` on the right) above it. `.hero-text` is a single bold paragraph (no heading element) — `.hero-shot` holds the app screenshot (`assets/screenshot-whos-around.png`), sized via `clamp(15rem,38vw,24rem)` with no border/frame wrapper, since the PNG already has transparent, rounded corners baked in.
- **Screenshot provenance**: the current image was captured directly from a live Claude design prototype (a `claude.ai/design` canvas) using the Claude-in-Chrome browser tool, exported at native 905×2033px resolution with a transparent background — not a manual screenshot crop. If it needs to be replaced/updated later, that prototype is the source of truth for what the app UI looks like; get a fresh native-resolution export rather than screenshotting-and-cropping a lower-res paste (that produced a visibly blurry result the first time around).
- **Page-level layout**: `body` is a `min-height:100dvh` flex column with `header`/`footer` fixed-size (`flex-shrink:0`) and `main` as the flexible middle (`flex:1 0 auto`, top-aligned via `justify-content:flex-start`) — a sticky-footer pattern that pins the footer to the viewport bottom without vertically centering the hero (centering previously caused an oversized gap under the header on wide/short viewports). Fluid type via `clamp()` throughout; single `.wrap` container class (`max-width:68rem`) used for header, hero, and footer alignment alike.
- **Typographic hierarchy**: the `.wordmark` ("Link" in the header) is sized `clamp(2.9rem,8vw,5.25rem)` and is deliberately the single largest text element on the page — bigger than the hero paragraph's `clamp(1.3rem,2.2vw,1.6rem)` at every viewport width. Keep it that way if either size changes.
- **Theme**: strict black & white (`--ink:#000`, `--paper:#fff`, `--quiet:#6B6B6B` for secondary/quiet text like the footer and header email link). All colors are CSS custom properties in `:root` — reuse them rather than hardcoding hex values.
- Branding used throughout: **LinkCalendar, LLC**, contact `contact@mylinkcal.com`.

## Conventions

- This page has been deliberately trimmed down over several iterations (an illustrated "connected dots" map, a calendar graphic, a prose pull-quote, a "How it works" section, and a dark CTA band were all added and later removed) to stay minimal for its current sole purpose. The one addition that *did* stick is the app screenshot, added specifically to satisfy Apple's "minimal content" rejection — don't re-add other sections/illustrations without being asked, default to concision.
- No survey/waitlist links belong on this page — survey distribution is handled separately for internal research purposes, by explicit product-owner decision.
- No privacy policy page yet — deferred until actual App Store submission (distinct from developer program enrollment, which doesn't require one).
- The repo is under git (`main` branch, pushed to `origin` = `Treejaber/Link-Website` on GitHub). Use normal git history (`git log`, `git show <hash>:index.html`) to recover earlier versions rather than keeping hand-maintained backup copies in this file.

## Deployment

GitHub Pages, deployed from the `main` branch root (repo Settings → Pages → "Deploy from a branch"). Custom domain wiring:
- `CNAME` file at repo root contains `mylinkcal.com` — required by GitHub Pages to bind the custom domain, and must stay in sync with whatever domain is configured in Settings → Pages.
- DNS is managed in **Porkbun** (Cloudflare-backed DNS panel), not GitHub: apex `mylinkcal.com` has four `A` records pointing at GitHub Pages' IPs (`185.199.108.153`/`.109`/`.110`/`.111.153`); `www` is a `CNAME` to `treejaber.github.io`. HTTPS is enforced.
- The same Porkbun DNS zone also carries `MX`/`TXT` (DKIM/SPF/site-verification) records for Google Workspace email on `contact@mylinkcal.com` — do not touch those when editing site-related DNS.

## Open items for a future session

1. **Confirm Apple resubmission outcome** — check with the user whether they've resubmitted enrollment (case #102939453646) with the new screenshot/copy, and whether it was accepted.
2. **Privacy policy page** — Apple requires a privacy policy URL for actual App Store *submission* (distinct from developer program enrollment, which is already satisfied by the live site). Not needed yet, but will be before shipping. No page exists for this currently.
3. **Business address** — `index.html` has a commented-out placeholder in the footer (`<!-- Optionally add a business address line here for verification purposes -->`). Apple's enrollment/verification process may want a physical address on the site — add if requested.
4. **App Store link** — once the app is live, add a real App Store badge/link to `index.html`.
5. **Untracked `link-screen-1-today.png`** — decide whether to delete this stale, superseded source image from the repo root (see Current status above).
