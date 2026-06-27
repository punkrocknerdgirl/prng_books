# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This repo is a single-page static marketing website for PRNG Bookkeeping Services (prngbooks.com). The entire site is one file: `index.html`. There is no build step, package manager, framework, or test suite — just HTML with an embedded `<style>` block.

## Working with `index.html`

- The file is large (~687KB, 642 lines) because it contains **two inline base64-encoded images** (the hero photo and the about-section photo), each occupying a single very long line (around line 468 and line 550).
- **Do not `Read` the whole file** — it exceeds the tool's token limit due to the base64 lines. Instead:
  - Use `grep -n` to find sections/styles by name or comment marker.
  - Use `sed -n 'START,ENDp' index.html | grep -v base64` to view a range of lines while excluding the image data.
  - Use `Read` with `offset`/`limit` for small ranges that don't include lines ~468 or ~550.
- To preview the site locally, just open `index.html` in a browser, or run a simple static server (e.g. `python3 -m http.server`) from this directory.

## Structure of `index.html`

The `<style>` block (head) is organized into clearly marked sections via comments (`/* ── NAME ── */`):
- NAV, HERO, SECTION BASE, WHAT I DO, ABOUT, WORK WITH ME, CONTACT, FOOTER, RESPONSIVE (single `@media (max-width: 768px)` breakpoint at the end)

Color palette and fonts are defined as CSS custom properties in `:root` (e.g. `--pink`, `--plum`, `--light`, `--mid`, `--dark`, `--text`, `--mono`, `--sans`). Fonts (Roboto, Roboto Mono) are loaded from Google Fonts.

The `<body>` mirrors the CSS sections, each marked with HTML comments and an `id` used for nav anchor links:
- `<nav>` — fixed top nav with logo and links to each section, plus a Calendly "Book a Call" CTA
- `#home` (`.hero`) — hero section with headline, tagline, CTAs, and embedded photo
- `#services` (`.what`) — grid of service cards (Bookkeeping, A/R & A/P, Payroll, Systems & Automation, Operations Consulting, Trucking & Construction)
- `#about` (`.about`) — bio text and embedded photo
- `#work-with-me` (`.work`) — 3-step process plus an embedded Calendly inline scheduling iframe
- `#contact` (`.contact`) — contact cards (email, website, Calendly link, location)
- `<footer>` — copyright line

## External integrations

- **Calendly** (`calendly.com/prngbooks`): used both as a link target (nav CTA, contact section) and as an embedded inline scheduling iframe in the "Work With Me" section. Query params on the iframe URL control theming (`embed_domain`, `background_color`, `text_color`, `primary_color`) — keep these in sync with the `:root` color variables if the palette changes.
- **Google Fonts**: Roboto and Roboto Mono, loaded via a single `<link>` in `<head>`.
