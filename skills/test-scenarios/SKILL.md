---
name: test-scenarios
description: "Create QA test scenarios in Given/When/Then form from a spec's acceptance criteria AND from the real edge cases customers actually hit — failure stories and complaints pulled from Evermuse so the test plan covers reality, not just the happy path. Use when the user wants to 'write test scenarios', 'create test cases', 'build a test plan', 'define acceptance tests', or 'QA scenarios for [feature]'. Trigger terms: test scenarios, test cases, QA test plan, acceptance tests, Given/When/Then, edge cases. Not for unit-test code generation with no customer-facing behavior."
---

# Test Scenarios (grounded in real customer edge cases)

Build a QA test plan for $ARGUMENTS that covers two sources: the **acceptance criteria** in the spec/stories, *and* the **edge cases customers actually hit** — the failure stories, complaints, and "it broke when…" moments in the corpus. Most test plans over-test the happy path and miss the failures customers already reported; this one closes that gap.

## Step 0 — Relevance & availability
Confirm this is customer-facing feature validation and Evermuse is present. If the MCP isn't connected, derive scenarios from the acceptance criteria alone and label the plan **⚠ ungrounded — edge-case coverage from evidence unavailable**; tell the user to authorize the MCP. (See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`, Step 0.)

## Evermuse Grounding (required)

> **Search first — non-negotiable.** Your opening Evermuse retrieval MUST be **2–4 `search` calls and nothing else.** Do **not** lead with `get_notes`, `find_supporting_quotes`, `get_meetings`, `view_item`, or `get_meeting_transcript` — those may only run *after* the searches. Word the searches from different angles, and **brace for a large payload**: a `search` can exceed the ~120K-char cap and be spilled to a file — read that file selectively (see `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/search-patterns.md`), never re-run with a broader query.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For test scenarios:

- **Ground.** Verify the product (Rule 1). Source the **acceptance criteria** from the local spec (`specs/<feature>/spec.md`), the `user-stories` output, or shaping notes. Then run **2–3 `evidence` searches** on *failure* in this feature area — "broke", "didn't work", "confusing", "error", "workaround", the specific error the feature area produces — plus `find_supporting_quotes(topic, limit: 6)` and `get_notes(note_types: ["problem","feedback"])` to surface real edge cases customers reported.
- **Work.** Write scenarios in Given/When/Then, one set from each AC and one set from each evidenced failure.
- **Cite.** Every evidence-derived edge-case scenario badges the quote it came from `[^n]`; preserve markers into a Sources footer (see `citations.md`).
- **Save.** After confirmation, `add_source(nature: "guidance", source_type: "document", title: "Test scenarios — <feature>", tags: ["evermuse-plugin","test-scenarios"])`. A test plan is company direction → **guidance**.

## Instructions

1. **Two sources, clearly separated:**
   - **From acceptance criteria** — for each AC, one positive scenario (the AC holds) and its negative/deny counterpart.
   - **From evidenced edge cases** — for each real failure story in the corpus, a scenario reproducing that condition and asserting the fixed behavior. These are the high-value tests. Replace the source skill's generic "consider edge cases" with **actual customer-reported failures** as the primary edge-case source.
2. **Write each scenario in Given/When/Then** (with the fuller template for hand-off to QA):
   - **Given** starting conditions (system state, data, permissions).
   - **When** the user action / trigger.
   - **Then** the expected, observable outcome — including the negative/deny case.
3. **Tag each scenario** with source (`AC-##` or `evidence [^n]`) and priority. Edge cases customers *actually hit* rank **High** by default — they've already caused real pain.
4. **Cover the boundaries** the evidence reveals: bad input, concurrency, permission edges, limits customers bumped into.
5. **Chain from stories.** If `user-stories` ran this session, reuse its ACs and grounding rather than re-searching (credits — Rule 7).

## Scenario template
```
Test Scenario: [name]            Source: [AC-## | evidence ^n]    Priority: [High/Med/Low]
Objective: [what this validates]
Given: [system state / data / permissions]
When:  [action / trigger]
Then:  [observable outcome — include the deny/negative case]
```
Group by source (AC-derived, then evidence-derived), then a Sources footer.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT).
- Pairs with `${CLAUDE_PLUGIN_ROOT}/skills/user-stories/SKILL.md` and `${CLAUDE_PLUGIN_ROOT}/skills/write-feature-spec/SKILL.md`.
