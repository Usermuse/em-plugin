---
name: strategy-frameworks
description: "Run a classic strategy framework — SWOT, PESTLE, Porter's Five Forces, or Ansoff Matrix — where every cell cites a real customer-evidence or market-context result instead of generic filler. Use when the user says 'do a SWOT', 'PESTLE analysis', 'Porter's five forces', 'Ansoff matrix', 'strategic assessment', 'analyze our competitive position', or 'macro environment'. Trigger terms: SWOT, PESTLE, Porter's five forces, Ansoff matrix, competitive forces, strategic analysis, macro environment, growth options. Not for product specs or roadmaps."
---

# Strategy Frameworks (SWOT · PESTLE · Porter's · Ansoff)

Run one of four classic frameworks — but grounded. The failure mode of these frameworks is generic filler ("Strength: strong brand"). Here, **every cell must cite an Evermuse `evidence` or `context` result**, or it doesn't go in the grid. A SWOT where each weakness is a customer complaint with a quote is worth ten brainstormed ones.

## Step 0 — Relevance & availability
Confirm this is strategic-analysis work and Evermuse is connected. If not, produce a framework-only grid labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Pick the framework
Ask the user (or infer from their ask) which to run, and load the matching reference:
- **SWOT** — internal strengths/weaknesses × external opportunities/threats → `references/swot.md`
- **PESTLE** — macro-environment scan (Political/Economic/Social/Technological/Legal/Environmental) → `references/pestle.md`
- **Porter's Five Forces** — industry structural attractiveness → `references/porters-five-forces.md`
- **Ansoff Matrix** — growth options across product × market → `references/ansoff.md`

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (guidance first):** verify the product. Run **1 `guidance` search** for existing strategy/objectives ("company strategy and objectives") so the framework serves the real direction. Then **2 `evidence` searches** for the customer-side cells — strengths/weaknesses/buyer power come from what customers actually praise and complain about ("what customers love / value most", "what frustrates them / makes them consider leaving"). Then **1–2 `context` searches** for the market-side cells — opportunities/threats/substitutes/forces/macro factors ("competitor and substitute landscape", "market and regulatory shifts"). Pull verbatim with `find_supporting_quotes(topic, limit: 5–6)` for customer-derived cells.
- **Work:** fill the chosen framework's grid using the reference file. **Each cell names the evidence or context result it rests on.** Customer-derived cells (evidence) stay separate from market cells (context).
- **Cite:** every cell carries a source badge (see `citations.md`).
- **Save (nature=context):** after confirmation, `add_source(nature: "context", source_type: "document", tags: ["evermuse-plugin","framework","<swot|pestle|porters-five-forces|ansoff>"])`.

## Instructions

You are a strategic analyst applying the chosen framework to **$ARGUMENTS**. Open the relevant `references/*.md` for the full cell definitions and output process, then:
1. Populate each cell **only** with items you can tie to an evidence or context result — no filler.
2. Mark each customer-derived cell with its quote/source; mark each market cell with its context source.
3. Cross-reference for strategic insight (e.g. SWOT: leverage strengths against opportunities; Porter's: which force is most binding), and end with 3–5 grounded recommendations, each traceable to the cells that motivated it.

Where the source frameworks reach for "market context / competitor data / customer feedback (optional)," use Evermuse `evidence` (customer voice) and `context` (market) as the **primary** sources for every cell.

## Deliverable format
The framework grid (table or quadrants) with a source badge in every populated cell, then the cross-referenced recommendations, then a Sources footer. If a cell has no supporting evidence or context, leave it explicitly empty and note it as a research gap rather than inventing content.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT), merging its `swot-analysis`, `pestle-analysis`, `porters-five-forces`, and `ansoff-matrix` skills; grounding is Evermuse-native.
- [The Product Management Frameworks Compendium + Templates](https://www.productcompass.pm/p/the-product-frameworks-compendium)
