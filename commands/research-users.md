---
description: Build evidence-based personas and segments from real people and conversations in the corpus
argument-hint: "<product or segment focus>"
---

# /evermuse:research-users

Start by calling the `user_personas` workflow tool with the request below — it runs the first grounding step and returns the methodology. Then:

Chain **user-personas** → **segmentation** for `$ARGUMENTS`. Personas built only from real people (`find_sources` by `attendee_domain`) with verbatim quote blocks; segments by evidence-based need differences.
