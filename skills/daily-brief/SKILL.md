---
name: daily-brief
description: >-
  Write and deliver the recurring Evermuse daily brief — everything Evermuse
  learned from customer sources in the last 24 hours, quoting customers only and
  never the internal team. Use when the scheduled daily report task fires, when
  the user runs find_skills:daily-brief, or asks 'send my daily brief', 'what did
  Evermuse learn yesterday', 'today's customer digest', 'run the daily report'.
  Trigger terms: daily brief, daily report, daily digest, what's new from
  customers, last 24 hours, morning report. (Scheduled by setup-evermuse step 3.)
category: Discovery & Research
tags:
  - evermuse
  - report
  - daily
---

# Daily Brief

The recurring digest of what Evermuse learned from the company's customer sources in the last 24 hours, delivered to a Slack channel, a DM, or an email. It is normally created as a scheduled task by `setup-evermuse` step 3 and then runs unattended, every day, with nobody watching — so everything below has to hold without a human in the loop.

Read `using-evermuse` first and follow its grounding rules; this skill only adds what is specific to the brief.

**The brief's whole value is that it carries the customer's voice.** A brief that quotes the product team back at the product team is worse than no brief: it launders internal opinion into something that looks like evidence. The rule below is therefore not a preference — it is the thing this skill exists to get right.


## Rule 1: internal voices never appear as customer evidence

Nobody on the user's own team — founders, PMs, designers, engineers, sales reps, CSMs, support agents, the facilitator running the interview — may be quoted, counted, or summarized as customer input in this brief. Their words are the company talking to itself.

Work it in three steps, in this order.

### 1a. Establish the internal roster before you search

Resolve who is internal from the most reliable source available, and stop at the first one that gives you a usable answer:

1. **A roster you were handed.** If the conversation or the task prompt happens to name the internal email domains, or teammates whose email sits off-domain (contractors on gmail, an agency address), that list is authoritative — use it as-is and do not second-guess it. **Usually there is no such list.** A scheduled brief is launched by a bare one-line prompt that says nothing but "read this skill and run it", so expect to resolve the roster yourself from the sources below. That is the normal path, not a degraded one.
2. **The host's own identity and directory tools.** The connected account's own email domain, the Slack workspace member directory, calendar organizers, the CRM's user list.
3. **Attendee emails on the sources themselves.** `find_sources` returns each source's `attendees[]` with `name` and `email`. Every attendee on an internal domain is internal; the rest are external. This step is worth running even when step 1 or 2 already answered the question, because it hands you the **name spellings that `who_said_it` actually uses** — that is what you match against later. Harvest them with one **`attendee_domain`-filtered** call per internal domain (`find_sources(product_id, date_from, date_to, attendee_domain: "<domain>", limit: 50)`): a filtered call scans far deeper into the window than an unfiltered browse does, so it is the reliable way to catch every teammate who appeared. Then read the exhaustion rule in "Pull the window" below before you trust the roster to be complete.
4. **Structural tells — tie-breakers only, never a basis for including a quote.** On a `qa` signal the question is the team's side by construction and the answer is the customer's, so the customer voice is the signal's `content` and never the question wrapped around it. A speaker who recurs across sources belonging to many different customer accounts is almost certainly internal. A speaker whose turns are mostly questions is usually the facilitator.

Build the roster once, at the top of the run, and reuse it for the whole brief.

### 1b. Classify every speaker before you quote them

Every signal from `search` carries `who_said_it` and `meeting_id`. For each candidate quote:

- Normalize both sides before comparing — trim, casefold, drop titles and trailing role suffixes ("Dana K. (Acme)" → "dana k"). Match on the full name; match on a first name only when it is unambiguous within that source's attendee list.
- Resolve through the signal's `meeting_id` to that source's `attendees[]` when the bare name is ambiguous.
- **A speaker you cannot place is not a customer.** Do not quote them. You may still count the source in the volume line, but an unattributed line never becomes a featured quote.

### 1c. What to do with internal material

- **Drop wholly internal sources entirely.** If every attendee on a source is internal — a standup, a planning call, an internal Slack channel, a spec or strategy doc — it contributes nothing to the brief but the "sources processed" count.
- **Drop internal turns from mixed sources.** A customer call has the team on it too. Keep the customer's turns, drop the team's, including the facilitator's questions.
- **Never let internal material into a tally.** "7 mentions across 5 accounts" must count external speakers only. An internal person repeating a customer's complaint is not a second mention of it.
- **`guidance`-nature items are internal by definition** — company strategy, objectives, positioning — as are shaping notes, AI-suggested opportunities, and the competitor list. Never dress any of them as a customer quote (see `references/citations.md`).
- **Internal material may appear as labeled context, once, and never in a quote block.** "The team committed to a fix on that call" is a legitimate line; the same sentence inside quote marks attributed as customer voice is not.


## Rule 2: run it end to end, unattended

A scheduled brief fires when nobody is watching, so a brief that pauses is a brief that never arrives.

- **Never stop to ask a question.** If something is ambiguous — which product, which channel, whether a name is internal — make the safest choice, continue, and note the choice in one line at the bottom of the brief. The safest choice on an ambiguous speaker is always to leave them out.
- **Always send something.** An empty window is a finding, not a reason to stay silent. See "When the window is empty" below.
- **Fail cleanly.** On a hard error, retry at most once. If it still fails, deliver the brief with whatever data you successfully collected, clearly prefixed with a warning that data collection was incomplete, and state which step failed and why. Do not hold the brief waiting for recovery beyond that single retry.
- **Out of credits ends the run.** If Evermuse replies `MCP access is paused`, `Insufficient credits`, or HTTP `402`, stop immediately — do not retry, reword, or switch tools, because every Evermuse tool fails identically until credits are added. Send the brief you can assemble with a line saying Evermuse is out of credits and processing is paused.


## How to run it

### 1. Scope

You will normally start from nothing but a one-line instruction to read this skill and run it — no product, no window, no destination. Resolve all three yourself; none of them is a question to ask about.

Take `product_id` from the task prompt when it names one, or use `product_selection.selected_product_id` from the `find_skills` first-run context when present. Otherwise choose from `product_selection.products` (call `get_products` if you have no context); if several exist, pick the one with activity in the window and name your pick in the brief rather than treating list order as a default. The window is the last 24 hours — compute `date_from` / `date_to` as epoch-ms at run time, never from a date baked into a prompt.

### 2. Pull the window

Fire these together, not one at a time:

- `find_sources(product_id, date_from, date_to, limit: 50)` — what came in, and the attendee lists that feed the roster in 1a.
- `search(literal_user_question, product_id, date_from, date_to, note_types: ["need","problem","feedback","quote","qa"])` — filters-only mode, recency-ordered, everything Evermuse extracted in the window. Page with the digest's `next_offset` if the window is busy.
- Two or three semantic `search` calls with `nature: "evidence"` angled at what the window seems to be about, to pull the pull-quotes and surface themes that the flat listing buries.

**Exhaust the window — `find_sources` does not paginate.** It returns at most 50 sources (default 20, so always pass `limit: 50`) and offers **no `offset`**, so a busy day silently hands you a partial roster. Its `total` is the count of matches *before* the page cut and `has_more` says the cut happened. Both matter here:

- **While `has_more` is true, split the window and re-call per slice** — halve the 24 hours, then halve again — until every slice comes back with `has_more: false`. Union the results. This is the only pagination the tool has.
- **Take the header's source count from the summed `total`, never from the length of the array you got back.** They differ on exactly the days when the difference matters.
- A partial roster is not a neutral failure: 1b excludes speakers it cannot place, so every source you never fetched turns real customer quotes into dropped ones. If you genuinely cannot exhaust the window, say so in one line of the brief rather than shipping a quiet undercount.

Read each digest before the items — it hands you the cluster headlines, the distinct meeting count, and the top speakers already tallied. Treat the top-speakers list as a **check on your roster**: a name near the top that you have classified as external is worth resolving against `attendees[]` before you quote it, because a chatty internal facilitator looks exactly like a prolific customer. It is also your tripwire for a truncated roster — a top speaker who appears on no source you fetched means the window is not exhausted.

### 3. Write it

Keep it short enough to read on a phone before standup.

- **Header** — the date, the window, and the real volume: sources processed, signals extracted, how many were customer-facing. Never invent a number; use what the digests actually reported.
- **2–4 themes** — what customers said, strongest first, each with one verbatim external quote and a citation. Prefer a theme that is new or shifting over one that repeats yesterday's.
- **Worth a look** — a handful of single signals that don't form a theme but deserve eyes: a churn risk, a competitor mention, a blunt piece of feedback.
- **One line of context** if the internal side genuinely changes how to read the day ("two of these came from the same escalation call").
- Cite with the inline badge from `references/citations.md`, using each result's real `url`. Never fabricate a quote, a speaker, or a link. Verbatim means verbatim.

### 4. Deliver it

The destination is usually not handed to you either, so resolve it — don't stall on it. Stop at the first of these that gives you a real answer:

1. **A destination named in the task prompt or the conversation** — a channel, a DM recipient, an email address. Use it exactly as written.
2. **Wherever previous briefs went.** Search the connected messaging and mail tools for the last brief you sent (a Slack channel carrying earlier daily briefs, an email thread with the same subject). Continuing an existing thread is almost always what the user set up.
3. **The run's own output.** If neither resolves, write the brief as the run's final output so it still reaches whoever reads the task's results, and say in one line that you couldn't determine a destination and where it went instead. A brief delivered to the wrong place is recoverable; a brief silently not written is not.

Format for where it lands: Slack mrkdwn and short blocks for Slack, a subject line and plain paragraphs for email. Sending is the point of the run — a brief assembled and never delivered anywhere is a failed run.


## When the window is empty

Send the brief anyway. Say plainly that the last 24 hours brought nothing new, name the sources you checked, and stop there — don't pad it with yesterday's themes or widen the window to manufacture content.

Distinguish the two empty cases, because they need different fixes:

- **Nothing came in** — no new sources at all. Worth mentioning if it repeats several days running, since it usually means an ingest task has stopped working.
- **Things came in but nothing was customer-facing** — say that. A day of internal calls is a real answer, and it is exactly the answer Rule 1 exists to produce.
