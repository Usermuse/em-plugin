---
name: market-sizing
description: "Estimate market size (TAM, SAM, SOM) with top-down and bottom-up approaches, grounded in market/industry signals and validated against a real customer beachhead. Use when the user says 'how big is the market', 'what's our TAM', 'size this opportunity', 'estimate the addressable market', 'prep market size for the pitch', or is evaluating market entry. Trigger terms: market size, TAM, SAM, SOM, addressable market, market opportunity, market sizing, beachhead. Not for internal usage/revenue analytics of an existing product."
---

# Market Sizing (TAM / SAM / SOM)

Estimate the opportunity with defensible top-down and bottom-up numbers — and, crucially, ground the **wedge** (which slice you actually win first) in real customer evidence. Most market-sizing decks are context-only guesses; the Evermuse edge is proving the beachhead is real with customers who already want it.

This is the showcase skill for **nature=context**: the market numbers come from external/industry signals, and customer evidence is used to *validate the wedge*, not to size the whole market.

## Step 0 — Relevance & availability
Confirm this is a market-opportunity question and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, produce the sizing framework labeled **⚠ ungrounded** (external numbers still possible via web research, but the wedge won't be evidence-validated) and tell the user to authorize the MCP.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill the natures split cleanly:
- **Ground (context — the market):** verify the product, then run **2–3 `context` searches** for market/industry signals ("<market> size", "<industry> growth", "<segment> spend on <category>"). Supplement with web research / analyst reports for TAM inputs where the corpus is thin — label external figures with their source.
- **Ground (evidence — the wedge only):** run **1–2 `evidence` searches** + `find_supporting_quotes` to confirm a real, urgent beachhead ("who is desperate for this", "willing to pay for <capability>"). This validates SOM/SAM assumptions — it does **not** size TAM.
- **Work:** triangulate top-down and bottom-up; scope SAM/SOM; project growth (see Instructions).
- **Cite:** market figures cite their external source; wedge claims carry customer source badges (see `citations.md`). Keep the two visibly separate — never let a customer quote masquerade as a market number, or vice versa.
- **Save:** `add_source(nature: "context", source_type: "document", tags: ["evermuse-plugin","market-sizing","<product-slug>"])` after confirmation.

## Instructions

Size the market for **$ARGUMENTS** within the stated constraints (geography, vertical, customer type).

1. **Market definition** — the problem space, segment boundaries, geography, scoping decisions.
2. **TAM** — top-down (industry total → relevant slice, cite sources) *and* bottom-up (customers × price × frequency) to cross-validate; reconcile the two.
3. **SAM** — the portion realistically serviceable given product, channels, language, pricing tier; as a % of TAM with reasoning.
4. **SOM** — achievable share in 1-3 years given competitive position and GTM capacity; **this is where the customer wedge validates the number** — if evidence shows a hot beachhead, cite it as the basis for near-term obtainability.
5. **Growth projection** — 2-3 year evolution and the drivers/trends behind it.
6. **Assumptions & risks** — numbered, each with a confidence level (high/med/low) and how to validate it.

### Summary table

```markdown
| Metric | Current estimate | 2-3 yr projection | Basis |
|--------|------------------|-------------------|-------|
| TAM | | | [external source] |
| SAM | | | product/channel constraints |
| SOM | | | GTM capacity + evidenced wedge [^1] |
```

### Wedge validation (the Evermuse differentiator)
A short block: **which slice do we win first, and who in the corpus already wants it?** 1-2 verbatim quotes proving urgency:
```markdown
> "[verbatim quote showing pull/urgency]" — [Name], [Meeting], [Date] · [View in Evermuse](LINK) [^1]
```

Close with **key assumptions & risks** and a **Sources** footer separating market sources from customer evidence. Be explicit about what's data vs. estimate, and where confidence intervals are wide.

---
### Further reading
- TAM/SAM/SOM method adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); wedge-validation grounding is Evermuse-native. See [Market Research: Advanced Techniques](https://www.productcompass.pm/p/market-research-advanced-techniques).
