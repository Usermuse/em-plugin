---
name: business-model
description: "Build a Business Model Canvas — all 9 blocks — with the problem and customer-segment boxes grounded in real customer evidence and the channel/competition boxes grounded in market context. Use when the user says 'business model', 'business model canvas', 'how do we make money', 'model this venture', 'lean canvas', 'startup canvas', or 'map our business'. Trigger terms: business model, business model canvas, BMC, lean canvas, startup canvas, how we make money, revenue streams, cost structure. Not for detailed pricing tiers — use pricing-strategy for that."
---

# Business Model Canvas

Generate a Business Model Canvas whose value-creation side (problem, value prop, customer segments) is anchored in real customer evidence, and whose delivery side (channels, relationships, competition) is anchored in market context. A canvas built on quotes and market signal beats one built on assumptions. Lean Canvas and Startup Canvas are available as alternate templates in `references/`.

## Step 0 — Relevance & availability
Confirm this is business-model work and Evermuse is connected. If not, produce a framework-only canvas labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Pick the template
- **Business Model Canvas (default, below):** established businesses, corporate strategy, investor materials.
- **Lean Canvas** → `references/lean-canvas.md`: fast hypothesis testing for a new venture.
- **Startup Canvas** → `references/startup-canvas.md`: new products needing strategic clarity *and* a business model (recommended for early-stage). Ask the user which fits if ambiguous; default to BMC.

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (guidance first):** verify the product. Run **1 `guidance` search** for existing strategy/objectives ("company strategy and objectives"). Then **2 `evidence` searches** for the customer problem and who has it ("biggest recurring pain", "which customers feel this most / willingness to pay signals") to fill the **Value Proposition** and **Customer Segments** boxes. Then **1 `context` search** for channels and competition ("how customers discover tools like ours", "competitor landscape"). Pull verbatim voice with `find_supporting_quotes(topic, limit: 6)`.
- **Work:** fill all 9 blocks. The **problem/value-prop and customer-segment boxes are evidence-backed and cited**; the **channels, customer-relationships, and competitive framing are context-backed**.
- **Cite:** every problem, segment, and market claim carries a source badge (see `citations.md`); keep evidence and context separate.
- **Save (nature=guidance):** after confirmation, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","business-model","canvas"])`.

## Instructions

You are a business-model strategist building a canvas for **$ARGUMENTS**. Create, deliver, capture value — and make the blocks reinforce each other.

### The 9 blocks
**Creating value:** 1. Key Partners · 2. Key Activities · 3. Key Resources.
**Center:** 4. **Value Propositions** — the problems solved and needs met. **Grounded in cited evidence.**
**Delivering value:** 5. Customer Relationships (context) · 6. Channels — awareness → purchase → delivery → after-sales (context) · 7. **Customer Segments** — mass/niche/segmented; defining characteristics. **Grounded in cited evidence.**
**Financial viability:** 8. Cost Structure (fixed vs. variable; cost- vs. value-driven) · 9. Revenue Streams (per customer/transaction/subscription; pricing mechanism).

### Output process
1. Profile customer segments from evidence.
2. Define the value proposition(s) from cited pains.
3. Map relationships and channels from context.
4. List key activities, resources, partners.
5. Outline cost structure and revenue streams.
6. Align all 9 blocks; test economic viability (LTV > 3× CAC).
7. Surface key assumptions and risks with the cheapest test for each.

Where the source reaches for "current operations / assumptions," use Evermuse `evidence` for the problem and segment boxes and `context` for channel/competition boxes as the primary sources.

## Deliverable format
A labeled 9-block canvas (markdown table or bulleted blocks), with source badges on the value-prop, segment, channel, and competition boxes, then a Sources footer. End with the LTV/CAC sanity check and the top 3 assumptions to test.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native.
- [Business Model Canvas Examples: Google Maps, Airbnb, Uber](https://www.productcompass.pm/p/business-model-canvas-examples)
- [Startup Canvas](https://www.productcompass.pm/p/startup-canvas)
