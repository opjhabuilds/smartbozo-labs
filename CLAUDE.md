# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The live marketing site for **smartbozo.com**, deployed on Vercel via GitHub (push to `main` auto-deploys — no build step, no CI config in this repo).

## Repository structure

This is a single static HTML file with no build tooling, no package manager, no dependencies:

- `index.html` — the entire site: all CSS lives in one `<style>` block in `<head>`, all JS lives in one `<script>` block before `</body>`. There is no separate CSS/JS file, no bundler, no framework.
- `README.md` — one-line project name only.

There is nothing to install, build, lint, or test. To preview changes, open `index.html` directly in a browser or serve the directory with any static file server (e.g. `python3 -m http.server`).

## Architecture of index.html

The page is one long scroll of `<section>`s, each with an `id` matched by an anchor in the top `<nav>`, in order:

1. `.ticker` — scrolling marquee of status lines (not nav-linked)
2. `#top` `.hero` — wordmark/intro
3. `#philosophy` — three numbered "pillars" (operating principles)
4. `#index` `.lab-index` — the product grid (`.specimen` cards), the main content block to edit when products change
5. `#notes` — external writing links (Substack/Medium/LinkedIn)
6. `#builder` — founder bio/stats
7. `#contact` — contact methods + footer colophon

All content (product descriptions, links, status stamps) is hardcoded directly in the HTML as static markup — there is no CMS, data file, or templating layer. Adding/editing a product means duplicating a `.specimen` `<article>` block in the `#index` section and updating `.index-count strong` (the product count shown top-right of that section).

Each specimen has a `.stamp` with a status modifier class that drives its color: `live`, `building`, `planned`, `concept`, `exploring` (see CSS rules `.stamp.*`). Use the matching class when changing a product's status.

## Styling conventions

- Color palette is defined once as CSS custom properties on `:root` (`--paper`, `--ink`, `--vermillion`, `--ochre`, `--moss`, `--indigo`, etc.) — reuse these variables rather than hardcoding new colors.
- Three font families loaded from Google Fonts and used consistently by role: `Fraunces` (serif, headings/display), `Newsreader` (serif, body text), `JetBrains Mono` (monospace, labels/meta/UI chrome like the ticker, nav, chips, stamps).
- Responsive breakpoints are `@media (max-width: 900px)` and `@media (max-width: 500px)` at the bottom of the `<style>` block — add overrides there rather than inline.

## JS behavior

The inline `<script>` only does two things: smooth-scroll for in-page nav links, and an `IntersectionObserver` scroll-reveal applied to `.specimen`, `.pillar`, and `.note-card` elements. Any new repeating card type that should animate in on scroll needs to be added to that `querySelectorAll` selector list.

## Git / deploy

- Remote: `origin` → `github.com/opjhabuilds/smartbozo-labs.git`
- Pushing to `main` deploys straight to production on Vercel (static site, zero build config) — treat commits to `main` as live-site changes.

## Permanent rules

- Header, footer, Google Tag, and analytics setup must stay consistent across all pages if/when this site grows beyond one page.
- Canonical contact details: WhatsApp `+91 79770 47646`, email `hello@smartbozo.com`. Never change these values unless explicitly asked to.
- Never use em dashes ( — ) anywhere in site content. Use a period, comma, or parentheses instead.
