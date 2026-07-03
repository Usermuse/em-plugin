---
name: competitive-battlecard
description: "Create a sales-ready competitive battlecard against a named competitor where the 'they say / we say' rows are ACTUAL objections and competitor mentions pulled from real customer conversations — not invented talking points. Use when the user says 'build a battlecard', 'how do we beat competitor X', 'why not competitor X', 'competitive comparison', 'handle this objection', or 'prep sales against a competitor'. Trigger terms: battlecard, competitor, competitive, objection handling, win/loss, why not X, versus, compete. Not for broad market sizing — this is a one-competitor sales asset."
---

# Competitive Battlecard

Create a concise, sales-ready battlecard against a specific competitor. The Evermuse twist: the objections and competitor claims in the card are **the real ones customers actually raised** in recorded conversations — mined verbatim — so reps counter what prospects genuinely say, not a strawman. Competitor capability data from Evermuse is used only as a labeled secondary source.

## Step 0 — Relevance & availability
Confirm this is competitive / sales-enablement work and Evermuse is connected. If the tools aren't present, produce a framework-only battlecard labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (real objections first):** verify the product (`get_products`/`switch_product`). Run **2–3 `evidence` searches** for how this competitor comes up in real calls ("[competitor] mentioned", "why they considered [competitor]", "objection we lost on", "what [competitor] does better"), and pull the verbatim lines with **`find_supporting_quotes(topic: "[competitor]", limit: 6–8)`** and **`get_notes(keyword: "[competitor]", note_types: ["feedback","problem","qa"])`**. Use `get_meetings(transcript_keyword: "[competitor]")` to find the exact deals where it surfaced.
- **Secondary (label it):** `list_competitors` / `get_competitor_capabilities` for a structured capability read — mark every such row **(secondary — Evermuse capability data, not the customer speaking)**. Never let it override an actual customer objection.
- **Work:** build the battlecard below. Each "They say / We say" row is an actual objection or competitor mention (evidence), quoted, with a grounded counter. Win/loss patterns come from what the corpus shows about deals where this competitor appeared.
- **Cite:** every objection and competitor-mention row carries a source badge (see `citations.md`). Keep customer `evidence` visibly separate from `context`/capability data.
- **Save (nature=context):** this is market/competitor output. After the user confirms, `add_source(nature: "context", source_type: "document", tags: ["evermuse-plugin","battlecard","competitive","<competitor>"])`.

## Instructions

You are creating a battlecard against **$ARGUMENTS**. Where the source method reaches for "web search / G2 / Reddit," use Evermuse `evidence` (real objections and mentions from calls) as the PRIMARY source, and competitor-capability data as clearly-labeled secondary.

Build these sections, keeping it scannable — reps reference this live on calls:

### Company overview
One-sentence positioning, target market/ICP, and what's publicly known (funding/scale). Keep it tight.

### Quick comparison
| Capability | Us | Them | Winner |
|---|---|---|---|
| [area] | [our approach] | [their approach *(secondary)*] | [Us/Them/Tie] |

### Where we win / where they win
- **We win:** [advantage] — proof: > "[customer quote]" — [attribution] [^n] (real, not asserted).
- **They win:** [their strength, from a real objection [^n]] → our counter-positioning.

### Common objections & responses — the heart of the card
| Prospect actually said | Respond with |
|---|---|
| > "[verbatim objection]" — [attribution] [^n] | [grounded counter — reframe, value/ROI, proof quote] |
Only include objections you can trace to a real conversation. If you must add a likely-but-unheard objection, mark it **(anticipated — not yet heard in corpus)**.

### Landmines to plant
Questions that expose the competitor's weaknesses — each tied to a gap customers actually named.

### Win/loss patterns
- We tend to win when: [pattern from evidence].
- We tend to lose when: [pattern from evidence].
- What tips competitive deals: [the differentiator that shows up in wins].

## Deliverable format

```markdown
# Battlecard: Us vs [Competitor]

**Overview:** [one-line positioning · target · scale]

**Quick comparison** — [table]

**Where we win**
- [advantage] — > "[quote]" — [attribution] [^1]

**Objections & responses**
| Prospect said | Respond with |
|---|---|
| > "[verbatim]" — [attribution] [^2] | [counter] |

**Win/loss:** win when [pattern [^3]] · lose when [pattern [^4]] · tipping point [diff].
---
## Sources
[^1]: …   (customer evidence)
[^2]: …
_(secondary: Evermuse competitor-capability data where marked)_
```

## Honesty when evidence is thin
If the competitor barely appears in the corpus, say so — "mentioned in 1 deal" — and lean on labeled secondary capability data while flagging that the objection set is unvalidated. Don't manufacture objections and present them as heard.

## Save & hand off
After saving, offer: **"Turn wins into positioning?"** (`/evermuse:positioning-and-messaging`), or **"Fold this into the GTM plan?"** (`/evermuse:gtm-strategy`).

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) `competitive-battlecard` (MIT); grounding is Evermuse-native.
- [How to Design a Value Proposition Customers Can't Resist?](https://www.productcompass.pm/p/how-to-design-value-proposition-template)
