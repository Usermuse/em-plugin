# Evermuse MCP — Tool Reference

Exact signatures and gotchas for all Evermuse tools. Tool names are prefixed `mcp__evermuse__` (shown here without the prefix). Every data tool is scoped to the **current Product** (set via `switch_product`); most also accept an optional `product_id` / `project_id` override.

> **Global gotchas**
> - **Product is required.** With no product selected, data calls fail with "Missing product ID". Always resolve the product first (Rule 1).
> - **Session persists per API key.** `switch_product` / `switch_project` set state that survives across calls in the session. **`switch_product` clears the current project** — re-select the project after switching products if you need it.
> - **`search` is paginated and opens with a digest.** Ask for the page size you want (`limit`, default 50) and read the digest for the shape of the whole result set. See `search-patterns.md`. A ~120K-char response cap still exists as a safety net for unusually large pages; when it bites, the digest says `size_capped: true` and the trimmed items wait at `next_offset`. `see_updated_roadmap` is **not** paginated and can still return a very large payload — prefer `search` for evidence.
> - **Credits are billed per call** (≈1 per own-tool call, ≈2 per third-party `call_tool`). Each page of search results is its own call, so page deliberately; keep to 2–4 searches per task.

## Product & project context

### `get_products()`
Lists all products in the workspace: `id`, `name`, `tagline`, `description`, `status`, `platform`, `segments`. Call this first when the product is unknown.

### `switch_product(product_id)`
Sets the current product for the session. **Clears the current project.** Returns a confirmation with the product name.

### `get_projects(all_products?)`
Lists projects for the current product (`id`, `name`, `description`). `all_products: true` groups projects across every product. Typical projects: Support, Discovery, Sales, plus named research projects.

### `switch_project(project_id)`
Narrows data scope to one project. Pass an **empty string** to clear the filter and return to full-product scope.

## Customer evidence (the voice of the customer)

### `search(literal_user_question, search_query, product_id?, project_id?, nature?, limit?, offset?)`
Vector search across needs, feedback, quotes, pain points, transcript sections, competitor capabilities, and news. **Ranked purely by relevance** — there is no per-type quota, so one search may return mostly pain points and another mostly needs. That mix is a finding, not an artifact.
- `literal_user_question` (required) — the actual user question that triggered the search.
- `search_query` (required) — an optimized retrieval query. Vary this across your 2–4 searches.
- `nature` (optional) — `evidence` | `context` | `guidance` | `all`. Declares what you're seeking. **May be silently dropped on workspaces without the lab flag** — always also triage results by their `type`.
- `limit` (optional) — page size. Default 50, max 100.
- `offset` (optional) — how many to skip. Default 0. Use the digest's `next_offset` for the following page; it reflects what was actually returned. Each call re-runs the search, so ordering can shift slightly if new data lands between pages.
- **Returns a digest first, then the items.** The digest summarizes the *full* result set — counts by type, cluster headlines (sized by the whole dataset), distinct meetings/speakers and top speakers, date range, optional nature split, and pagination state (`returned`, `offset`, `total`, `next_offset`, `size_capped`). Read it before the items: it is the fastest route to the patterns, and its counts are exact. Note the two totals: `total_results` is what you page over (a cluster is one row), `item_count` is the evidence inside them (what `counts_by_type` sums to).
- Items carry `id`, `type` (`need` | `feedback` | `quote` | `pain_point`/`problem` | `transcript_section` | competitor / news), `content`, `created_at`, `url` (deep link back into Evermuse — **cite with this**, see `citations.md`), and often `meeting_name` / `meeting_id` / `who_said_it` / `project_name`. Clusters carry `is_cluster: true`, `children` (only the members this search surfaced), and `total_items` (the cluster's true size in the DB). (Items also carry `footnote_marker` (`[^n]`) / `marker_id` — that is the first-party app's own numbering; **ignore it** and cite via `url` with your own sequential numbers.)
- **Cluster members live in `children[]`** — enumerating only top-level items skips them. See the cluster trap in `search-patterns.md`.

### `find_supporting_quotes(topic, limit?, product_id?)`
Verbatim quotes for a topic. `limit` defaults to 10 — use 6–8. Each quote: `content`, `who_said_it`, `facilitator_question`, `meeting_name`, `meeting_id`, `transcript_context`, `sentiment_analysis`, `emotion`, `priority`, `url` (cite with this — see `citations.md`). **Preferred tool for showing customer voice** — small, rich, quotable.

### `get_notes(keyword?, note_types?, meeting_id?, date_from?, date_to?, limit?, product_id?, project_id?)`
Filtered note search. `note_types`: array of `need` | `feedback` | `quote` | `problem` | `qa`. `date_from`/`date_to` are Unix ms. Use when you want to filter by type or time window rather than semantic relevance (e.g. sentiment over a quarter, all needs for a meeting).

### `get_meetings(attendee_email?, attendee_domain?, title_keyword?, transcript_keyword?, date_from?, date_to?, limit?, product_id?, project_id?)`
Find conversations/emails/ingested docs. `transcript_keyword` is semantic. `attendee_domain` (e.g. "acme.com") is great for building personas/ICPs from real accounts.

### `get_meeting_transcript(meeting_id)`
Full transcript of one meeting (or full text of one ingested document). **Long.** Deep dives only.

### `view_item(item_id, item_type)`
Detail on one item. `item_type`: `need` | `problem` | `pain_point` | `feedback` | `quote` | `opportunity`. Use to expand a specific hot item surfaced by `search`.

## Secondary / internal assets (use sparingly — not customer ground truth)

### `see_updated_roadmap(product_id?)`
Prioritized roadmap opportunities; candidates carry `Evidence` and `PRD` fields. **AI-generated — huge payload.** Cross-check only; label as AI-generated when referenced.

### `get_research_questions(project_id?, product_id?)`
The team's research goals. Use to find what's already being investigated (e.g. to target an interview script at open questions).

### `get_shaping_notes(keyword?, status?, tag?, limit?, product_id?)`
Lists collaborative markdown docs in the workspace lifecycle (statuses like Ideas → PRDs → Dev Plans → Build Notes → Revisions → Ready → Done; tags like Prompts, Interview Notes, Research Notes). Response includes the workspace's valid `available_statuses` and `available_tags`. Notes carry **branch** and **PR number** fields — the hook for `review-pr`.

### `read_shaping_note(note_ids)`
Full body of one or more shaping notes by ID: title, subtitle, status, tags, branch, PR number, markdown body.

### `list_competitors(product_id?)` / `get_competitor_capabilities(competitor_id)`
Competitor list (with threat level) and a competitor's capabilities. The list is a curated/less-reliable category — cross-check against what customers actually say (`search` for competitor mentions, win/loss quotes).

## Saving back

### `add_source(project_id, nature, source_type, …)`
Persists a note, document, meeting or communication thread into a Project. See `saving-to-evermuse.md` for full recipes. Requires a `project_id`. `nature`: `evidence` | `guidance` | `context`. Four modes:

- **Note** — `source_type` `document` | `meeting_notes` | `call_transcription`, with `content` (markdown) + `title` + optional `subtitle`/`tags`.
- **Document** — `source_type` `document` | `spreadsheet` | `meeting_notes`, with `file_base64` + `filename` (+ optional `mime_type`). Tags are rejected in this mode.
- **Meeting** — `source_type: 'meeting'`, for a real call pulled from a vendor MCP (Gong, Zoom, Fireflies, Granola…). Supply one or more of `transcript_turns` (`[{speaker, text, start_ms?}]` — speaker required on every turn), `vendor_payload` (raw payload; supported vendors only, today `gong`), `media` (`{url, type: 'video'|'audio'}`). Plus optional `participants`, `occurred_at`, `thread_id`. Produces speaker-attributed transcripts, participants, playable media and citation clips — prefer it over `call_transcription`, which only creates a flat unattributed note.
- **Communication** — `source_type` `conversation` (Slack thread / Zendesk ticket / Intercom conversation) | `message` | `email` | `email_thread`, with `messages` (`[{author, text, sent_at?}]`, preferred) or `content`.

Set `external_source` + `external_id` on anything fetched from another system: resubmitting the same `external_id` returns the existing source instead of duplicating or re-billing it.

## Third-party bridge

### `find_tool(query?, server?, offset?)` / `call_tool(connection_id, tool_name, arguments)`
Discover and call tools on workspace-connected external MCPs (Linear, Jira, GitHub, Notion, Attio, Intercom, Fireflies). Requires `mcp:thirdparty` scope — **may be blocked**. See `third-party-bridge.md` for the try-once-then-fallback pattern.

> `run_subagent` exists on some Evermuse deployments but is not available to API-key/plugin sessions — do not rely on it.
