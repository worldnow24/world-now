# WORLD NOW — The World, Live on One Map. (full project)

A single-page interactive world map. Visitors click a country to see a
news panel with real country facts and demo headlines.

## Tech stack (100% free, no paid keys)
- **Map geometry:** Natural Earth data (public domain) via `world-atlas`
  (ISC license), loaded at runtime from jsDelivr (`countries-50m.json`).
- **Rendering / zoom / pan:** D3.js + `topojson-client`, both loaded from
  cdnjs.
- **Country facts** (capital, population, region, flag): REST Countries
  API (https://restcountries.com) — free, no API key required.
- **News content:** locally generated demo/mock headlines (no external
  API, no cost) — seeded per country so results are consistent.
- **Fonts:** Google Fonts (Newsreader, Inter, IBM Plex Mono).

Everything is a single self-contained file: `index.html`. No build step,
no server-side code, no database.

## Run it locally
Just open `index.html` in a browser. An internet connection is needed
(for the map data, fonts, and country-facts API — all free/public).

## Deploy it (free options)
Any static-hosting service works since this is a single HTML file:
- **Netlify / Vercel:** drag-and-drop the folder, or connect a GitHub repo.
- **GitHub Pages:** push this folder to a repo and enable Pages on the
  `main` branch.
- **Cloudflare Pages:** same drag-and-drop flow.

## Features implemented
- Zoomable / pannable map (mouse wheel, pinch, +/− buttons, reset)
- Search box to jump to any country
- Shareable deep link per country (`?country=France`)
- Live clock in the visitor's own local timezone + a small world-clock
  strip (New York, London, Dubai, Tokyo)
- Real country facts pulled from REST Countries
- Demo news panel ("wire dispatch" style), Esc-to-close, keyboard-friendly
- English UI built from a single `STRINGS` dictionary in the `<script>`
  block — add a new language by adding a new key (e.g. `ar`, `fr`) with
  translated values, no markup changes needed

## Where things live in index.html
- CSS design tokens: top of the `<style>` block (`:root { --ink: ...}`)
- `STRINGS` object: language dictionary
- `WORLD_ATLAS_URL`: map data source
- `HEADLINE_TEMPLATES` / `SOURCES`: demo news content — replace
  `generateFakeNews()` with a real API call here when ready to go live
  with real news

## Changelog
- **Selected-country card** (left side of map): shows flag, population,
  live local time (derived from the country's UTC offset via REST
  Countries `timezones`), and currency. Populated in `updateCountryCard()`,
  called from `fetchCountryFacts()`. Ticks every second via
  `tickCountryCardClock()`, hooked into the existing `tickClock()` interval.
  Hidden on narrow screens (`.country-card{display:none}` in the mobile
  media query) and closes together with the news panel.
- **Mini-map** (bottom-right overview): a second small `<svg id="miniMap">`
  reusing the same `pathGen`/projection as the main map. `buildMiniMap()`
  draws the country silhouettes once the world data loads; `updateMiniViewport()`
  draws/moves the amber viewport rectangle and is called on every `zoom`
  event from the main map. Click the mini-map to collapse/expand it.
  Also hidden on narrow screens.
- Both features are additive — no changes to the news generation, REST
  Countries base fields (only extended with `flags`, `currencies`,
  `timezones`), search, deep-linking, or zoom/pan behavior.

## design/ folder
Design tokens, component spec, and reference screenshots for continuing
visual work in Figma or handing off to a designer.
