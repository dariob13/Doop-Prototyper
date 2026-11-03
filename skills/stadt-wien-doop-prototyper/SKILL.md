---
name: stadt-wien-doop-prototyper
description: Build Stadt Wien (wien.gv.at) page prototypes in Doop by fetching official HTML from the Wiener Melange pattern library at handbuch.wien.gv.at. Use for any Stadt Wien prompt that should land on a Doop or Paper canvas. Doop is the primary canvas. Paper is the fallback. Not for other websites, Figma, or production implementation.
---

# Stadt Wien Doop Prototyper

Turn a City of Vienna website brief into a Doop canvas that looks like a real wien.gv.at page. The page is official Wiener Melange HTML, loaded with the theme stylesheet and component bundle. Do not redraw header, stage, cards, lists, or footer as generic divs.

This skill is an independent aid. It is not an official Stadt Wien release and it does not distribute the theme or the Wiener Melange font.

## Choose the deliverable

- **Build in Doop** when the user wants a prototype, mock, page, or design. A written plan does not finish a build request.
- **Blueprint** when the user wants a plan, specification, or component map only. Do not create a canvas just to describe a layout.
- **Paper** only when the user names Paper, or Doop is not connected after one authentication attempt. Same HTML. See [references/doop-workflow.md](references/doop-workflow.md).
- Name a new canvas `Stadt Wien Entwurf - [Topic]`, with a plain hyphen and spaces, for example `Stadt Wien Entwurf - Fahrradwerkstätte Wien`. Keep an existing canvas name unless the user asks for a rename.
- If the destination is ambiguous, keep researching sources and ask only for the missing canvas URL or tool choice.

## The rule that overrides a generic design-tool guide

Doop's own guide tells agents to invent layouts with inline CSS, stock icons, and generated images. For this skill, do the opposite of that invention:

1. Call `get_guide` once, because Doop requires it, then follow **this** skill wherever the two disagree.
2. Every visible region comes from a handbook pattern example, fetched for this task. Read [references/handbuch-fetch.md](references/handbuch-fetch.md) and [references/pattern-index.md](references/pattern-index.md).
3. The frame document loads the current theme CSS and `bundle.min.js` from `assets.wien.gv.at`. That is what makes `<wm-*>` render. The shell is in [references/page-shell.md](references/page-shell.md).
4. Change the example's text, links, and images to the page's content. Keep the tags, slots, and attributes.
5. If no pattern fits, say so and leave a labelled gap. Do not invent a compliant-looking replacement.
6. Do not use `search_icons`, `search_logos`, or `generate_image` for the Stadt Wien logo, icons, or facility photos. Icons are `<wm-icon>`. The logo SVG is copied from the header pattern.

## Authorities

Use sources in this order. Record the URL, access date, and theme version in a canvas guideline and in the handoff.

1. The live wien.gv.at page for this topic, when one exists. It wins for wording, section order, and what is actually on the page.
2. The pattern page on [handbuch.wien.gv.at/pattern-library/patterns/](https://handbuch.wien.gv.at/pattern-library/patterns/) for the markup of each region.
3. The handbook for usage rules: [patterns](https://handbuch.wien.gv.at/pattern-library/patterns/), [colors](https://handbuch.wien.gv.at/look-and-feel/farben/), [typography](https://handbuch.wien.gv.at/look-and-feel/typography/), [icons](https://handbuch.wien.gv.at/look-and-feel/icons-und-illustrationen/), [development](https://handbuch.wien.gv.at/development/anforderungen/), [editorial](https://handbuch.wien.gv.at/redaktion/).
4. The theme CSS, version from the handbook footer or [Erste Schritte](https://handbuch.wien.gv.at/development/erste-schritte/). Cached version on 2026-11-03: **v2.10.0**. Recheck it.
5. [references/foundations.md](references/foundations.md) and [references/layout-archetypes.md](references/layout-archetypes.md) when the live page and the handbook do not already settle a measurement or a sequence.

`import_webpage` of a live wien.gv.at URL is a **reference frame only**. Doop strips scripts from imports, so `<wm-*>` on that snapshot are frozen. Leave it untouched. Build the prototype in separate frames from handbook HTML.

## Build in Doop

Read [references/doop-workflow.md](references/doop-workflow.md) and follow it. Short version:

1. Connect to `https://doop.design/mcp` (or the user's self-hosted `/mcp`). If the first call returns 401, ask the user to approve Doop in the browser, then continue. Use one `agent_name` for the whole task, such as `Stadt Wien`.
2. Open the canvas the user named. Otherwise `create_canvas` with the name above. `get_canvas` before adding frames.
3. `set_guidelines` with a short guide named `Stadt Wien`: fetch handbook HTML, load the theme bundle, do not redraw patterns, German copy, no invented contacts or prices.
4. On a page called `Referenz`, import the live URL when one exists. On a page called `Prototyp`, create two frames that share one HTML document:
   - `Mobil` at width **375**
   - `Desktop` at width **1440**
   - Start the height around **3200** and correct it with `update_frame` after the screenshot.
5. Stream with `append_frame_html`: `start=true` on the document head (theme CSS, font preload, `bundle.min.js`), then one complete region per chunk (header, stage, each section, footer), `done=true` on the last chunk. The same chunks go into both frames. Viewport width selects the theme's mobile or desktop layout. Do not fork the markup unless the live page actually serves different content.
6. `get_frame_screenshot` on both frames. If tags are still unstyled, the module has not applied yet: screenshot again before editing. Fix copy and structure with `edit_frame_html`. Fix size with `update_frame`.
7. `set_prototype_start` on `Mobil`. Add `data-doop-on` / `data-doop-action` only for a journey **between** frames. Menu, carousel, accordion, and tabs belong to the component JavaScript.

Component behavior lives in the shadow DOM. Edit the light DOM you fetched (slots and attributes). Judge the look from the screenshot.

## Content

Write clear German unless asked otherwise. Prefer the live page's wording, links, and contact names. Check department names, prices, hours, dates, and statistics against a current official page and keep the source. Mark drafted sentences as draft. Unknown facts stay explicit placeholders (`[Telefonnummer prüfen]`). Never invent a plausible address, price, phone number, or opening hour.

Images on a prototype of an existing page may use that page's absolute image URLs. A new topic with no permitted photo uses a Nebelgrau-light (`#F3F1EF`) placeholder and a credit note. Maps are labelled placeholders. Do not draw a fake map or generate a photograph of a facility.

## Verify before you call it done

Compare both screenshots with the live page at the same widths: header, stage title box, tags, card grid, section bands, quicklinks, footer. Check wrapping, clipping, empty bands, and components that never upgraded.

Heading order is one H1 in the stage, then H2 per section or card. Links that leave the site keep the external icon from the link pattern. Focus styling is the theme's (3 px, interactive color). A Doop frame does not prove keyboard or screen-reader behavior; list those checks in the handoff.

## Handoff

For a build, return the canvas URL, both frame names and sizes, the theme version, the pattern URLs you fetched, what you checked on the screenshots, and any gap you left. For a blueprint, return the section sequence, the pattern URL for each section, draft content, and assumptions. Claim only the frames and facts you actually checked.
