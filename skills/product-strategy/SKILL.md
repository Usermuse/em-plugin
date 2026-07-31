---
name: product-strategy
description: >-
  Build a product strategy on the 9-section Product Strategy Canvas where every
  pillar is anchored in real customer evidence and market context, not
  guesswork. Use when the user says 'build a product strategy', 'what's our
  strategy', 'define our product direction', 'strategic plan', 'how do we win',
  or 'where should we focus'. Trigger terms: product strategy, strategy canvas,
  strategic plan, product direction, how we win, defensibility, north star. Not
  for sprint plans or individual feature specs — this is company-level
  direction.
category: Strategy & Vision
tags:
  - strategy
  - vision
  - planning
---

# Product Strategy Canvas

Produce a comprehensive Product Strategy Canvas — vision, segments, costs, value props, trade-offs, metrics, growth, capabilities, defensibility — where each pillar cites the customer pains that justify it and the market shifts that make it timely. Grounding turns a plausible-sounding strategy into one you can defend with quotes.

## Step 0 — Relevance & availability
Confirm this is product/company-direction work and Evermuse is connected. If the tools aren't present, produce a framework-only canvas labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (guidance first):** verify the product (`get_products`/`switch_product`). Then run **1 `guidance` search** for existing company strategy/objectives/values ("current strategy and objectives", "company vision and values") so you extend rather than reinvent direction. Then **2 `evidence` searches** for the top customer pains driving the strategy ("biggest recurring pain", "why customers churn / stall") and **1 `context` search** for market shifts ("competitor and market shifts", "emerging alternatives"). Pull verbatim voice with `find_supporting_quotes(topic, limit: 6–8)`.
- **Work:** fill the 9-section canvas below. Each **Market Segment** is defined by a customer problem drawn from evidence; each **Value Proposition** pillar cites the pain (evidence) it relieves AND the market shift (context) that makes it winnable now.
- **Cite:** every segment, pain, and market claim carries an inline linked-number badge [`1`](URL) — see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/citations.md`. Keep `evidence` (customer voice) separate from `context` (market) in the output.
- **Save (nature=guidance):** after the user confirms, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","strategy","product-strategy"])`.

## Instructions

You are an experienced product strategist developing a strategy for **$ARGUMENTS**. Build the canvas so its elements reinforce each other and every choice traces to evidence.

### The 9-Section Product Strategy Canvas

1. **Vision** — What we aspire to achieve and the values we uphold. Anchor in the future customers say they want (evidence quotes about desired outcomes), not internal ambition alone.
2. **Market Segments** — Defined by *problems people have*, not demographics. State each segment's Job to Be Done, desired outcome, and constraint. **Each segment's problem comes from an `evidence` result, cited.** Name the first segment and why it goes first (strongest, most-cited pain wins).
3. **Relative Costs** — Do we optimize for low cost (Southwest) or unique value (Starbucks)? State cost position vs. competitors; back competitor claims with `context`.
4. **Value Proposition** (per segment) — *What before* (the pain, quoted from evidence) → *How* (our mechanism) → *What after* (the outcome customers named) → *Alternatives* (what they use today, from evidence/context). **Every pillar cites a pain [evidence] and a market shift [context].**
5. **Trade-offs** — What we will NOT do. Use evidence to justify saying no (segments whose pains we deprioritize, and why that focus amplifies value for the chosen segment).
6. **Key Metrics** — North Star Metric + the One Metric That Matters this quarter. Tie the North Star to the customer outcome in the value prop.
7. **Growth** — Product-Led vs. Sales-Led; primary acquisition channels; unit-economics logic. Ground channel choice in where evidence shows customers actually discover tools like ours.
8. **Capabilities** — Competencies/resources to build vs. partner for, to win the chosen segment.
9. **Can't/Won't** — Why competitors can't or won't copy the *integrated* set of choices (network effects, switching costs, IP). Cite `context` on competitor capabilities where relevant (secondary — label it).

### Output process
1. Draft vision from customer-desired-future evidence.
2. Identify 2–3 segments, each grounded in a cited pain.
3. Set cost positioning.
4. Write value props with paired [evidence]+[context] citations.
5. State explicit trade-offs with rationale.
6. Set North Star + OMTM.
7. Outline growth and capabilities.
8. Explain defensibility.
9. **Coherence check:** do the nine reinforce each other? Surface the critical hypotheses that must be true, and one cheap experiment per hypothesis.

Where the source method reaches for "web search / user-provided data," use Evermuse `evidence` (customer voice) as the primary source and `context` (market) as secondary.

## Deliverable format

```markdown
# Product Strategy — [product]

**1. Vision:** [one memorable sentence] — grounded in [`1`](URL)

**2. Market Segments**
- **Segment A (first):** [JTBD]. Core pain: > "[quote]" — [attribution] [`2`](URL). Why first: [most-cited].

**4. Value Proposition — Segment A**
| Before (pain) | How | After (outcome) | Alternatives |
|---|---|---|---|
| [pain, cited [`3`](URL)] | [mechanism] | [outcome, cited [`4`](URL)] | [today's tools] |
*Market shift making this winnable now:* [context, cited [`5`](URL)]

… (sections 3, 5–9) …

**Critical hypotheses & cheapest tests**
- [hypothesis] → [test]
```

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native.
- [Product Strategy Canvas: From Vision to Action](https://www.productcompass.pm/p/product-strategy-canvas)
- [Product Vision vs Strategy vs Objectives vs Roadmap](https://www.productcompass.pm/p/product-vision-strategy-goals-and)
