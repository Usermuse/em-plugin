---
name: summarize-conversation
description: "Summarize one customer interview or team meeting into structured notes — participants, needs/JTBD, verbatim quotes with timestamps, decisions, action items — by locating the conversation in Evermuse and reading its transcript. Use when the user says 'summarize this interview', 'summarize the meeting', 'recap the call', 'meeting notes', 'meeting minutes', or 'write up the interview'. Trigger terms: summarize meeting, summarize interview, meeting notes, meeting minutes, recap the call, interview summary. Not for research across many calls (that's customer-research)."
---

# Summarize a Conversation (interview or meeting)

Turn one specific conversation into a structured, accessible summary — pulling needs, decisions, and **verbatim quotes with timestamps** out of the actual transcript. This is the **sanctioned place to use `get_meeting_transcript`**: a single-conversation deep dive, not a corpus search. The summary can then be saved back so one call's insights compound into the evidence base.

## Step 0 — Relevance & availability
Confirm the user means a specific conversation (not "what do customers think about X" across many — that's `/evermuse:customer-research`) and Evermuse is connected (see `using-evermuse` Step 0). If disconnected but the user pasted a transcript, summarize that directly and label it **⚠ ungrounded — not from Evermuse** (no source links).

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify product. **Locate the conversation** with `get_meetings(attendee_domain / title_keyword / transcript_keyword / date_from / date_to)` — narrow to the one meeting the user means; if several match, list them and ask which. Then **`get_meeting_transcript(meeting_id)`** for that one conversation (this is the deep-dive exception to the "no transcripts for search" rule).
- **Work:** extract into the template below, pulling **verbatim quotes with their timestamps**.
- **Cite:** quotes carry speaker + timestamp; the summary links back to the meeting in Evermuse.
- **Save:** `add_source(nature: "evidence", source_type: "meeting_notes", tags: ["evermuse-plugin","<interview-summary | meeting-summary>","<topic>"])` after confirmation — so the extracted needs/quotes rejoin the corpus.

## Two flavors (pick by conversation type)

### A — Customer interview → discovery summary (JTBD-focused)
Tag `interview-summary`. Template:
```markdown
**Date**: […]   **Participants**: [names + roles]
**Background**: [about the customer]
**Current Solution**: [what they use today]

**What They Like** (JTBD · desired outcome · importance · satisfaction):
- …

**Problems With Current Solution** (JTBD · desired outcome · importance · satisfaction):
- …

**Key Insights / Notable Quotes**:
- > "[verbatim]" — [Speaker] [12:340] · [View](LINK)

**Action Items**: [Date · Owner · Action]
```

### B — Team / stakeholder meeting → minutes (decision-focused)
Tag `meeting-summary`. Template:
```markdown
## Meeting Summary
**Date & Time**: […]   **Participants**: [names + roles]   **Topic**: […]

**Summary**
- **Point 1**: [key discussion point or decision]
- **Point 2**: …

**Action Items**
| Due Date | Owner | Action |
|----------|-------|--------|

**Decisions Made**
- …
**Open Questions**
- …
```

## Instructions
1. **Locate, then read** the one conversation (never dump the whole corpus). If ambiguous, confirm which meeting.
2. **Read the full transcript** before writing.
3. **Extract to the right template.** Use "-" where info is missing. Preserve **timestamps** on quotes (they're the transcript's citation). Pull needs and pains as JTBD (interview) or decisions and owners (meeting).
4. **Write for clarity** — plain language a non-attendee understands; be objective, summarize what was said not opinions; use "we" for team meetings.
5. **Offer to save** so the needs/quotes rejoin the evidence base, then offer next steps (`/evermuse:opportunity-solution-tree`, `/evermuse:analyze-feature-requests`).

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT) — summarize-interview (pm-product-discovery) + summarize-meeting (pm-execution).
