---
name: customer-research
description: "Answer questions about what customers actually think, need, ask for, or complain about — by searching real customer evidence in Evermuse and returning a quote-rich, source-linked brief. Use when the user asks 'what do customers think about X', 'have customers asked for Y', 'what are the biggest pain points', 'who mentioned Z', 'why are users churning', or wants voice-of-customer / user-research / feedback synthesis. Trigger terms: what do customers think, customer feedback, user research, voice of customer, pain points, what are customers saying, have customers asked for."
---

# Customer Research (voice-of-customer synthesis)

The everyday workhorse: someone asks a question about customers and gets back not a plausible guess but a **quote-rich, sourced brief** drawn from real conversations. This is the skill that most visibly changes the agent's behavior after Evermuse is installed — so lean into the evidence.

## Step 0 — Relevance & availability
Confirm it's a customer question and Evermuse is connected. If not connected, say so plainly and tell the user to authorize the MCP — don't answer a "what do customers think" question from memory and pass it off as grounded.

## Scope the question
Pin down what's really being asked: a topic ("onboarding"), a decision it feeds ("should we build X"), a segment or time window. If a research **project** is clearly implied (e.g. "the audio-listeners study"), `switch_project` to it; otherwise stay at product scope.

## Ground (the heart of this skill)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` and `search-patterns.md`:
- **3–4 `evidence` searches**, worded from different angles (literal ask → underlying pain → adjacent workflow → objection).
- **`find_supporting_quotes(topic, limit: 8–10)`** for the verbatim voice.
- **`get_notes`** with `note_types`/`date_from`/`date_to` when filtering by type or period sharpens the answer (e.g. sentiment this quarter, all needs from one account).
- **`view_item`** to expand a hot item; **`get_meeting_transcript`** only if the user is asking about one specific conversation in depth.

## Synthesize
Cluster the evidence into **themes**, and for each: demand strength (mentions across distinct accounts), sentiment split, who said it and when, and the sharpest verbatim quote. Represent **dissenting voices** — don't flatten disagreement into a false consensus. Separate `evidence` (what customers said) from any `context` (market) you pulled; never blend them.

## Deliver
A brief that leads with the answer, then themes with quote blocks and source badges, then a Sources footer:

```markdown
**Short answer:** [1–2 sentences that directly answer the question.]

### Theme 1 — [name] ([N mentions / M accounts], mostly [sentiment])
> "[verbatim quote]" — [Name], [Meeting], [Date] · [View in Evermuse](LINK) [^1]
[one line of interpretation]

### Theme 2 — …

**Dissent / nuance:** [minority view, cited]
---
## Sources
[^1]: …
```

## Save & hand off
Offer to save the brief: `add_source(nature: "evidence", source_type: "document", tags: ["evermuse-plugin","research","voice-of-customer","<topic>"])`. Then offer next steps: **"Want a spec from this?"** (`/evermuse:spec`), **"Brainstorm solutions?"** (`/evermuse:brainstorm`), or **"Prioritize against the roadmap?"**

## Honesty when evidence is thin
If searches come back sparse, **say so** — "only 2 mentions, both from one account" — rather than dressing a weak signal as a trend. Thin evidence is itself a finding (maybe it's worth an interview: offer `/evermuse:interview`).

---
### Further reading
- Research method draws on [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native.
