---
name: prioritize-features
description: "Rank a backlog of feature ideas using real demand counts from customer evidence — Reach and Impact come from how many distinct accounts actually asked, not a gut number — and return a top-5 with the quotes behind each score. Use when the user asks to 'prioritize features', 'what should we build next', 'rank this backlog', 'score these ideas', 'RICE this', 'make scope decisions', or 'which feature first'. Trigger terms: prioritize, prioritization, rank features, backlog, RICE, ICE, what to build next, scope decision, score features. Not for open-ended ideation (that's brainstorm-ideas)."
---

# Prioritize Features (scored on real demand)

Rank a backlog to find the top 5 to pursue. The Evermuse difference: **Reach and Impact aren't gut numbers** — they come from counting how many distinct accounts actually asked, and how sharply, in real conversations. A backlog scored on evidence looks different from one scored on the loudest stakeholder.

## Step 0 — Relevance & availability
Confirm this is backlog prioritization and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, you can still apply the frameworks, but every Reach/Impact number is a guess — label the output **⚠ ungrounded** and say the scores are unvalidated.

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify product. For **each feature/theme in the backlog**, run a targeted `evidence` search and `find_supporting_quotes(topic, limit: 6–8)` to get the **actual demand count** — distinct accounts that raised it — and the sharpest quote. `get_notes(note_types:[need,feedback])` helps tally asks over a period. Keep to a few well-worded searches per feature (credits).
- **Work:** score with a framework from `references/prioritization-frameworks.md`, feeding the real counts in.
- **Cite:** every Reach/Impact figure links to the evidence that produced it.
- **Save:** `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","prioritization","<topic>"])` after confirmation.

## Instructions

1. **Confirm the objective and success metric.** Prioritization is meaningless without the outcome you're ranking against. Ask if it's not stated.

2. **Ground each item in demand.** For every feature, pull its real signal: how many distinct accounts asked, the sentiment, and one verbatim quote. Prioritize **problems (opportunities), not solutions** — "never allow customers to design solutions."

3. **Score with an evidence-fed framework** (details in `references/prioritization-frameworks.md`):
   - **Reach** = number of distinct accounts affected — **from the evidence count**, not a guess.
   - **Impact** = Opportunity Score (Importance × (1 − Satisfaction)) where the corpus reveals importance and dissatisfaction — value per customer.
   - **Confidence** = how sure are we? Higher when the evidence is thick and consistent, lower when it's one account.
   - **Effort** = dev/design/coordination cost.
   RICE = (R × I × C) / E · ICE = I × C × E · Opportunity Score for problem-ranking.

4. **`see_updated_roadmap` is a labeled cross-check only.** You may pull Evermuse's AI-generated roadmap to sanity-check your ranking, but present it explicitly as **"AI-generated cross-check (not customer ground truth)"** — never let it override the evidence-backed scores. Same caution for `get_research_questions`.

5. **Recommend the top 5** with ranking, rationale naming the evidence, key trade-offs, and **what was deprioritized and why**.

## Deliverable

```markdown
**Objective:** [outcome being ranked against]

| # | Feature | Reach (accounts) | Impact (Opp. Score) | Conf | Effort | Score | Evidence |
|---|---------|------------------|---------------------|------|--------|-------|----------|
| 1 | Scheduled export | 6 | 0.72 | High | M | … | > "[quote]" [^1] |
| 2 | … | 3 | 0.55 | Med | S | … | [^2] |

**Deprioritized:** [feature] — [why, e.g. "only 1 account, low importance"].

**AI-generated cross-check (not ground truth):** roadmap suggests … — [agrees/differs with evidence ranking].
---
## Sources
[^1]: …
```

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT) — prioritize-features; frameworks reference adapted from pm-execution/prioritization-frameworks. Opportunity Score: Dan Olsen, *The Lean Product Playbook*.
