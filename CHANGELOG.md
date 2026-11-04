# Changelog

## 1.2.0 — 2026-11-04

- No terminal needed. `dist/stadt-wien-doop-prototyper.zip` uploads in the Claude app (Customize > Skills > Upload a skill), or a Team/Enterprise owner provisions it for everyone. Doop connects as a custom connector in the app.
- `scripts/fetch_pattern.py` fetches the handbook samples. It follows the header's template link, so it copies the real header and not the "Header öffnen" link. `scripts/snapshot_patterns.py` saves an offline copy of all samples to `references/samples/`, and the skill uses it when its sandbox has no network.
- Handbook changes from 2026-11-04: `divider` is now `trenner`, there is a new pattern `ladebildschirm`, and the icon and illustration catalogs are refreshed.

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
