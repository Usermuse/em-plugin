---
name: brainstorm-ideas
description: "Generate product ideas from PM, Designer, and Engineer perspectives — seeded by the real unmet needs customers actually voiced — and return an idea→evidencing-quote mapping. Use when the user asks to 'brainstorm ideas', 'what should we build', 'come up with feature ideas', 'ideate solutions', 'what could solve this problem', or 'help me think of new features' for either an existing product or a brand-new concept. Trigger terms: brainstorm, ideate, product ideas, feature ideas, come up with ideas, what should we build, solution ideas. Not for prioritizing an existing backlog (that's prioritize-features) or writing the spec."
---

# Brainstorm Product Ideas (multi-perspective, evidence-seeded)

Multi-perspective ideation (PM / Designer / Engineer) for product discovery. The difference from a generic brainstorm: every idea is **seeded by a real unmet need** pulled from Evermuse and mapped back to the customer quote that justifies it. If an idea has no evidencing quote, it's flagged as a pure hypothesis.

## Step 0 — Relevance & availability
Confirm this is product ideation and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, produce framework-only ideas labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP — but say plainly the ideas are guesses, not evidence-seeded.

## Two modes (ask which, or infer)
- **Existing product** — continuous discovery. Lean on **`nature=evidence`**: mine unmet needs, pains, and feature asks from real conversations. This is the default and the strong mode.
- **New product / new concept** — initial discovery. Evidence may be thin, so lean more on **`nature=context`** (market/competitor signals) and **`nature=guidance`** (the company's own strategy/vision), and be honest that ideas rest on hypotheses rather than a customer corpus.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product (`get_products`/`switch_product`). Run **2–4 searches** worded from different angles around $ARGUMENTS — e.g. the objective ("$ARGUMENTS the outcome"), the underlying pain, the adjacent workflow, the objection. Existing product → nature `evidence`; new concept → mix `context` + `guidance`. Then `find_supporting_quotes(topic, limit: 8–10)` to capture the verbatim unmet needs that will seed ideation.
- **Work:** the three-perspective ideation below, seeded by those needs.
- **Cite:** every idea that maps to a real need carries a source badge; ideas with no signal are flagged `hypothesis — no evidence yet`.
- **Save:** `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","ideation","<topic>"])` after the user confirms.

## Instructions

1. **Understand the opportunity.** Confirm product, objective, target segment, and desired outcome. Ask if ambiguous — don't ideate against the wrong goal.

2. **Extract the unmet needs first.** Before generating anything, distill the grounded evidence into a short list of **customer unmet needs**, each with its sharpest verbatim quote and how many distinct accounts voiced it. These are the seeds. (In new-concept mode where evidence is thin, seed instead from market gaps and strategy, and say so.)

3. **Ideate from three perspectives** — generate ~5 ideas each, each idea explicitly tied to one of the seed needs where possible:
   - **Product Manager** — business value, strategic alignment, customer impact.
   - **Product Designer** — user experience, usability, delight, onboarding.
   - **Software Engineer** — technical possibilities, data leverage, scalable/novel solutions. ("Best ideas often come from engineers." — Teresa Torres.)

4. **Prioritize the top 5** across all perspectives. Weight by: strength of the evidencing signal (how many accounts, how sharp the pain), strategic alignment, impact on the desired outcome, feasibility, and differentiation. For a **new concept**, weight instead toward core value delivery, speed to validate, and differentiation.

5. **For each of the top 5**, give: a name + one-sentence description, why it was selected (naming the evidence), and the key assumption to validate next (hand off to `/evermuse:assumptions`).

## Deliverable — the idea→evidence mapping table

Lead with the top 5, then the full idea→evidence map so nothing is unsourced:

```markdown
### Top 5 ideas
1. **[Name]** — [one-sentence description]. *Selected because:* [reason naming the need].

### Idea → evidencing quote
| Idea | Persona lens | Seed need | Evidencing quote | Accounts |
|------|--------------|-----------|------------------|----------|
| [Idea] | PM/Design/Eng | [unmet need] | > "[verbatim]" — [Name], [Meeting], [Date] · [View](LINK) [^1] | 3 |
| [Idea] | Eng | — | *hypothesis — no evidence yet* | 0 |
---
## Sources
[^1]: …
```

Be honest about thin evidence: an idea seeded by a single mention is a weaker bet than one seeded by six across distinct accounts — reflect that in the ranking, don't hide it.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT) — brainstorm-ideas (existing + new). Product Trio & Opportunity Solution Tree: Teresa Torres, *Continuous Discovery Habits*.
