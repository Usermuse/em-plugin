# Saving Back to Evermuse

Assume that **every important piece of research or every deliverable worth keeping should be saved back into Evermuse**, so the corpus compounds over time. Use `add_source`. Always offer the save and let the user confirm before writing — but do offer it; don't quietly skip it.

## The nature-of-save rule

Pick `nature` by *what the artifact is*:

| Artifact | nature |
|----------|--------|
| Customer-voice synthesis (research brief, VoC summary, persona, journey map, sentiment analysis, feature-request triage) | `evidence` |
| Market / competitor output (competitive analysis, battlecard, market sizing) | `context` |
| Company direction (feature spec, dev plan, PRD, strategy, roadmap, OKRs, pricing, positioning, gap analysis, red-team) | `guidance` |

Rationale: a spec is the company telling itself what to build (guidance); a research brief is the customer's voice captured (evidence); a competitor teardown is external market context.

## Project resolution (add_source requires a project_id)

1. Call `get_projects`.
2. **One project** → use it.
3. **Several** → ask the user once which project this belongs to (Discovery for research, Sales for deal insights, Support for tickets, or a named research project), then `switch_project` so the choice sticks for the session.
4. **None** → don't invent an ID. Skip the save and tell the user: "I couldn't save this to Evermuse — there's no project to attach it to. Create one in Evermuse and I'll save it next time."

If you switched product mid-task, re-verify the project (switching product clears it).

## Recipes

**Markdown deliverable (specs, plans, research briefs, most outputs):**
```
add_source(
  project_id: "<resolved>",
  nature: "guidance",              // per the table above
  source_type: "document",
  title: "Spec: CSV export with headers",
  subtitle: "Grounded in 7 customer mentions across 5 accounts",
  content: "<the full markdown deliverable, including its Sources footer>",
  tags: ["evermuse-plugin", "feature-spec", "csv-export"]
)
```

**Meeting/interview summary:**
```
add_source(
  project_id: "<resolved>",
  nature: "evidence",
  source_type: "meeting_notes",
  title: "Interview summary: Dana K. (Acme)",
  content: "<summary markdown>",
  tags: ["evermuse-plugin", "interview-summary"]
)
```

**Binary document the user hands you (PDF/DOCX/XLSX):** use `file_base64` + `filename` (title/tags are ignored in binary mode; `spreadsheet` source_type is binary-only). Max ~50 MB.

## Tagging convention

Every save includes `"evermuse-plugin"` plus the skill's own tag (e.g. `"feature-spec"`, `"research"`, `"gap-analysis"`) plus a slug for the subject (`"csv-export"`). Consistent tags make the saved artifacts findable later.

## Don't double-save

Within a chained workflow (spec → dev-plan → gap-analysis), each step saves its own distinct artifact. Don't re-save the same content. When a deliverable is a minor iteration on one you already saved this session, offer to update rather than create a second copy.
