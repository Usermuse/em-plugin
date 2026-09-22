---
description: Answer a question about what customers think, need, ask for, or complain about — a quote-rich, source-linked brief drawn from real customer conversations in Evermuse
argument-hint: "<question about customers>"
---

# /evermuse:research — Voice-of-customer brief

Start by calling the `customer_research` workflow tool with the request below — it runs the first grounding step and returns the methodology. Then:

Invoke the **customer-research** skill with `$ARGUMENTS`. Scope the question → run 3–4 varied `search` calls over evidence (vary `nature`, `note_types` and date filters) → synthesize into themes with sentiment, demand strength, and dissent → deliver a quote-rich brief with a Sources footer → offer to save (nature=evidence) and to spin into a spec.
