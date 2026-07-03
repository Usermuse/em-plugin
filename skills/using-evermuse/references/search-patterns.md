# Search Patterns

How to get sharp, grounded results out of Evermuse without burning credits or drowning in 100K-char payloads.

## Triangulate: 2–4 searches, worded differently

One search finds one facet. Real grounding comes from attacking the topic from several angles. For a feature or topic, run searches like:

1. **The literal ask** — the user's own phrasing ("bulk export to CSV").
2. **The underlying pain** — the problem behind it ("manual re-keying of data / copy-paste into spreadsheets").
3. **The adjacent workflow** — where it lives ("end-of-month reporting", "handoff to finance").
4. **The objection / negative** — friction and complaints ("export is broken / missing columns / too slow").

Different wording surfaces different items because retrieval is by vector similarity. Stop at 2–4; more than that rarely adds signal and costs credits.

## Declare the nature you want

| You're looking for… | nature | Example query |
|---|---|---|
| What customers said/need/feel | `evidence` | "frustration with onboarding data entry" |
| Market, competitors, news | `context` | "competitor pricing for bulk export" |
| Company strategy/objectives/values | `guidance` | "our stated goals for the reporting area" |

Set `nature` per search so the sets stay clean. Because the param can be silently dropped, **also** triage what comes back by each item's `type` — keep `quote`/`need`/`feedback`/`pain_point` (evidence) apart from competitor/news items (context).

## Quotes: use the right tool

To *show* the customer's voice, call `find_supporting_quotes(topic, limit: 6-8)` — it's smaller and richer than `search` and returns speaker + meeting + sentiment ready to cite. Use `search` to *map the landscape* (what themes/needs exist); use `find_supporting_quotes` to *pull the pull-quotes*.

## Filtering instead of searching

When the question is about a **type** or a **time window**, `get_notes` beats `search`:
- All needs mentioned in one meeting → `get_notes(meeting_id, note_types:["need"])`.
- Sentiment shift this quarter → `get_notes(date_from, date_to, note_types:["feedback","problem"])`.

## Surviving huge payloads

`search` and `see_updated_roadmap` can exceed the ~120K-char cap and get saved to a file by the harness instead of returned inline. When that happens:
- **Don't** re-run with a broader query. **Do** read the saved file selectively.
- Use `jq` to pull just what you need, e.g. counts by type, or the top items' `content` + `who_said_it` + `footnote_marker`:
  ```
  jq -r '.data[] | select(.type=="quote") | "\(.footnote_marker) \(.who_said_it): \(.content)"' <file>
  jq -r '[.data[].type] | group_by(.)[] | "\(length) \(.[0])"' <file>
  ```
- Keep `limit` low and pass `project_id` to pre-scope when you already know the project.
- **Never** paste a raw payload into a deliverable — extract, cite, discard.

## Reuse across a session

Grounding is expensive; reuse it. A spec's evidence pass should feed its dev plan and its gap analysis. Within one session, remember what you already searched and cite it again rather than re-querying.
