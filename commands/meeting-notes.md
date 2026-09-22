---
description: Summarize a customer meeting or interview transcript into decisions, needs, verbatim quotes, and action items
argument-hint: "<meeting name, id, or paste transcript>"
---

# /evermuse:meeting-notes

Start by calling the `summarize_conversation` workflow tool with the request below — it runs the first grounding step and returns the methodology. Then:

Invoke the **summarize-conversation** skill with `$ARGUMENTS`. Locate via `find_sources` → `read_source`, extract with timestamps, save to Evermuse (nature=evidence).
