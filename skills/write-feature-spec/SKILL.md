---
name: write-feature-spec
description: "Write a feature specification grounded in real customer evidence — verbatim quotes, needs, and pain points with source links from Evermuse — using spec-driven-development structure (prioritized user stories, Given/When/Then acceptance scenarios, functional requirements, measurable success criteria). Use when the user wants to write a spec, feature spec, 'spec out' a feature, or turn an idea into a rigorous specification. Trigger terms: write a spec, feature spec, specify, spec out, requirements doc, user stories. Not for low-level API/interface specs with no customer-facing behavior."
---

# Write a Feature Spec (grounded in customer evidence)

Produce a feature specification that a team can build from — with the rigor of spec-driven development **and** the customer's own voice woven through it. The difference between this and a generic spec is that every "why", every priority, and every edge case is backed by something a real customer actually said, with a source link.

> This is the rigorous, spec-driven sibling of `create-prd`. Use `write-feature-spec` when you want prioritized user stories, testable acceptance scenarios, and measurable success criteria. Use `create-prd` for a lighter 8-section business PRD. When unsure, offer both.

## Step 0 — Relevance & availability
Confirm this is customer-facing product work and the Evermuse tools are present. If the MCP isn't connected, produce the spec from the templates but label it **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. (See `using-evermuse` SKILL.md, Step 0.)

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For a spec:

- **Ground.** Verify the product (Rule 1). Then run **3–4 `evidence` searches** worded from different angles — the exact feature phrasing, the underlying pain, the adjacent workflow, and an objection/failure angle (see `search-patterns.md`). Pull verbatim quotes with `find_supporting_quotes(topic, limit: 8)`. Run **one `guidance` search** for strategy/objectives touching this area, and — only if competitor parity matters — one `context` search.
- **Work.** Draft the spec using `references/spec-template.md`.
- **Cite.** Every customer-derived claim carries a source badge; preserve `[^n]` markers into a Sources footer (see `citations.md`).
- **Save.** After the user confirms, save via `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","feature-spec","<slug>"])` (see `saving-to-evermuse.md`).

**The quote discipline.** Quotes are not decoration. Each one must *earn its place* by enriching the WHAT, the HOW, or the WHY:
- In a user story's **Why this priority** — a quote proving the demand and its intensity.
- Under a **functional requirement** — a quote that pins down how it must behave ("export with the header row, we paste straight into our template" → FR mandates headers).
- In an **edge case** — a customer who actually hit the boundary.
Never dump a wall of quotes. If evidence for a claim doesn't exist, mark the claim a **hypothesis**, don't invent a quote.

## Clarify before drafting (spec-kit discipline)
Scan for ambiguity using `references/clarification-guide.md`. Two rules:
1. **Check evidence before asking.** Often the corpus already answers the question — if three customers described the same workflow, that's your answer; cite it and move on.
2. **Cap at 3 `[NEEDS CLARIFICATION]` markers.** Present each as a short multiple-choice question, and **attach the evidence** to it: "Customers split on default scope — 3 quotes want per-project export [^2][^5][^9], 1 wants everything [^11]. Which do we serve in v1?" Resolve, then draft.

## Draft the spec
Fill `references/spec-template.md`. Beyond the standard spec-kit sections it adds:
- A **Customer Evidence** section up top: the themes you found, demand strength (counts across distinct accounts), and dissenting voices — quote-rich, badged.
- An **Evidence:** line under each prioritized user story (the quote that motivates it).
- `[^n]` markers on functional requirements that trace to a customer ask.
- Success criteria (SC-###) that are technology-agnostic and, where possible, tied to the *magnitude* of the observed pain.
- An **Assumptions** section split into "Confirmed (cited)" vs "Assumed (no signal yet)".

## Quality gate
Run `references/spec-quality-checklist.md` before finishing: every user story independently testable, P1 is a true MVP slice, every claimed customer need is either badged or explicitly marked a hypothesis, ≤3 clarification markers, success criteria measurable and tech-agnostic.

## Save & hand off
Write the spec to `specs/<feature-slug>/spec.md` in the working directory (create the folder). Save it back to Evermuse (nature `guidance`). Then offer the next step: **"Want me to turn this into a development plan?"** (`/evermuse:dev-plan`).

---
### Further reading
- Spec structure adapted from [github/spec-kit](https://github.com/github/spec-kit) (MIT).
- Evidence discipline is the core of the Evermuse plugin — see `using-evermuse`.
