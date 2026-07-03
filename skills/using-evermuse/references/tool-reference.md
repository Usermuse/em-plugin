# Evermuse MCP — Tool Reference

Exact signatures and gotchas for all Evermuse tools. Tool names are prefixed `mcp__evermuse__` (shown here without the prefix). Every data tool is scoped to the **current Product** (set via `switch_product`); most also accept an optional `product_id` / `project_id` override.

> **Global gotchas**
> - **Product is required.** With no product selected, data calls fail with "Missing product ID". Always resolve the product first (Rule 1).
> - **Session persists per API key.** `switch_product` / `switch_project` set state that survives across calls in the session. **`switch_product` clears the current project** — re-select the project after switching products if you need it.
> - **Results truncate at ~120K characters** server-side. `search` and `see_updated_roadmap` can return 100K–175K-char payloads. Scope tightly and prefer `find_supporting_quotes` for quotes. See `search-patterns.md`.
> - **Credits are billed per call** (≈1 per own-tool call, ≈2 per third-party `call_tool`). Keep to 2–4 searches per task.

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

### `search(literal_user_question, search_query, product_id?, project_id?, nature?)`
Vector search across needs, feedback, quotes, pain points, transcript sections, competitor capabilities, and news.
- `literal_user_question` (required) — the actual user question that triggered the search.
- `search_query` (required) — an optimized retrieval query. Vary this across your 2–4 searches.
- `nature` (optional) — `evidence` | `context` | `guidance` | `all`. Declares what you're seeking. **May be silently dropped on workspaces without the lab flag** — always also triage results by their `type`.
- Returns items with `id`, `type` (`need` | `feedback` | `quote` | `pain_point`/`problem` | `transcript_section` | competitor / news), `content`, `created_at`, `footnote_marker` (`[^n]`), `marker_id`, and often `meeting_name` / `meeting_id` / `who_said_it` / `project_name`. Clusters carry `is_cluster: true` and `children`.
- **Large payloads.** If a result set is huge, read it via the saved-file path the harness returns and use `jq` to extract only the fields you need — never paste raw payloads into a deliverable.

### `find_supporting_quotes(topic, limit?, product_id?)`
Verbatim quotes for a topic. `limit` defaults to 10 — use 6–8. Each quote: `content`, `who_said_it`, `facilitator_question`, `meeting_name`, `meeting_id`, `transcript_context`, `sentiment_analysis`, `emotion`, `priority`, `footnote_marker`. **Preferred tool for showing customer voice** — small, rich, quotable.

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

### `add_source(project_id, nature, source_type, content? | file_base64?+filename?, title?, subtitle?, tags?, mime_type?)`
Persists a note or document into a Project. See `saving-to-evermuse.md` for full recipes. Requires a `project_id`. `nature`: `evidence` | `guidance` | `context`. `source_type`: `document` | `meeting_notes` | `call_transcription` | `spreadsheet` (`call_transcription` markdown-only; `spreadsheet` binary-only). Markdown mode uses `content` + `title` + optional `tags`; binary mode uses `file_base64` + `filename`.

## Third-party bridge

### `find_tool(query?, server?, offset?)` / `call_tool(connection_id, tool_name, arguments)`
Discover and call tools on workspace-connected external MCPs (Linear, Jira, GitHub, Notion, Attio, Intercom, Fireflies). Requires `mcp:thirdparty` scope — **may be blocked**. See `third-party-bridge.md` for the try-once-then-fallback pattern.

> `run_subagent` exists on some Evermuse deployments but is not available to API-key/plugin sessions — do not rely on it.
