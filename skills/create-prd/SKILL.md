---
name: create-prd
description: "Create a business Product Requirements Document from an 8-section template — problem, objectives, segments, value propositions, solution, release — grounded in real customer evidence (verbatim quotes, needs, pains with source links) from Evermuse. Use when the user wants to 'write a PRD', 'create a PRD', 'document product requirements', 'draft a product doc', or 'prd for [feature]'. Trigger terms: PRD, product requirements document, product doc, requirements doc, business case. Not for low-level API/interface specs — and use write-feature-spec instead when the user wants a rigorous, testable, story-by-story spec."
---

# Create a PRD (grounded in customer evidence)

Produce a comprehensive but readable Product Requirements Document — the business case for $ARGUMENTS — where the problem, the objective, the segments, and the value propositions are each backed by something a real customer actually said, with a source link. A generic PRD reads plausibly; this one reads *true*, because the "why" comes from the corpus.

> **Pick the right sibling.** `create-prd` (this skill) = a lighter **8-section business PRD** for aligning stakeholders and leadership on the what/why. `write-feature-spec` = the **rigorous, spec-driven** sibling with prioritized user stories, Given/When/Then acceptance scenarios, and measurable success criteria for a team to build from. When unsure which the user wants, offer both.

## Step 0 — Relevance & availability
Confirm this is customer-facing product work and the Evermuse tools are present. If the MCP isn't connected, produce the PRD from the template but label it **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. (See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`, Step 0.)

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For a PRD:

- **Ground.** Verify the product (Rule 1). Run **3–4 `evidence` searches** worded from different angles — the exact feature ask, the underlying pain, the adjacent workflow, and an objection/failure angle. Pull verbatim quotes with `find_supporting_quotes(topic, limit: 6)` for the sections that carry customer voice (Background, Objective, Segments, Value Props). Run **one `guidance` search** for company objectives/strategy this feature should ladder up to. Only if competitor parity is part of the value story, run **one `context` search**.
- **Work.** Fill the 8-section template in `references/prd-template.md`. Replace the source skill's "web search / user-provided data" with Evermuse evidence as the **primary** source for every "why".
- **Cite.** Every customer-derived claim (a stated pain, a demand count, a quote) carries a source badge; preserve `[^n]` markers into a Sources footer (see `citations.md`).
- **Save.** After the user confirms, `add_source(nature: "guidance", source_type: "document", title: "PRD — <feature>", tags: ["evermuse-plugin","prd"])`. A PRD is company direction → **guidance**.

## Instructions

1. **Anchor each section in evidence, not assertion.** Walk the 8 sections in `references/prd-template.md`. For each, ask what the corpus says before you write:
   - **Background / Why now** — what changed for customers? Lead with the pain, quoted. `find_supporting_quotes` on the problem.
   - **Objective + Key Results** — the objective maps to an *evidenced customer outcome*; the KRs measure *reduction of a stated pain*. (For a full OKR set, chain to `brainstorm-okrs`.)
   - **Market Segment(s)** — segments are defined by the *problem/job* customers described, not demographics. Name the accounts/personas the evidence actually came from.
   - **Value Proposition(s)** — each pain avoided / gain created is a real quote, not a guess. This is where the customer's voice does the most work.
   - **Solution** (UX, key features, tech, assumptions) — flag assumptions explicitly so the team can validate them.
   - **Release** — relative timeframes (now / next / later), never hard dates.
2. **Reuse grounding across the session.** If a spec, OKR set, or research brief already pulled this evidence, carry the quotes and source badges forward instead of re-searching (credits — Rule 7).
3. **Write for a wide audience.** Short sentences, minimal jargon; a PRD is read by leadership and engineers alike.
4. **Working-Backwards variant (optional).** If the user wants an Amazon-style press release / FAQ instead of (or before) the full PRD, use `references/wwas-press-release-template.md`. Its customer-quote sections must use **real customer quotes** from `find_supporting_quotes`, attributed — never invented testimonials.

## Deliverable
An 8-section PRD (or the Working-Backwards press release) as clean markdown, with a **Customer Evidence** callout near the top summarizing themes + demand strength, source badges inline, and a Sources footer. Offer to save it (guidance).

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT).
- [PRD template](https://www.productcompass.pm/p/prd-template) · [AI PRD template](https://www.productcompass.pm/p/ai-prd-template)
- Rigorous sibling: `${CLAUDE_PLUGIN_ROOT}/skills/write-feature-spec/SKILL.md`
