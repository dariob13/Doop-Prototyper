# Stadt Wien Doop Prototyper

An agent skill that builds City of Vienna page prototypes in [Doop](https://doop.design/). When someone asks for a wien.gv.at page, the agent fetches the official HTML from the [Wiener Melange pattern library](https://handbuch.wien.gv.at/pattern-library/patterns/) and places it in Doop frames, with the theme stylesheet and component bundle so `<wm-*>` actually render.

[Paper](https://paper.design/) can take the same HTML when Doop is not available. The skill does not redraw the pattern library in Figma or as one-off divs.

This is an independent project, not an official Stadt Wien release. It does not include the Wiener Melange font or the theme files. Those stay on `assets.wien.gv.at`.

## Install

### Without a terminal (Claude app: Desktop or claude.ai)

1. Turn on code execution: Settings > Capabilities > "Create and edit files". On Team/Enterprise, the owner sets it in Organization settings > Plugins & skills > Policy.
2. Download [`dist/stadt-wien-doop-prototyper.zip`](https://github.com/dariob13/Doop-Prototyper/raw/main/dist/stadt-wien-doop-prototyper.zip). In Claude go to Customize > Skills, then "+" > Create skill > Upload a skill. On Team/Enterprise the owner can provision it for all members.
3. Doop: Customize > Connectors > + Add > Add custom connector, URL `https://doop.design/mcp`. In a chat, turn it on under "+" > Connectors.

The skill ships an offline copy of the handbook samples (`references/samples/`), so it also works when the app's sandbox cannot reach handbuch.wien.gv.at.

### With a terminal

Requires Node.js 22.20 or newer for the [Skills CLI](https://github.com/vercel-labs/skills).

```bash
npx skills add dariob13/Doop-Prototyper --skill stadt-wien-doop-prototyper
```

Global install for Claude Code and Codex, without prompts:

```bash
npx skills add dariob13/Doop-Prototyper --skill stadt-wien-doop-prototyper -g -a claude-code -a codex -y
```

List skills without installing:

```bash
npx skills add dariob13/Doop-Prototyper --list
```

Update with `npx skills update`.

### Standalone installer

Requires Node.js 18 or newer. Copies the skill into `~/.claude/skills/` and `~/.agents/skills/`.

```bash
npx github:dariob13/Doop-Prototyper
```

- `--claude` or `--agents` picks one of those folders
- `--dir <path>` picks another folder
- `--force` replaces an existing copy

## Connect Doop

Doop's MCP endpoint is `https://doop.design/mcp`. In Cursor, add this to `~/.cursor/mcp.json` and approve the browser login the first time:

```json
{
  "mcpServers": {
    "doop": { "type": "http", "url": "https://doop.design/mcp" }
  }
}
```

Claude Code:

```bash
claude mcp add --transport http doop https://doop.design/mcp
```

A self-hosted Doop uses that instance's `/mcp` path instead. Setup details: [doop.design/docs/get-started](https://doop.design/docs/get-started).

## Ask for a page

> Use `stadt-wien-doop-prototyper` to build a Doop prototype of the Fahrradwerkstätte page. Content source: https://www.wien.gv.at/verkehr/radfahren/

The agent should:

1. Read the live page for content and section order.
2. Fetch each region's HTML from `handbuch.wien.gv.at/pattern-library/patterns/…`.
3. Stream that document into a 375 frame and a 1440 frame on a canvas named `Stadt Wien Entwurf - [Topic]`.
4. Load `wiener-melange.bundle.min.css` and `bundle.min.js` for the current theme version (v2.10.0 on 2026-11-03).
5. Screenshot both frames and correct what does not match.

An imported copy of the live page is a reference only. Doop strips scripts on import, so the working frames are the fetched pattern HTML.

## What is in the skill

- [SKILL.md](skills/stadt-wien-doop-prototyper/SKILL.md) — when to build, the fetch-don't-redraw rule, content rules, verification
- [Doop workflow](skills/stadt-wien-doop-prototyper/references/doop-workflow.md) — tool sequence, frame sizes, streaming, Paper fallback
- [Page shell](skills/stadt-wien-doop-prototyper/references/page-shell.md) — the document head that loads the theme
- [Handbook fetch](skills/stadt-wien-doop-prototyper/references/handbuch-fetch.md) — where on a pattern page the real example HTML sits
- [Pattern index](skills/stadt-wien-doop-prototyper/references/pattern-index.md) — the 67 pattern slugs
- [Foundations](skills/stadt-wien-doop-prototyper/references/foundations.md) — tokens for checking a screenshot
- [Layout archetypes](skills/stadt-wien-doop-prototyper/references/layout-archetypes.md) — sequences taken from live pages

## Icons and illustrations

The skill only uses icons (pictograms) from <https://handbuch.wien.gv.at/look-and-feel/icons-und-illustrationen/icons/> and illustrations from <https://handbuch.wien.gv.at/look-and-feel/icons-und-illustrationen/illustrationen/>. They are cached in `references/icon-catalog.tsv` and `references/illustration-catalog.tsv`. Search with `python3 -I scripts/fetch_assets.py icons <Wort>`. When the handbook adds new ones, refresh the caches with `python3 -I scripts/fetch_assets.py --refresh`. UI icons inside component samples stay as `wm-icon`. Illustrations need sign-off from the Handbuch team.

## Paper

Paper is the second canvas, used when the prompt names Paper or Doop is not signed in. Paper Desktop must be open. Its MCP is local (`paper mcp` from Paper's MCP panel). The same handbook HTML is written into 375 and 1440 artboards. Paper may not execute the component bundle; the skill says to report that and keep the official markup.

## References

- [Handbook](https://handbuch.wien.gv.at/)
- [Pattern library](https://handbuch.wien.gv.at/pattern-library/patterns/)
- [Erste Schritte](https://handbuch.wien.gv.at/development/erste-schritte/) — current theme version and the script URL
- [wien.gv.at](https://www.wien.gv.at/)
- [Doop](https://doop.design/docs)

For a Figma kit workflow, use [dariob13/Figma-Prototyper](https://github.com/dariob13/Figma-Prototyper).
