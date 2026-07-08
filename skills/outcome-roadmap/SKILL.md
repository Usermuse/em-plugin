---
name: outcome-roadmap
description: "Transform a feature-list (output) roadmap into an outcome-focused one — each lane rewritten as 'Enable [segment] to [outcome] so that [business impact]' and citing the demand evidence behind it, grounded in Evermuse. Use when the user wants to 'make the roadmap outcome-focused', 'rewrite the roadmap', 'turn features into outcomes', 'make the roadmap more strategic', or 'build an outcome roadmap'. Trigger terms: roadmap, outcome roadmap, outcome-based roadmap, strategic roadmap, feature list to outcomes. Not for OKRs (use brainstorm-okrs) or a per-feature spec."
---

# Outcome-Focused Roadmap (grounded in demand evidence)

Shift $ARGUMENTS from an output roadmap (a list of features by quarter) to an **outcome roadmap** (customer/business impact by horizon), where **each outcome lane cites the demand evidence that justifies it**. A feature list says what you'll build; an outcome lane says why it matters and proves customers are asking for it.

## Step 0 — Relevance & availability
Confirm this is product-roadmap work and Evermuse is present. If the MCP isn't connected, transform the roadmap using the framework but label it **⚠ ungrounded** and tell the user to authorize the MCP. (See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`, Step 0.)

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For a roadmap:

- **Ground.** Verify the product (Rule 1). For **each initiative/lane**, run a focused **`evidence` search** on the pain it addresses + `find_supporting_quotes(topic, limit: 4)` to establish demand strength (mentions across accounts). Run **1 `guidance` search** for the company strategy the roadmap must align to. Keep to a few well-worded searches total — reuse across lanes (credits — Rule 7).
- **Work.** Rewrite each output as an outcome statement and attach its demand evidence + a proposed success metric.
- **Cite.** Every lane's demand claim carries a source badge; preserve `[^n]` into a Sources footer (see `citations.md`).
- **Save.** After confirmation, `add_source(nature: "guidance", source_type: "document", title: "Outcome roadmap — <year>", tags: ["evermuse-plugin","roadmap"])`. A roadmap is company direction → **guidance**.

## Instructions

1. **For each initiative, run the transform:**
   - **Output (old):** the feature/project as currently listed.
   - **Outcome (new):** `Enable [customer segment] to [desired customer outcome] so that [business impact]`.
   - **Demand evidence:** the quotes + mention count proving customers want this outcome. [^n]
   - **Success metric:** how you'll know the outcome landed (ties to `brainstorm-okrs` KRs if present).
2. **Let evidence reorder, not just relabel.** If a lane has thin demand evidence, flag it (**low-demand — validate or defer**); if a strongly-evidenced outcome has no lane, surface it as a **missing lane**. Replace the source skill's "web search for alignment" with **Evermuse evidence as the primary source** for what customers actually want.
3. **`see_updated_roadmap` is a labeled cross-check ONLY.** You may call `see_updated_roadmap` once to compare your evidence-built lanes against Evermuse's AI-generated roadmap — but present it explicitly as **"AI-generated cross-check (secondary, not customer ground truth)"** and never let it originate a lane or override the demand evidence. Same for shaping notes.
4. **Keep horizons flexible.** Now / Next / Later (or quarters), never hard dates. Multiple outputs can serve one outcome — focus on the outcome.
5. **Apply the "So what?" test.** For any feature you can't tie to a customer/business outcome, ask "so what?" until you reach real value — or drop it from the roadmap.

## Output
Transformed roadmap grouped by horizon. Per lane: outcome statement · demand evidence (source badge) · success metric. A short **alignment note** to company strategy [^guidance], an optional **AI cross-check** section, and a Sources footer.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT).
- [Vision vs Strategy vs Objectives vs Roadmap](https://www.productcompass.pm/p/product-vision-strategy-goals-and) · [OKRs 101](https://www.productcompass.pm/p/okrs-101-advanced-techniques) · [Business vs Product vs Customer Outcomes](https://www.productcompass.pm/p/business-outcomes-vs-product-outcomes)
