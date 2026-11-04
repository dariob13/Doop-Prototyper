# Changelog

## 1.1.0 — 2026-11-04

- Icons and illustrations: only the official handbook sources are allowed. Icons come from <https://handbuch.wien.gv.at/look-and-feel/icons-und-illustrationen/icons/> (228 pictograms) and illustrations from <https://handbuch.wien.gv.at/look-and-feel/icons-und-illustrationen/illustrationen/> (219).
- New `scripts/fetch_assets.py` (search and `--refresh`), `references/icons-illustrations.md`, and cached catalogs `references/icon-catalog.tsv` and `references/illustration-catalog.tsv`.
- UI icons inside fetched samples stay as `wm-icon`. Chosen content icons are pictogram `<img>`s. Illustrations are flagged for Handbuch-team sign-off.

## 1.0.0 — 2026-11-03

First release of `stadt-wien-doop-prototyper`.

- Doop is the canvas. Frames are 375 and 1440, sharing one HTML document so the theme's breakpoints apply.
- Component markup is fetched from the handbook pattern pages (`code-preview-rendered`), not redrawn.
- The frame head loads the Wiener Melange stylesheet and `bundle.min.js` from `assets.wien.gv.at`.
- Paper is documented as the fallback and uses the same HTML.
- Pattern index covers the 67 handbook slugs listed on 2026-11-03. Theme version cached in the shell is v2.10.0.
