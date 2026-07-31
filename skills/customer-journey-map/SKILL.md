---
name: customer-journey-map
description: >-
  Map the end-to-end customer journey — awareness through advocacy — with the
  real pain point, emotion, and a verbatim customer quote at each stage, plus
  prioritized fixes. Use when the user says 'map the customer journey', 'map the
  user journey', 'where do users drop off', 'improve onboarding', 'what does the
  customer experience look like', or wants a journey/experience map grounded in
  real feedback. Trigger terms: customer journey, journey map, user journey,
  drop-off, onboarding friction, moments of truth, experience map. Not for
  internal process/workflow diagrams with no customer involved.
category: Discovery & Research
tags:
  - journey-map
  - ux
  - customer-experience
---

# Customer Journey Map (evidence at every stage)

Map the journey from first awareness to advocacy — and make each stage carry a real customer's words about the friction there. A journey map with plausible-but-invented pains is just a diagram; this one shows, stage by stage, where real users actually struggle and what they said about it.

## Step 0 — Relevance & availability
Confirm this is a customer-experience question and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, produce the framework labeled **⚠ ungrounded** and tell the user to authorize the MCP.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product. For **each stage**, pull the pain in the customer's voice: `get_notes(note_types: ["problem","feedback"], keyword: "<stage keyword>")` — e.g. keyword `signup`/`trial` for Acquisition, `onboarding`/`setup`/`first` for Onboarding, `cancel`/`churn` for Retention. Reinforce with a stage-scoped `evidence` search and `find_supporting_quotes("<stage> friction", limit: 3–5)`. That's roughly 2 focused calls per painful stage — concentrate on the stages the user cares about, don't grind all seven.
- **Work:** map stages, then attach the real pain + quote + emotion to each (see Instructions).
- **Cite:** every pain point and quote carries an inline citation per `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/citations.md` — a linked-number code badge [`1`](URL), attributed to speaker/meeting/date.
- **Save:** `add_source(nature: "evidence", source_type: "document", tags: ["evermuse-plugin","journey-map","<product-slug>"])` after confirmation.

## Instructions

Build the journey map for **$ARGUMENTS**.

1. **Name the traveler.** Use a specific persona with a JTBD, not a generic user — reuse one from `/evermuse:user-personas` if it exists in the session.
2. **Map the stages** (adapt to the product): Awareness → Consideration → Acquisition → Onboarding → Engagement → Retention → Advocacy.
3. **For each stage, document** — grounding each pain in the notes/quotes you pulled, not in intuition:
   - **Touchpoints** — where they interact (site, email, in-app, support).
   - **User actions** — what they do here.
   - **Thoughts & questions** — what's on their mind.
   - **Emotion** — how they feel (emoji or –2…+2 scale), inferred from the sentiment of real feedback where available.
   - **Pain point** — the real friction, **badged with a quote**.
   - **Opportunity** — how to fix it.
4. **Mark the critical moments:** the **aha moment** (first real value), **moments of truth** (commit/abandon), and **churn triggers** — anchor each to evidence.

### The map

```markdown
| Stage | Touchpoint | Action | Emotion | Pain (evidence) | Opportunity |
|---|---|---|---|---|---|
| Onboarding | in-app setup | connects data source | 😣 –1 | "I gave up wiring it in on day one" — [Name], [Mtg], [Date] [`1`](URL) | guided setup wizard |
```

Under the table, put a **stage-by-stage quote reel** — the sharpest verbatim line per painful stage — then **prioritized improvements** (highest impact on conversion/retention first; quick wins vs. deeper bets). Every quote carries its inline badge.

If a stage has no evidence in the corpus, say so explicitly ("no feedback captured for Advocacy") rather than inventing a pain — that gap is itself a finding. For a visual map, suggest recreating this in FigJam/Miro.

---
### Further reading
- Journey-map structure adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native. See [User Journey Mapping 101](https://www.productcompass.pm/p/user-journey-mapping-101).
