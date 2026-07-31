---
name: user-personas
description: >-
  Build user personas from the real people in a company's customer corpus — 3
  evidence-backed personas with jobs-to-be-done, pains, gains, and an 'in their
  own words' verbatim quote block each. Use when the user says 'build personas',
  'create user personas', 'who are our users', 'make a persona for X', 'segment
  our users into profiles', or wants research-backed profiles instead of
  invented archetypes. Trigger terms: personas, user persona, buyer persona, who
  are our users, JTBD profiles, user archetypes. Not for fictional marketing
  avatars invented without evidence.
category: Discovery & Research
tags:
  - personas
  - users
  - research
---

# User Personas (from real people, not archetypes)

Produce 3 personas that a skeptical stakeholder can't dismiss as made up — because every trait, pain, and goal traces to a real person who appears in the Evermuse corpus, and each persona carries a block of that person's own words. A persona nobody in the corpus resembles is a failure of this skill.

## Step 0 — Relevance & availability
Confirm this is user/customer profiling work and Evermuse is connected (see `using-evermuse` Step 0). If the MCP isn't connected, produce framework-only personas labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. Never present invented archetypes as research-backed.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product (`get_products`/`switch_product`). Find out **who actually exists** before inventing anyone: `get_meetings(attendee_domain: "<customer-domain>")` (and/or `get_meetings(title_keyword: "interview")`) to see the real people and accounts in the corpus. Then run **2–3 `evidence` searches** per emerging cluster, worded around behavior and goals ("<workflow> how they do it today", "why they <goal>", "frustration with <task>"). Pull the voice with `find_supporting_quotes(topic, limit: 4–6)` for each persona.
- **Work:** cluster the real people into 3 distinct personas by shared job-to-be-done and behavior — never by demographics alone (see Instructions).
- **Cite:** every pain, gain, and the quote block carries an inline linked-number badge. Cite every customer-derived claim inline per `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/citations.md` — a linked-number code badge [`1`](URL). Attribute the verbatim quotes to the real speaker, meeting, and date.
- **Save:** `add_source(nature: "evidence", source_type: "document", tags: ["evermuse-plugin","personas","<product-slug>"])` after the user confirms.

## Instructions

Build **3 refined personas for $ARGUMENTS**, grounded entirely in the corpus.

1. **Map who's really there.** From `get_meetings`, list the distinct people/accounts. This is your raw material — personas are composites of these real users, not personas you'd expect the market to have.
2. **Cluster by job and behavior.** Group the real people by shared jobs-to-be-done, workflows, and motivations. Distinct *needs*, not distinct age brackets. Aim for 3 non-overlapping personas; if the corpus only supports 2, say so rather than padding.
3. **Enrich each persona from evidence**, using the structure below. Where the source method would reach for a survey or "if the user provides data", reach for `evidence` searches and `find_supporting_quotes` instead.
4. **Validate.** Every attribute must trace to something a real person said or did. Flag any trait you're inferring vs. one that's directly evidenced.

### Persona structure (each of the 3)

**Name & snapshot** — a memorable label + role/context (the composite of who this represents in the corpus).

**Primary job-to-be-done** — the core outcome they're trying to achieve, with context and frequency, drawn from what they described.

**Top 3 pains** — each a real obstacle, each badged with the account(s) it came from and severity.

**Top 3 gains** — the outcomes they seek and how they'd measure success.

**One unexpected insight** — a counterintuitive pattern the evidence reveals, and why it matters for the product.

**In their own words** (required) — a block of 2–4 verbatim quotes from real people in this cluster:
```markdown
> "[verbatim quote]" — [Name], [Meeting], [Date] [`1`](URL)
> "[verbatim quote]" — [Name], [Meeting], [Date] [`2`](URL)
```

**Product fit** — how $ARGUMENTS serves (or fails) this persona, with the friction points evidenced.

End with a one-line honesty note if any persona rests on thin evidence (e.g. "Persona C: only 2 accounts — validate with more interviews"). Offer `/evermuse:customer-research` to go deeper on any one persona.

---
### Further reading
- Persona method adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native. See also the [JTBD Masterclass](https://www.productcompass.pm/p/jobs-to-be-done-masterclass-with).
