---
name: sentiment-analysis
description: "Analyze the feedback corpus over a time period to surface sentiment, themes, and satisfaction shifts — with a score per theme and verbatim quotes carrying their own sentiment labels. Use when the user says 'how do users feel about X', 'what's the sentiment on Y', 'analyze feedback from last quarter', 'are customers happy', 'what are people complaining about', or wants a satisfaction/theme read across feedback. Trigger terms: sentiment analysis, how do users feel, customer satisfaction, feedback themes, complaints, are customers happy, sentiment over time. Not for analyzing a single conversation (use customer-research)."
---

# Sentiment Analysis (feedback corpus over a period)

Read the mood of the feedback corpus — for a topic, a segment, or a time window — and turn it into scored themes backed by real quotes. Not a vague "customers seem happy," but "onboarding sentiment is –0.4 this quarter, driven by setup friction, and here are the six people who said so."

## Step 0 — Relevance & availability
Confirm this is a feedback/satisfaction question spanning multiple conversations and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, produce the framework labeled **⚠ ungrounded** and tell the user to authorize the MCP. For a single conversation's sentiment, use `/evermuse:customer-research` instead.

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product. Pull the feedback body over the window: `get_notes(note_types: ["feedback","problem","quote"], date_from: "<start>", date_to: "<end>", keyword: "<topic>")`. Add **2–3 `evidence` searches** worded across the sentiment spectrum ("what customers love about <topic>", "frustration with <topic>", "why <topic> falls short"). Pull the voice with `find_supporting_quotes(topic, limit: 6–10)` — **each returned quote carries a `sentiment_analysis` field; use it** as the per-quote sentiment label rather than guessing.
- **Work:** cluster into themes, score each, split positive vs. negative drivers (see Instructions).
- **Cite:** every theme and quote badged (see `citations.md`); attach the quote's own `sentiment_analysis` label.
- **Save:** `add_source(nature: "evidence", source_type: "document", tags: ["evermuse-plugin","sentiment","feedback-analysis","<product-slug>"])` after confirmation.

## Instructions

Analyze sentiment for **$ARGUMENTS** over the specified period (or all-time if none given — state which).

1. **Inventory** the feedback pulled — count of notes, date span, distinct accounts. If the window is thin, say so before scoring.
2. **Cluster into themes** — recurring topics of praise and complaint. Aim for a handful, not one bucket.
3. **Score each theme** — a sentiment score from **–1 to +1**, derived from the `sentiment_analysis` labels on the underlying quotes, not vibes. Note the split (e.g. "mostly negative, 5 of 7 mentions").
4. **Separate drivers** — for each theme, the satisfaction drivers vs. detractors, each with a real example.
5. **Track shift over time** — if the window allows, whether sentiment on a theme is rising or falling, and against what (a release, a change).

### Output

Lead with a one-line **overall read** (net sentiment + the single biggest driver), then per theme:

```markdown
### [Theme] — sentiment [score] ([N mentions / M accounts], mostly [pos/neg])
**Loves:** [driver]
> "[verbatim]" — [Name], [Meeting], [Date] · sentiment: [positive] · [View in Evermuse](LINK) [^1]
**Frustrations:** [detractor]
> "[verbatim]" — [Name], [Meeting], [Date] · sentiment: [negative] · [View in Evermuse](LINK) [^2]
```

Then **top pain points ranked by frequency × severity**, **2-3 highest-impact recommendations**, and a **Sources** footer. Represent minority/dissenting sentiment — don't flatten a split into a false consensus. Flag any theme resting on a small sample. Offer `/evermuse:customer-research` to drill into any single theme.

---
### Further reading
- Sentiment-synthesis method adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native.
