---
name: positioning-and-messaging
description: "Generate differentiated positioning options and campaign/message ideas written in your customers' own mined vocabulary, where every message maps to a real quote that proves it resonates. Use when the user says 'how should we position this', 'position the product', 'differentiate from competitors', 'write our messaging', 'brainstorm marketing campaigns', 'promote the product', or 'what's our positioning'. Trigger terms: positioning, positioning statement, messaging, differentiation, value proposition, marketing ideas, campaign ideas, tagline, how to position. Not for a full launch plan — use gtm-strategy for that."
---

# Positioning & Messaging

Produce positioning options and campaign/message ideas — but written in the **customer's own mined vocabulary**, not marketer-speak. The Evermuse twist: every positioning angle and every campaign message maps to a verbatim quote that proves real people talk that way and care about that thing. A message you can't attach to a quote is a guess; cut it.

## Step 0 — Relevance & availability
Confirm this is positioning / marketing work and Evermuse is connected. If the tools aren't present, produce framework-only positioning labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (mine the vocabulary):** verify the product (`get_products`/`switch_product`). Run **2–3 `evidence` searches** for how customers describe the pain, the win, and the alternatives in their own words ("how they describe the problem", "the phrase they use for the outcome", "what they compared us to"), and pull the actual phrasing with **`find_supporting_quotes(topic, limit: 8–10)`** — this is the raw material for the language. Add **1 `context` search** on competitor positioning ("how competitors position themselves", "gaps competitors leave open") to find unclaimed territory.
- **Work:** generate positioning options and campaign ideas below, each written in mined phrasing and **each mapped to a proof quote**. Positioning claims a territory competitors leave open; campaigns carry the message into a channel.
- **Cite:** every positioning statement and campaign message carries a source badge to the quote that grounds it (see `citations.md`). Keep `evidence` (customer voice) separate from `context` (competitor positioning).
- **Save (nature=guidance):** positioning is company direction. After the user confirms, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","positioning","messaging"])`.

## Instructions

You are developing positioning and messaging for **$ARGUMENTS**. Where the source method reaches for "brainstorm from context," use Evermuse `evidence` as the primary source — the customer's actual words — and `context` (competitor positioning) as secondary.

### Part A — Positioning options (differentiated)

**A1. Competitive landscape.** Identify the top competitors (from `context` + `list_competitors` if useful, labeled secondary). For each: their positioning angle, audience focus, differentiators they emphasize, and the gap they leave open.

**A2. Generate 5 positioning options.** Each must be differentiated from competitors, resonate with the mined customer values/needs, emphasize a capability competitors downplay, and claim unclaimed territory. For each, provide:
1. **Positioning statement** — e.g. "The only [category] for [segment] who want to [outcome]" — using the customer's own words for [outcome].
2. **Strategic rationale** — why it resonates and differentiates.
3. **Supporting messages** — reinforcing lines.
4. **Competitive advantage** — the capability that lets you own this claim.
5. **Proof quote** — > "[verbatim]" — [attribution] [^n]. If no quote supports it, the option is speculative — mark it.

### Part B — Campaign / message ideas

Generate **5 creative, cost-effective campaign ideas** to carry the chosen positioning to the target segment. For each:
1. **Channel** — primary channel (content, social, community, partnerships, email…).
2. **Core message** — in mined customer vocabulary.
3. **Why it works** — grounded in a customer quote about what they care about ([^n]).
4. **Cost efficiency** — what makes it high-impact on a limited budget.

Prioritize high-impact-low-budget and unconventional angles. Every core message must map to a proof quote — that mapping is the whole point.

## Deliverable format

```markdown
# Positioning & Messaging — [product]

## Positioning options
### Option 1 — [name]
**Statement:** [The only … for … who want to …]
**Rationale:** [why]. **Advantage:** [capability]. **Territory:** [unclaimed gap vs [competitor], [^context]].
**Proof:** > "[verbatim]" — [attribution] [^1]
… (options 2–5) …

## Campaign ideas
| # | Channel | Core message (mined) | Why it works | Proof quote |
|---|---|---|---|---|
| 1 | [channel] | "[message]" | [reason] | > "[quote]" — [attribution] [^n] |
---
## Sources
[^1]: …
```

## Honesty when evidence is thin
If the corpus doesn't yield distinctive customer language, say so — generic phrasing is a signal the positioning isn't validated yet. Flag which messages rest on real quotes vs. which are hypotheses to test.

## Save & hand off
After saving, offer: **"Fold into the launch plan?"** (`/evermuse:gtm-strategy`), **"Build a battlecard for the top competitor?"** (`/evermuse:competitive-battlecard`), or **"Sharpen the ICP behind this?"** (`/evermuse:ideal-customer-profile`).

---
### Further reading
- Merged and adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) `positioning-ideas` + `marketing-ideas` (MIT); grounding is Evermuse-native.
- [How to Design a Value Proposition Customers Can't Resist?](https://www.productcompass.pm/p/how-to-design-value-proposition-template)
- [Product Management vs. Product Marketing vs. Product Growth 101](https://www.productcompass.pm/p/product-management-vs-product-marketing)
