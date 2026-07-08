---
name: value-proposition
description: "Design a value proposition on the 6-part JTBD structure, then turn it into ready-to-use marketing/sales/onboarding statements — all in customers' verbatim language. Use when the user says 'write our value proposition', 'value prop', 'why should customers choose us', 'positioning statement', or 'articulate our value'. Trigger terms: value proposition, value prop, JTBD, positioning statement, marketing copy, sales messaging, customer value. Not for full pricing or business-model work."
---

# Value Proposition (JTBD + statements)

Two artifacts in one pass: a rigorous 6-part JTBD value proposition per segment, then the marketing/sales/onboarding **statements** derived from it. The pains and gains are verbatim customer language pulled from Evermuse; the statements reuse the customer's own phrasing, so the copy already sounds like the market.

## Step 0 — Relevance & availability
Confirm this is customer-value/positioning work and Evermuse is connected. If not, produce a framework-only value prop labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product. Run **1 `guidance` search** for existing positioning/values ("current positioning", "how we describe our value") so statements stay on-message. Then **2–3 `evidence` searches** for the pains and desired gains, worded from angles: the current-state friction ("what's painful about how they do this today"), the desired outcome ("what they wish they could do"), and the objection to alternatives ("why the tools they use fall short"). Pull the verbatim voice with `find_supporting_quotes(topic, limit: 6–8)` — this is the raw material for both the JTBD boxes and the statement phrasing. Add **1 `context` search** for competitive alternatives.
- **Work:** fill the 6-part template per segment (below), then generate 2–3 statements per segment.
- **Cite:** every "What before" pain and "What after" gain carries a source badge with the quote it came from (see `citations.md`).
- **Save (nature=guidance):** after confirmation, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","value-prop","positioning"])`.

## Instructions

You are a product strategist and growth expert designing the value proposition for **$ARGUMENTS**. Do one segment per pass — value props are segment-specific.

### Part A — 6-Part JTBD Value Proposition (per segment)
1. **Who** — the segment and its constraints.
2. **Why (Problem / JTBD)** — the progress they're trying to make and the desired outcome. Grounded in evidence.
3. **What Before** — current state and friction. **Quote the pain verbatim** from a `find_supporting_quotes` result; cite it.
4. **How (Solution)** — the mechanism/features that change the situation.
5. **What After** — the improved state. **Quote the desired gain verbatim** where evidence provides it; cite it.
6. **Alternatives** — what they use today and why we're better; switching friction. Ground in evidence/context.

This customer-first order (Who/Why → solution) beats solution-first canvases. Section 6 forces you to name real substitutes rather than assume none.

### Part B — Value Proposition Statements (per segment)
From the JTBD above, write 2–3 statements for marketing / sales / onboarding. Each statement:
- addresses one segment/use case directly,
- emphasizes the primary benefit and desired outcome,
- names the capability that makes it possible,
- **reuses the customer's own phrasing** from the quotes — if customers say "re-keying," don't write "manual data entry." The verbatim language is the differentiator.

Where the source method reaches for "user-provided data / brand voice files," use Evermuse `evidence` (the customer's actual words) as the primary source for both pains/gains and copy voice.

## Deliverable format

```markdown
# Value Proposition — [segment]

| Who | Why (JTBD) | What before | How | What after | Alternatives |
|---|---|---|---|---|---|
| … | … | > "[pain quote]" [^1] | … | > "[gain quote]" [^2] | … |

### Statements
- **Marketing:** [statement reusing customer phrasing]
- **Sales:** [statement]
- **Onboarding:** [statement]
---
## Sources
[^1]: …  [^2]: …
```

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT), merging its `value-proposition` and `value-prop-statements` skills; grounding is Evermuse-native.
- [How to Design a Value Proposition Customers Can't Resist?](https://www.productcompass.pm/p/how-to-design-value-proposition-template)
