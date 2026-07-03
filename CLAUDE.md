# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single static HTML page for **Link** (business entity: LinkCalendar, LLC) — an iOS app that lets you share your whereabouts with chosen friends, see who's nearby, and make plans in a few taps. The site's current purpose is narrow: it exists to give **Apple Developer Program enrollment** a legitimate, live web presence. It is intentionally minimal — not a marketing/conversion page — so keep additions concise unless asked to expand scope.

## Commands

There is no build, package manager, lint, or test setup — `index.html` is the entire site: a single file with inline `<style>` and no JS. Preview it by opening `index.html` directly in a browser, or serve it with any static file server, e.g.:

```
python3 -m http.server 8743 --directory .
```

## Architecture

- **`index.html`** — the whole site. Structure: `<header>` (wordmark + contact email) → `<main class="hero">` (headline, one-paragraph pitch, dev-status line) → `<footer>` (copyright + contact email). No JS, no external assets besides two Google Fonts (`Instrument Serif` for headings/serif accents, `Instrument Sans` for body text).
- **Theme**: strict black & white (`--ink:#000`, `--paper:#fff`, `--quiet:#6B6B6B` for secondary text). All colors are CSS custom properties in `:root` — reuse them rather than hardcoding hex values.
- **Layout**: mobile-considerate but currently reviewed primarily in desktop viewport. Fluid type via `clamp()`; single `.wrap` container class (`max-width:68rem`) used for both header and content alignment.
- Branding used throughout: **LinkCalendar, LLC**, contact `contact@mylinkcal.com`.

## Conventions

- This page has been deliberately trimmed down over several iterations (an illustrated "connected dots" map, then a calendar graphic, then a prose pull-quote, then a "How it works" section and a dark CTA band were all added and later removed) to stay minimal for its current sole purpose. Don't re-add sections/illustrations without being asked — default to concision.
- No survey/waitlist links belong on this page — survey distribution is handled separately for internal research purposes, by explicit product-owner decision.
- No privacy policy page yet — deferred until actual App Store submission (distinct from developer program enrollment, which doesn't require one).

## Open items for a future session

1. **Privacy policy page** — Apple requires a privacy policy URL for actual App Store *submission* (distinct from developer program enrollment). Not needed yet, but will be before shipping. No page exists for this currently.
2. ~~`hello@linkcalendar.com`~~ — resolved: site now uses the real inbox `contact@mylinkcal.com`.
3. **Business address** — `index.html` has a commented-out placeholder in the footer (`<!-- Optionally add a business address line here for verification purposes -->`). Apple's enrollment/verification process may want a physical address on the site — add if requested.
4. **App Store link** — once the app is live, add a real App Store badge/link to `index.html`.

## Deploying

This is a single dependency-free HTML file — any static host works with zero config:
- GitHub Pages: push this folder to a repo, enable Pages on `main`/`docs`.
- Netlify/Vercel: drag-and-drop the folder or connect the repo.
- S3 + CloudFront, or any plain web host: upload `index.html` as-is.

No environment variables, no build command, no server required.

## Archived snapshot

A backup copy of the current, minimal `index.html` — kept so this exact version isn't lost if the site is later expanded into a fuller marketing page and needs to be reverted.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Link — See which friends are nearby</title>
<meta name="description" content="Link lets you share your whereabouts with the people you choose, see which friends are nearby, and make plans in a few taps. Built by LinkCalendar, LLC.">
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Ccircle cx='7' cy='16' r='4' fill='black'/%3E%3Ccircle cx='25' cy='16' r='4' fill='black'/%3E%3Cline x1='7' y1='16' x2='25' y2='16' stroke='black' stroke-width='2'/%3E%3C/svg%3E">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Instrument+Sans:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#000000;
    --paper:#FFFFFF;
    --quiet:#6B6B6B;
    --measure:34rem;
  }
  *{margin:0;padding:0;box-sizing:border-box}
  html{scroll-behavior:smooth}
  body{
    background:var(--paper);
    color:var(--ink);
    font-family:"Instrument Sans",-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
    font-size:1.0625rem;
    line-height:1.6;
    -webkit-font-smoothing:antialiased;
  }
  a{color:inherit;text-decoration-thickness:1px;text-underline-offset:3px}
  a:focus-visible{outline:2px solid var(--ink);outline-offset:3px}
  .wrap{max-width:68rem;margin:0 auto;padding:0 1.5rem}

  /* ---------- header ---------- */
  header{padding:1.75rem 0}
  header .wrap{display:flex;justify-content:space-between;align-items:baseline;gap:1rem}
  .wordmark{
    font-family:"Instrument Serif",Georgia,serif;
    font-size:1.6rem;
    letter-spacing:-.01em;
    text-decoration:none;
  }
  .header-mail{font-size:.9rem;color:var(--quiet);text-decoration:none}
  .header-mail:hover{color:var(--ink);text-decoration:underline}

  /* ---------- hero ---------- */
  .hero{padding:5.5rem 0 3.5rem}
  h1{
    font-family:"Instrument Serif",Georgia,serif;
    font-weight:400;
    font-size:clamp(2.6rem,7vw,4.6rem);
    line-height:1.05;
    letter-spacing:-.015em;
    max-width:16ch;
  }
  h1 em{font-style:italic}
  .hero p{
    margin-top:1.75rem;
    max-width:var(--measure);
    color:var(--quiet);
    font-size:clamp(1.05rem,1.6vw,1.2rem);
  }
  .hero p strong{color:var(--ink);font-weight:500}
  .hero .status{
    margin-top:1.75rem;
    font-size:.85rem;
    color:var(--quiet);
  }

  /* ---------- footer ---------- */
  footer{padding:2.25rem 0 3rem}
  footer .wrap{
    display:flex;flex-wrap:wrap;gap:.4rem 1.75rem;
    justify-content:space-between;
    font-size:.85rem;color:var(--quiet);
  }
  footer a{color:var(--quiet)}
  footer a:hover{color:var(--ink)}
</style>
</head>
<body>

<header>
  <div class="wrap">
    <a class="wordmark" href="/">Link</a>
    <a class="header-mail" href="mailto:contact@mylinkcal.com">contact@mylinkcal.com</a>
  </div>
</header>

<main>
  <section class="hero">
    <div class="wrap">
      <h1>Your friends are closer than <em>you think.</em></h1>
      <p>You and a friend are in the same city — and <strong>neither of you knows it</strong>. Link lets you share your whereabouts with the people you choose, see who&rsquo;s nearby, and make plans in a few taps.</p>
      <p class="status">Currently in development.</p>
    </div>
  </section>
</main>

<footer>
  <div class="wrap">
    <span>&copy; 2026 LinkCalendar, LLC. All rights reserved.</span>
    <!-- Optionally add a business address line here for verification purposes -->
    <a href="mailto:contact@mylinkcal.com">contact@mylinkcal.com</a>
  </div>
</footer>

</body>
</html>
```
