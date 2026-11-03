# Doop workflow

Doop is the canvas. Endpoint: `https://doop.design/mcp` (streamable HTTP, OAuth). A self-hosted instance uses its own origin plus `/mcp`, for example `http://localhost:4300/mcp`.

Cursor, once:

```json
{
  "mcpServers": {
    "doop": { "type": "http", "url": "https://doop.design/mcp" }
  }
}
```

A 401 means the browser approval has not finished. Ask the user to authenticate Doop, then retry the same call once.

## Tools you actually use

Call `get_guide` once (`topic` `doop-instructions` when a topic is required). Then:

| Step | Tool | What to pass |
| --- | --- | --- |
| See canvases | `list_canvases` | — |
| New canvas | `create_canvas` | name `Stadt Wien Entwurf - [Topic]` |
| Read layout | `get_canvas` | before creating or editing frames |
| House rules | `set_guidelines` | name `Stadt Wien`, the hard rules from the skill |
| Pages | `set_canvas_pages` | `Referenz`, then `Prototyp`. Pass the previous page list as `expectedPages` when the tool asks for it |
| Reference only | `import_webpage` | the live wien.gv.at URL, onto `Referenz`. Do not edit that frame |
| Artboards | `create_frame` | `html` can be an empty string. `width` 375 or 1440. `height` about 3200. `pageId` of `Prototyp` |
| Stream | `append_frame_html` | `html_chunk`, `start=true` on the first chunk, `done=true` on the last |
| Small change | `edit_frame_html` | `old_str` must match once. Read `get_frame` first |
| Full replace | `set_frame_html` | only when streaming a rewrite would be larger than the edit |
| Size | `update_frame` | after the screenshot, fit the height to the page |
| Look | `get_frame_screenshot` | both frames, before you say the page is done |
| Flow start | `set_prototype_start` | the `Mobil` frame |
| Status | `set_status` | one present-tense line when the focus changes |

Keep any `data-doop-*` attribute already on an element. `data-doop-id` is how comments point at it.

Frame HTML is parsed as a document and morphed into the iframe. A `<link rel="stylesheet">` in `head` loads. A `<script src type="module">` is reinserted so it runs. `assets.wien.gv.at` sends `Access-Control-Allow-Origin: *` for the theme CSS, `bundle.min.js`, and the font (checked 2026-11-03), so the sandboxed frame can load them.

`create_frame` defaults to 640×480. Always pass 375 or 1440 and a tall height. The theme's breakpoints are 48em and 64em, so a 375 frame is the mobile layout and a 1440 frame is the desktop layout of the **same** HTML.

## What the generic Doop guide gets wrong here

- Inline CSS that restyles a card, header, or button. The theme already did that. Extra CSS is only for a gap the handbook does not cover, and it must be labelled.
- `search_icons`, `search_logos`, `list_backgrounds`, and `generate_image` for Stadt Wien chrome or official photography.
- Treating `import_webpage` as the prototype. The importer strips scripts. The snapshot is the reference beside the real frames.
- Stock photography and invented wordmarks.

## Streaming order

Send raw markup. Never HTML-escape the chunk.

1. `start=true`. The head from [page-shell.md](page-shell.md), through the opening `<body>`.
2. Header example, fetched that task.
3. Stage example.
4. Each following region as its own complete chunk (`wm-section`, grid, or equivalent).
5. Footer example. `done=true`, and close `body` and `html`.

End every chunk on a closed element. A 1–4 KB chunk is the Doop habit; a real header with the logo SVG can be larger. Send the header whole anyway. Splitting inside the logo path paints a broken mark.

Repeat the identical stream for `Desktop`. If the two frames drift, `get_frame` on `Mobil` and `set_frame_html` that document into `Desktop`, then only the width differs.

## Between-frame links

Component JS handles the open menu, carousel, accordion, and tabs inside one frame. For a second page or an explicit open-menu artboard, put this on the control and point it at the other frame id from `get_canvas`:

```html
<a href="https://www.wien.gv.at/example/"
   data-doop-on="click"
   data-doop-action="navigate:FRAME_ID"
   data-doop-transition="push-left">Weiter</a>
```

Keep a real `href` when the destination is a verified URL. Check the wiring with `get_prototype`.

## Paper, when Doop is not the canvas

Paper Desktop has to be open. Its MCP is a local stdio server (`paper` with argument `mcp`), from the MCP panel in Paper. It is not `doop.design`.

- `create_artboard` twice: `Mobil` at 375 wide, `Desktop` at 1440 wide.
- `write_html` with the same handbook document, `mode` insert or replace, `parentId` the artboard.
- `get_screenshot` on both artboards.
- `finish_working_on_nodes` when you leave an artboard.

Paper stores HTML as its own nodes. It may not run `bundle.min.js`, so `<wm-*>` can stay unupgraded. Say that in the handoff. Do not redraw the pattern as divs to force a picture. If the user needs the components to actually render, move the same HTML to Doop.
