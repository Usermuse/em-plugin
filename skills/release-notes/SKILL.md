---
name: release-notes
description: "Generate user-facing release notes / changelogs from tickets, PRDs, or git logs — organized by new features, improvements, fixes — with a 'You asked, we built' angle that finds the real customers who requested each shipped item via Evermuse. Use when the user wants to 'write release notes', 'create a changelog', 'announce what shipped', 'draft product update notes', or 'you asked we built'. Trigger terms: release notes, changelog, product update, what shipped, announcement notes, you asked we built. Not for internal-only engineering changelogs with no customer-facing change."
---

# Release Notes ("You asked, we built") — grounded in customer requests

Turn tickets/PRDs/git logs for $ARGUMENTS into polished, user-facing release notes — and make them land harder by **naming the customers who actually asked for each shipped item**. Internally, quote the requesters; externally, count them ("requested by 12 accounts"). This closes the loop and turns a changelog into evidence that you listen.

## Step 0 — Relevance & availability
Confirm these are customer-facing changes and Evermuse is present. If the MCP isn't connected, produce the notes from the raw material but label them **⚠ ungrounded** (no "you asked" attribution) and tell the user to authorize the MCP. (See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`, Step 0.)

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For release notes:

- **Ground.** Verify the product (Rule 1). For **each shipped item**, run `find_supporting_quotes(topic, limit: 3)` and `get_notes(keyword, note_types: ["need","feedback"])` to find who requested it and count requesting accounts. Keep searches tight — one pass per notable item, skip minor fixes (credits — Rule 7).
- **Work.** Categorize changes and write user-benefit-first entries, attaching the "who asked" layer.
- **Cite.** Internal version: each entry badges its requester quotes `[^n]`. External version: aggregate to a count, no names. Preserve markers into a Sources footer on the internal cut (see `citations.md`).
- **Save.** After confirmation, `add_source(nature: "guidance", source_type: "document", title: "Release notes — <version>", tags: ["evermuse-plugin","release-notes"])`. Release notes are outbound company communication → **guidance**.

## Instructions

1. **Extract from raw material.** Read all provided tickets, PRDs, git logs, changelogs. For each: what changed, who it affects, why it matters (the user benefit). Replace the source skill's "web search for product context" with the Evermuse corpus.
2. **Categorize:** New Features · Improvements · Bug Fixes · Breaking Changes (action required) · Deprecations.
3. **Write benefit-first, jargon-free.** 1–3 sentences per entry; lead with the user outcome, not the technical change. (e.g. "Dashboards load 3× faster" not "added Redis caching layer".)
4. **Add the "You asked, we built" layer per item:**
   - `find_supporting_quotes` + `get_notes` on the item's topic → the customers who asked.
   - **Internal cut:** quote 1–2 requesters verbatim, attributed and cited. [^n]
   - **External cut:** aggregate to a number — "Requested by 12 accounts" — no names or quotes.
   - If nothing in the corpus maps to an item, ship it plainly (don't invent a requester).
5. **Flag follow-up opportunities (optional).** For high-demand items, offer to notify the specific requesters that their ask shipped. This uses the third-party bridge: `find_tool` → `call_tool` to reach **Intercom / email**. **Degrade gracefully** — if the bridge is blocked or unauthenticated, produce a ready-to-send draft + the requester list instead, and tell the user the channel needs authorizing (see `third-party-bridge.md`). Never send without explicit user go-ahead.
6. **Match the product's voice** — B2B professional, consumer friendly, or developer-focused.

## Output
Two cuts when grounded: an **internal** version (with requester quotes + Sources footer) and an **external** version (aggregated counts, publish-ready). Plus an optional **follow-up list** of requesters per item.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT).
