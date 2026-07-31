---
name: beachhead-segment
description: >-
  Pick the first beachhead market segment by measuring which segment's people
  show up most and most painfully in real customer evidence, then evaluating it
  on burning pain, willingness to pay, winnable share, and reachability. Use
  when the user says 'who should we target first', 'what's our beachhead',
  'which segment do we start with', 'where do we focus first', 'pick a first
  market', or 'initial market entry'. Trigger terms: beachhead, first market,
  initial segment, target first, market entry, where to focus, crossing the
  chasm. Not for a broad ICP definition — this is the single first wedge.
category: Segmentation & Targeting
tags:
  - beachhead
  - segmentation
  - gtm
  - focus
---

# Beachhead Segment

Pick the smallest winnable, referenceable first market — the wedge that gets you to PMF fastest and opens adjacent expansion. The Evermuse twist: instead of brainstorming segments in the abstract, you **measure demand density in the actual corpus** — which segment's people show up most often and most painfully in real conversations — then pressure-test the front-runner against four criteria, each backed by evidence.

## Step 0 — Relevance & availability
Confirm this is a first-market / segmentation task and Evermuse is connected. If the tools aren't present, produce a framework-only analysis labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (demand density first):** verify the product (`get_products`/`switch_product`). To gauge which segments actually show up, call **`get_meetings(attendee_domain: …)`** across the candidate segments' domains/industries and note volume and recency — who is in the room most. Then run **2–3 `evidence` searches** for the acute pain per candidate segment ("who is most desperate about X", "which role feels this daily", "willing to pay to fix"), and pull verbatim voice with **`find_supporting_quotes(topic, limit: 6–8)`**. Add **1 `context` search** on competitive saturation per segment ("who already serves segment Y"). `get_notes(note_types: ["need","problem"])` filtered by keyword helps rank pain intensity.
- **Work:** rank candidate segments by demand density (frequency + intensity of pain in the corpus), then score the front-runners on the four criteria below — each criterion backed by a cited quote or a count of distinct accounts.
- **Cite:** every pain, willingness-to-pay, and reachability claim carries an inline citation per `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/citations.md` — a linked-number code badge [`1`](URL). Keep `evidence` separate from `context`.
- **Save (nature=guidance):** after the user confirms, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","beachhead","gtm","segmentation"])`.

## Instructions

You are choosing the beachhead for **$ARGUMENTS**. Where the source method reaches for "customer interviews / market research," use the Evermuse corpus as the primary source: demand density from `get_meetings`, pain from `evidence` searches and quotes.

### Step 1 — Read demand density from the corpus
List the candidate segments (verticals, company sizes, roles, use cases). For each, gauge presence in the evidence:
- **Frequency** — how many distinct accounts/meetings feature this segment (`get_meetings` by `attendee_domain`).
- **Intensity** — how burning the pain reads in their quotes (`find_supporting_quotes`, `get_notes`).
The segment with the most people showing up, most strongly, is your demand-density front-runner — it earns the burden of proof, not an automatic win.

### Step 2 — Score the front-runners on four criteria
For the top 2–3 candidates, evaluate each criterion and **back it with evidence**:

1. **Burning pain** — acute, daily, unmet? Cite the sharpest pain quote and count how many accounts echo it. Thin evidence is a finding, not a gap to paper over.
2. **Willingness to pay** — is there budget, clear ROI, and no free workaround that fully satisfies? Cite any quotes about cost of the problem or current spend.
3. **Winnable share** — can you realistically take 60–70% in 3–18 months? Assess competitive saturation (context) and your differentiation; large enough but not owned.
4. **Reachability** — can you actually reach and refer within this segment? Look for professional communities, network effects, and adjacency to segments already in the corpus (a solved beachhead should reference into the next market).

### Step 3 — Select and plan
Choose the segment with the best combined picture: strongest cited pain, clear willingness to pay, winnable, reachable, and a natural bridge to adjacent segments. Then outline a 90-day acquisition plan for the beachhead and a post-beachhead expansion path (which adjacent segment the references unlock next).

## Deliverable format

```markdown
# Beachhead — [product]

**Demand density ranking**
| Segment | Accounts in corpus | Pain intensity | Note |
|---|---|---|---|
| [seg] | [N meetings/domains] | [high/med/low] | [one line] |

**Recommended beachhead: [segment]**

| Criterion | Verdict | Evidence |
|---|---|---|
| Burning pain | [strong/weak] | > "[quote]" — [attribution] [`1`](URL) ([M accounts]) |
| Willingness to pay | … | [cited [`2`](URL)] |
| Winnable share | … | [competitive context, cited [`3`](URL)] |
| Reachability | … | [cited [`4`](URL)] |

**Why first:** [1–2 sentences]. **90-day acquisition plan:** […]. **Expansion path:** [next adjacent segment, why].
```

## Honesty when evidence is thin
If a candidate segment barely appears in the corpus, say so plainly — "only 2 meetings, one account" — rather than inflating it into a market. A segment with no evidence isn't a beachhead; it's a hypothesis worth an interview (`/evermuse:interview` if available).

## Save & hand off
After saving, offer: **"Build the full ICP for this segment?"** (`/evermuse:ideal-customer-profile`), **"Plan the launch into it?"** (`/evermuse:gtm-strategy`), or **"Position for this segment?"** (`/evermuse:positioning-and-messaging`).

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) `beachhead-segment` (MIT); grounding is Evermuse-native.
- Based on Geoffrey Moore's beachhead strategy in *Crossing the Chasm*.
- [5 GTM Principles You Should Know as a PM](https://www.productcompass.pm/p/5-gtm-principles-with-frameworks-templates)
- [How to Achieve Product-Market Fit? Part I](https://www.productcompass.pm/p/how-to-achieve-the-product-market)
