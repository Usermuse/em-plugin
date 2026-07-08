---
name: using-evermuse
description: "Foundational doctrine and mechanics for grounding product work in real customer evidence via the Evermuse MCP — loaded by every other Evermuse skill. Use when you need the shared rules for how to search Evermuse, cite customer quotes with source links, resolve the active Product/Project, or save deliverables back via add_source. Trigger terms: Evermuse doctrine, Evermuse MCP, ground this in Evermuse, how to use Evermuse, save to Evermuse. (For an actual customer question, use customer-research; this skill supplies the mechanics other skills reuse.)"
---

# Using Evermuse

This is the shared foundation for every skill in the Evermuse plugin. It defines **how** to reach into a company's real customer evidence — recorded calls, interviews, feedback, needs, pain points, quotes — and turn any product deliverable from a generic, plausible-sounding answer into one grounded in what customers actually said, with clickable source links.

**The promise this plugin makes:** after it is installed, the agent's product answers should visibly change. They stop being generic best-practice and start carrying the customer's own voice, with quotes and citations. If your output looks the same as it would without Evermuse, you have not used this skill correctly.

## The Loop: Ground → Work → Cite → Save

Every Evermuse skill follows the same four beats. The skill supplies the *Work*; this foundation supplies the other three.

1. **Ground** — Before producing anything, pull real evidence from Evermuse (searches + supporting quotes). Never answer a customer-related question from memory when the evidence is one tool call away.
2. **Work** — Apply the skill's own method (spec template, prioritization framework, journey map, etc.), shaped by what the evidence actually says.
3. **Cite** — Every claim that rests on customer input carries a source badge. See `references/citations.md`.
4. **Save** — Offer to persist important research and deliverables back into Evermuse via `add_source`, so the corpus compounds. See `references/saving-to-evermuse.md`.

## The Seven Rules (read once, apply always)

**1. Product first, always.** Every Evermuse call is scoped to a Product. At the start of a task, call `get_products`. If exactly one product exists (or one obviously matches the repo/context), `switch_product` to it and mention which one you picked. If several plausibly match, **ask the user** which product before searching — grounding against the wrong product is worse than not grounding at all. A Project (e.g. "Discovery", "Support", "Sales") is optional; only `switch_project` when the task is scoped to one research effort. If no product exists, tell the user to set one up in Evermuse and proceed with a clearly-labeled **⚠ ungrounded** deliverable.

**2. `search` first, always — then everything else.** The **first** Evermuse retrieval on any task MUST be the `search` tool, and you MUST run **2–4 `search` calls before touching any other note-retrieval tool** — not `get_notes`, not `find_supporting_quotes`, not `get_meetings`, `view_item`, or `get_meeting_transcript`. Those tools are only allowed *after* the opening searches have run. Never rely on a single search: word the 2–4 from different angles (the literal ask, the underlying pain, the adjacent workflow, the objection) before answering. **Brace for a large payload** — a `search` can exceed the ~120K-char cap and be spilled to a file instead of returned inline; when that happens, read the file selectively (see `references/search-patterns.md`), never re-run with a broader query. For each search, decide up front what *nature* of information you want and pass it:
   - `evidence` — direct customer signal: needs, feedback, quotes, pain points, Q&A from real conversations. **This is the voice of the customer.**
   - `context` — market/industry: competitor capabilities, news, external signals.
   - `guidance` — the company's own internal direction: strategy, objectives, values, positioning.
   
   Keep the natures separate in your output — don't blend a competitor's press release with a customer's complaint. (The `nature` parameter may be silently ignored on some workspaces; regardless, triage results by their `type` field yourself so the separation always holds.)

**3. Prefer quotes for credibility.** `find_supporting_quotes(topic, limit)` returns small, rich, verbatim quotes with speaker, meeting, and sentiment. Reach for it whenever you want to *show* the customer's voice rather than summarize it. It is cheaper and sharper than a broad `search`.

**4. Transcripts are for deep dives only.** `get_meeting_transcript` returns a full, long conversation. Use it only when analyzing a single specific conversation in depth — never as a general search. For finding things across conversations, use `search` / `get_notes` / `find_supporting_quotes`.

**5. Treat AI/human-generated assets as secondary.** Shaping notes, research questions, the competitor list, and the "updated roadmap" are generated or curated *inside* Evermuse. They are useful context but are **not** ground truth about what customers need. Use them sparingly and only when specifically appropriate (e.g. a shaping note's branch/PR field during a PR review). Never cite them as if they were the customer speaking.

**6. Cite everything customer-derived, with source badges.** Assume every result may carry a resource link (deep link back into Evermuse). Render each customer-backed claim with a source badge and preserve the `[^n]` footnote markers into a Sources section. When a result has no link yet, fall back to attribution (who said it, which meeting, when). Full format in `references/citations.md`.

**7. Save what matters, and spend credits purposefully.** Important syntheses and deliverables should be offered back to Evermuse via `add_source` so the knowledge base grows. At the same time, every tool call costs credits — keep to 2–4 well-worded searches per task, low `limit` values, and reuse grounding across chained skills within a session (a spec's evidence feeds its dev plan). Don't spam searches.

## Step 0 for every skill: relevance & availability check

Before grounding, sanity-check two things:
- **Is this actually product/customer work?** If the session is clearly non-product (debugging library internals, CI config, infra plumbing with no customer-facing behavior), don't force Evermuse calls. Say so and offer the plain, ungrounded version of the task.
- **Is the MCP available?** If the `mcp__evermuse__*` (a.k.a. Evermuse) tools are not present, the MCP isn't connected/authenticated. Tell the user to authorize it (via `/mcp` in an interactive session, or their claude.ai connector settings) and produce a framework-only deliverable labeled **⚠ ungrounded — Evermuse not connected**. Never silently skip grounding without telling the user.

## Tool map (full schemas in references/tool-reference.md)

| Need | Tool |
|------|------|
| Pick / confirm the product | `get_products`, `switch_product` |
| Narrow to a research project | `get_projects`, `switch_project` |
| Find customer evidence (declare nature) | `search` |
| Pull verbatim quotes | `find_supporting_quotes` |
| Filter notes by type/date/meeting | `get_notes` |
| Find conversations | `get_meetings` |
| Deep-dive one conversation | `get_meeting_transcript` |
| Inspect one item in detail | `view_item` |
| Save research/deliverables back | `add_source` |
| Secondary/internal assets (use sparingly) | `get_shaping_notes`, `read_shaping_note`, `see_updated_roadmap`, `get_research_questions`, `list_competitors`, `get_competitor_capabilities` |
| Bridge to external tools (Linear/Jira/GitHub/Notion…) | `find_tool`, `call_tool` |

## Reference files (load only the one you need)

- `references/tool-reference.md` — every tool: exact signature, params, enums, return shape, and gotchas.
- `references/citations.md` — the source-badge format and the no-link fallback. **Load this whenever you output customer-derived claims.**
- `references/saving-to-evermuse.md` — `add_source` recipes, project resolution, and the nature-of-save rule.
- `references/search-patterns.md` — how to word 2–4 varied searches, the nature-selection table, and how to survive huge (100K+) result payloads.
- `references/third-party-bridge.md` — using `find_tool`/`call_tool` and degrading gracefully when third-party access is blocked.
