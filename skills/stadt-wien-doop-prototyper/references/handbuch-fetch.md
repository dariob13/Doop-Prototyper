# Fetching pattern HTML

The pattern library is the source of the markup. A remembered snippet is not.

Index: <https://handbuch.wien.gv.at/pattern-library/patterns/>

Pattern URL: `https://handbuch.wien.gv.at/pattern-library/patterns/<slug>/`

Slugs are in [pattern-index.md](pattern-index.md). Fetch the **raw HTML** of the pattern page (curl or a browser). A markdown conversion of the page drops the attributes and slots you need.

## Where the example sits

Each variant is a heading followed by a tab pair.

- The variant name is the `h3` in the `wm-anchor` just above the sample.
- The copyable markup is the inner HTML of `wm-tabpanel.code-preview-rendered` (the tab labelled **Vorschau**). It is real markup, not escaped.
- `pre.language-html` (the tab labelled **Code**) is the same example run through a syntax highlighter: token spans and `&lt;` entities. Use it only if the Vorschau panel is missing, and strip the spans and decode the entities first.

Skip the handbook chrome. The page has its own `wm-header`, a title `wm-stage` named after the pattern ("Stage", "Card", …), and a sidebar `Alle Patterns`. Those are not the examples. Examples live under `.pl-samples`.

## What to keep

- The tags, including every `<wm-*>`, and the text inside them.
- Slot names (`slot="content"`, `slot="heading"`, `slot="media"`, `slot="postcontent"`, …).
- Attributes on the example (`size`, `color`, `gap`, `clickable`, `kind`, `type`, `iconid`, …). Copy the ones the variant uses. Do not add attributes you did not see on that example or on the pattern's property table.
- The logo `<svg>` inside the header example. Copy the paths. Do not redraw the shield.
- A source comment above the region:

```html
<!-- Source: https://handbuch.wien.gv.at/pattern-library/patterns/card/ — variant: Teaser -->
```

## What to change

- Visible German (or the live page's) wording, `href`s, and image URLs, so the example's demo content ("Gemeinderatswahl 2025", "Lorem ipsum", `#`) does not ship.
- Image `src` values that start with `/` — make them absolute, as [page-shell.md](page-shell.md) says.
- Nothing else. If the live page needs a region the pattern cannot express, fetch the closest pattern, keep it intact, and note the gap in the handoff.

Fetch a pattern once per task even if this skill mentions it. The handbook example wins over any template cached in an older skill or in the conversation.
