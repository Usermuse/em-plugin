---
name: competitor-analysis
description: >-
  Analyze competitors and find differentiation openings by cross-checking
  capability data against what customers ACTUALLY say about each rival —
  mentions, win/loss reasons, switching pain — with quotes. Use when the user
  says 'what do customers say about <competitor>', 'analyze our competitors',
  'competitive analysis', 'how do we compare to X', 'why do we lose to Y',
  'where can we differentiate', or wants a competitive landscape/brief. Trigger
  terms: competitor analysis, competitive landscape, competitors, win/loss,
  differentiation, how do we compare, why we lose deals. Not for internal
  team/vendor comparisons.
category: Market & Competition
tags:
  - competitors
  - analysis
  - market
---

# Competitor Analysis (what customers actually say)

Map the competitive landscape — but weight it by the customers' own words, not a feature spec sheet. The sharpest competitive intelligence isn't "Competitor X has feature Y"; it's a prospect saying *why* they picked or dropped a rival. This skill cross-checks the internal competitor data against real customer mentions and win/loss quotes.

## Step 0 — Relevance & availability
Confirm this is competitive/market work and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, produce the framework labeled **⚠ ungrounded** and tell the user to authorize the MCP.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. Two sources, ranked:
- **Primary — what customers say (evidence):** for each competitor, run an `evidence` search for **mentions** ("mentions of <competitor>", "compared us to <competitor>", "why they chose <competitor>", "switched from <competitor>") and pull `find_supporting_quotes("<competitor>", limit: 3–5)` for verbatim win/loss voice. This is the ground truth.
- **Secondary — the internal competitor list (label as less reliable):** `list_competitors` and `get_competitor_capabilities` are **AI-generated/curated inside Evermuse** — use them to enumerate the set and capability claims, but explicitly mark them *secondary* and reconcile every claim against what customers actually said. Never present a capability row as customer truth.
- **Market color (context):** 1–2 `context` searches (and web research) for positioning, pricing, funding, recent moves.
- **Cite:** customer mentions carry inline citations per `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/citations.md` — a linked-number code badge [`1`](URL); capability rows are tagged `(competitor DB — unverified)`; market facts cite their external source.
- **Save:** `add_source(nature: "context", source_type: "document", tags: ["evermuse-plugin","competitive","<product-slug>"])` after confirmation.

## Instructions

Analyze the competitive landscape for **$ARGUMENTS**.

1. **Scope the market** and assemble the competitor set: start from `list_competitors`, then add any rival that *customers actually name* in the corpus (evidence often surfaces competitors the DB misses — that's a finding).
2. **For each competitor**, build the profile below, and for every strength/weakness claim mark its source: `[customer]` (a real mention, badged), `[competitor DB]` (secondary, unverified), or `[market]` (external).
3. **Reconcile.** Where the competitor DB and customers disagree, trust the customers and say so.
4. **Find the differentiation openings** — the unmet needs and switching pains customers voice that no competitor solves well.

### Per-competitor profile

**Profile** — focus, segments served, positioning, stage `[market]`.

**Strengths** — what wins them deals. Prefer customer reasons:
```markdown
> "We went with [competitor] because [reason]" — [Name], [Meeting], [Date] [`1`](LINK)
```

**Weaknesses & gaps** — where customers are frustrated or churned away from them, badged. Add `[competitor DB]` capability gaps as secondary.

**Pricing / model** — `[market]`, cite source.

**Threat & win/loss pattern** — how they threaten us; the recurring reason we win or lose against them, from real deals.

### Differentiation opportunities (the payoff)
A ranked list of openings — each an **unmet need or switching pain customers voiced** that we could own — badged with the quote that proves it.

Close with a **competitive positioning recommendation** (differentiators to emphasize, segments to target, threats to monitor), keeping customer evidence visibly separate from competitor-DB and market sources. This feeds `/evermuse:market-sizing` (wedge) and positioning work.

---
### Further reading
- Competitive-analysis structure adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); customer cross-check is Evermuse-native.
