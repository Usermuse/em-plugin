# Evermuse MCP — Tool Reference

Exact signatures and gotchas for all Evermuse tools. Your client namespaces MCP tools its own way — `mcp__evermuse__search`, a raw server id, or the bare name — so look for the tool *names* below, not for a prefix. Every product-scoped tool takes a required `product_id` and an optional `project_id`.

> **Global gotchas**
> - **`product_id` is required, per call.** You do not switch sessions — you pass `product_id` on every tool that takes it (e.g. `search`, `find_sources`, the workflow tools, `get_product_summary`, `get_opportunities`, `list_competitors`, `get_shaping_notes`, `create_shaping_note`). Choose it from the `find_skills` first-run context's `product_selection.products` (using `selected_product_id` only when present), or from `get_products`; never infer a default from list order. Omitting it fails with a "Missing product ID" error. Pass `project_id` only to narrow to one project; omit it for full-product scope.
> - **`search` and `find_sources` are paginated and open with a digest.** Ask for the page size you want (`limit`) and read the digest for the shape of the whole result set. See `search-patterns.md`. An ~80K-char response cap still exists as a safety net for unusually large pages; when it bites, the digest says `size_capped: true` and the trimmed items wait at `next_offset`, so nothing is lost. `get_opportunities` paginates too (`limit`/`offset`) and returns a compact list — use `view_item` for one opportunity's full detail.
> - **Credits are billed per call** (≈1 per own-tool call). The required grounding batch is 3–4 `evidence` searches plus one `guidance` and one `context` search; each page after that is its own call, so page on judgment (let the digest decide) rather than reflexively. Independent reads can be issued **in parallel** in one batch — parallel calls cost the same as serial ones but finish far faster.

## Product & project context

### `get_products()`
Lists all products in the workspace: `id`, `name`, `tagline`, `description`, `status`, `platform`, `segments`. Your `find_skills` first-run context already carries every product id and any genuine session selection, so you usually don't need this — call it only when that context is unavailable or you need the full metadata.

### `get_projects(all_products?)`
Lists projects for a product (`id`, `name`, `description`). `all_products: true` groups projects across every product. Typical projects: Support, Discovery, Sales, plus named research projects. Pass the resulting `project_id` to a product-scoped tool to narrow it; most work stays at full-product scope with no project.

### `get_product_summary(product_id)`
The product's latest spec as JSON — mission/vision, problem statement, primary user segments, personas, and the feature inventory with statuses. The fastest way to load product context before any product task. A product with no spec generated yet returns an error saying so: move on rather than retry. Internal framing, not customer evidence: never cite it as the voice of the customer.

## Workflow tools (start here when the job matches)

`customer_research`, `competitor_analysis`, `create_prd`, `write_brief`, `write_feature_spec`, `user_personas`, `user_stories` take the same inputs as `search`, without `workflow`; `summarize_conversation` takes the same inputs as `find_sources`. Each runs its workflow's first step and, with `include_guidance` true (the default), returns its skill's main body (methodology and next steps). When that body points to a `references/` file (templates, checklists), load it with `read_skills`, which includes references by default.

- **Read-only.** A workflow tool creates and persists nothing. `create_prd` does not create a PRD: you write it, and it is only stored if you then call a write tool such as `add_source` or `create_shaping_note`.
- **One workflow per job.** Call the matching tool once at the start; do not call it again to "continue". Deepen with `search`, `find_sources`, `read_source`, `view_item`.
- Set `include_guidance: false` only when you already hold that workflow's methodology in this thread.

## Customer evidence (the voice of the customer)

### `search(literal_user_question, product_id, search_query?, project_id?, nature?, note_types?, meeting_id?, date_from?, date_to?, limit?, offset?)`
Two modes over the same corpus — needs, feedback, quotes, pain points, custom signal types, transcript sections, competitor capabilities, and news.

- **Semantic mode** — pass `search_query`. Items are **ranked purely by relevance** (each carries `relevance`, absolute 0–100); there is no per-type quota, so one search may return mostly pain points and another mostly needs. That mix is a finding, not an artifact. Vary `search_query` across your 3–4 `evidence` searches.
- **Filters-only browse** — omit `search_query` and pass at least one of `note_types` / `meeting_id` / `date_from` / `date_to`. Returns a **recency-ordered** listing of signal notes (newest first, no relevance score). Use it to page a source's notes or a date window, not to answer a semantic question.

Params:
- `literal_user_question` (required) — the actual user question that triggered the search.
- `product_id` (required) — the product to scope to (from the first-run context).
- `search_query` (optional) — the optimized retrieval query. Present ⇒ semantic mode; absent ⇒ filters-only mode.
- `project_id` (optional) — narrow to one project; overrides the session project.
- `nature` (optional) — `evidence` | `context` | `guidance` | `all`. Declares what you're seeking; applied server-side before the vector search, so an empty filtered result is a real answer (the digest reports the unfiltered candidate count). Also triage results by their `type`.
- `note_types` (optional, filters-only) — array restricting the listing to these signal types: `need`, `feedback`, `quote`, `problem` (alias for pain point), `qa`, or a custom signal type's name. **Ignored when `search_query` is provided.**
- `meeting_id` (optional, filters-only) — restrict the listing to notes from one source (meeting/document) id.
- `date_from` / `date_to` (optional) — inclusive `created_at` bounds, Unix ms.
- `limit` (optional) — page size. Default 50, max 100.
- `offset` (optional) — how many to skip. Default 0. Use the digest's `next_offset` for the following page.
- **Returns a digest first, then the items.** The digest summarizes the *full* result set — counts by type, cluster headlines (sized by the whole dataset), distinct meetings/speakers and top speakers, date range, optional nature split, a nature-filter diagnostic when a `nature`-filtered search came back empty (how many candidates the same query matched unfiltered), and pagination state (`returned`, `offset`, `total`, `next_offset`, `size_capped`). Read it before the items. Note the two totals: `total_results` is what you page over (a cluster is one row), `item_count` is the evidence inside them (what `counts_by_type` sums to).
- Items carry `id`, `type` (`need` | `feedback` | `quote` | `pain_point`/`problem` | `transcript_section` | competitor / news / custom), `content`, `created_at`, `url` (deep link — **cite with this**, see `citations.md`), and often `meeting_name` / `meeting_id` / `who_said_it` / `project_name`. Clusters carry `is_cluster: true`, `children` (only the members this search surfaced), and `total_items` (the cluster's true size). (Items also carry `footnote_marker` (`[^n]`) / `marker_id` — the first-party app's own numbering; **ignore it** and cite via `url`.)
- **Cluster members live in `children[]`** — enumerating only top-level items skips them. See the cluster trap in `search-patterns.md`.

**Quotes.** There is no separate quotes tool: to *show* the customer's voice, run a `search` whose `search_query` is worded for verbatim reactions and keep the `quote`-type items (each carries `who_said_it`, `meeting_name`, sentiment, and a `url`). For a source's or window's quotes without semantic ranking, use `search(note_types: ["quote"], …)` in filters-only mode.

### `view_item(item_id, item_type)`
Detail on one item. `item_type`: `need` | `problem` | `pain_point` | `feedback` | `quote` | `opportunity`, or a custom signal-type name. Use to expand a specific hot item surfaced by `search`.

## Sources (conversations, documents, and other ingested records)

### `find_sources(product_id, query?, project_id?, source_types?, date_from?, date_to?, participant?, attendee_email?, attendee_domain?, title_keyword?, include_participant_details?, limit?, offset?)`
Find sources — meetings, calls, ingested documents, spreadsheets, and email/communication records — for a product.
- **Query mode** — pass `query` to rank sources by semantic match over their content. Each result carries `relevance`, `match_count`, and `top_snippet`.
- **Browse mode** — omit `query` for the most recent sources, newest first. This includes sources still processing, marked by `processing_status`.
- `source_types` (optional) — filter by category: `meeting`, `call`, `document`, `spreadsheet`, `meeting_notes`, `communication`. Omit for all.
- `date_from` / `date_to` (Unix ms), `title_keyword`.
- **People filters** — `participant` (a name or an email: "Sarah", "Sarah Chen", "sarah@acme.com"), `attendee_email` (exact address), `attendee_domain` (e.g. "acme.com" — great for building personas/ICPs from real accounts). All three match **everyone recorded on the source**: transcript speakers, the sender and recipients of an ingested email or message, *and* calendar invitees. Name matching is case-insensitive and whole-word in any order, so a first or last name alone works and a trailing partial narrows ("sarah c"). Several people filters are ANDed. "Every source Sarah is on" is a single call — everyone the source itself names is indexed, so they are found however far back the source is. Two boundaries to know: a calendar invitee who never spoke is recorded only on the meeting record and is matched only over the sources the call scans, and a person on more sources than one search reads comes back truncated to the most recent with a `note` saying so. In both cases a `date_from`/`date_to` window makes that period complete, because everything inside a scanned window is checked exhaustively.
- `limit` (default 20, max 50) and `offset` (default 0). The response opens with a **digest** — total matches, counts by source type, the date range of the page, `next_offset`, and anything this call could not see (a truncated person filter, a capped customer scan, a size-trimmed page). Read it before the items, and follow `next_offset` rather than incrementing `offset` yourself: it reflects what was **actually** returned, so paging never skips or repeats a source.
- `include_participant_details` (default false) — adds each participant's `id` and `email`. Leave it off unless you genuinely need to reconcile people across sources; you cite and read a source by its **source** id.
- Each result is a **summary**: `id`, `name`, `type`, `nature?`, `created_at`, `start?`, `participants` (everyone on the source — transcript speakers, or the people an ingested email/document named — `{name, role?}`, or `{id, name, email?, role?}` under `include_participant_details`), `attendees` (calendar invitees; empty for uploaded/API-ingested sources), `duration?`, `origin_ref_type?`, `processing_status?`, and (query mode) `relevance` / `match_count` / `top_snippet`, plus a `url`. Use `read_source` with an `id` for full content.

### `read_source(source_id, offset?, limit?)`
Full content of one source, plus its `participants` roster and `attendees` (when the source has them). Documents are returned as **titled markdown sections**; conversations as **speaker-attributed segments**. **Long** — deep dives only. Paginated: `offset` (default 0) and `limit` (default 200 segments, max 500); the response returns `total_segments`, `has_more`, and `next_offset` to page through. Returns `source_type` and a `url`, with per-segment citation markers.

## Secondary / internal assets (use sparingly — not customer ground truth)

### `get_opportunities(product_id, limit?, offset?)`
The automated product opportunities the system has suggested for a product from its signal clusters. **Machine-generated — treat with a grain of salt** and corroborate against evidence (`search` / `find_sources`) before relying on them. Label as AI-generated when referenced.
- Returns a **compact list**: `{id, title, subtitle, type, created_at, evidence_count, unique_company_count?}` per row, plus `total` / `offset` / `next_offset`. `limit` defaults to 25 (max 100).
- The PRD and the underlying evidence are **not** in the list — call `view_item(item_type: "opportunity", item_id)` for the one you care about.

### `get_shaping_notes(keyword?, status?, tag?, limit?, product_id)`
Lists collaborative markdown docs in the workspace lifecycle (statuses like Ideas → PRDs → Dev Plans → Build Notes → Revisions → Ready → Done; tags like Prompts, Interview Notes, Research Notes). Response includes the workspace's valid `available_statuses` and `available_tags`. Notes carry **branch** and **PR number** fields — the hook for `review-pr`.

### `read_shaping_note(note_ids)`
Full body of one or more shaping notes by ID: title, subtitle, status, tags, branch, PR number, markdown body.

### `list_competitors(product_id)` / `get_competitor_capabilities(competitor_id)`
Competitor list (with threat level) and a competitor's capabilities. The list is a curated/less-reliable category — cross-check against what customers actually say (`search` for competitor mentions, win/loss quotes).

## Saving back

### `add_signals(source_id, signals)` / `update_signals(updates)`
`add_signals` records 1–100 extracted observations against an existing source using a provisioned signal-type name. Identical source/type/content observations are replay-safe and return their existing IDs. `update_signals` atomically edits 1–100 IDs; editable fields are `title`, `content`, `created_at` (Unix ms), `participant_id`, `who_said_it`, and `signal_type_id`. Omitted fields remain unchanged; `null` clears the optional title or attribution fields. Both return counts and each affected signal's durable ID, resulting editable fields, and signal type ID/name; adds also say whether each signal was created or reused. Both schemas embed the workspace's active signal-type names, IDs, and descriptions, including custom types. Both require `mcp:write` on external connections.

### `add_source(project_id, nature, source_type, …)`
Persists a note, document, meeting or communication thread into a Project. See `saving-to-evermuse.md` for full recipes. Requires a `project_id`. `nature`: `evidence` | `guidance` | `context`. Four modes:

- **Note** — `source_type` `document` | `meeting_notes` | `call_transcription`, with `content` (markdown) + `title` + optional `subtitle`/`tags`.
- **Document** — `source_type` `document` | `spreadsheet` | `meeting_notes`, with `file_base64` + `filename` (+ optional `mime_type`). Tags are rejected in this mode.
- **Meeting** — `source_type: 'meeting'`, for a real call pulled from a vendor MCP (Gong, Zoom, Fireflies, Granola…). Supply one or more of `transcript_turns` (`[{speaker, text, start_ms?}]` — speaker required on every turn), `vendor_payload` (raw payload; supported vendors only, today `gong`), `media` (`{url, type: 'video'|'audio'}`). Plus optional `participants`, `occurred_at`, `thread_id`. Produces speaker-attributed transcripts, participants, playable media and citation clips — prefer it over `call_transcription`, which only creates a flat unattributed note.
- **Communication** — `source_type` `conversation` (Slack thread / Zendesk ticket / Intercom conversation) | `message` | `email` | `email_thread`, with `messages` (`[{author, text, sent_at?}]`, preferred) or `content`.

Set `external_source` + `external_id` on anything fetched from another system: resubmitting the same `external_id` returns the existing source instead of duplicating or re-billing it.

### `create_shaping_note(product_id, title, content, subtitle?, status?, tags?)` / `update_shaping_note(note_id, title?, content?, subtitle?, status?, tags?)`
Shaping notes are collaborative markdown documents (feature idea, PRD, dev plan, research note) in the Shaping tab. Not every workspace is served these; check your tool list. Call `get_shaping_notes` first for the workspace's valid status keys and existing tags; omit `status` on create to keep the note outside the pipeline, or pass `""` on update to remove it. `update_shaping_note` changes only the fields you pass, each replacing the stored value outright (`content` and `tags` included — read the note first for partial edits); every update is a revertible version, so batch related edits into one call. Shaping notes are internal thinking — use `add_source` for customer evidence.

> `run_subagent` exists on some Evermuse deployments but is not available to API-key/plugin sessions — do not rely on it.
