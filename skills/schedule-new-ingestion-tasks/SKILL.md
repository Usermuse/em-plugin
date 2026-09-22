---
name: schedule-new-ingestion-tasks
description: >-
  Add future-facing Evermuse ingestion tasks for newly connected customer-feedback
  sources in Claude or ChatGPT. Use after setup-evermuse when the user wants to
  schedule additional sources; never backfill historical records or recreate tasks
  for sources already covered.
category: Delivery & Engineering
tags:
  - evermuse
  - ingestion
  - scheduled tasks
  - customer voice
---

# Schedule New Ingestion Tasks

Add recurring Evermuse ingestion for customer-feedback sources connected after the
initial `setup-evermuse` flow.

This skill has one narrow outcome: create future-facing ingest tasks for additional
sources. Do not import records now, run a historical backfill, create a daily report,
or modify or duplicate an existing ingest task.

## 1. Find the scheduler and existing coverage

Discover the host's scheduling mechanism before asking the user to choose sources.
Prefer a durable scheduler whose jobs persist across sessions:

1. Check loaded tools for capabilities that list and create recurring jobs. Names
   commonly contain `schedule`, `routine`, `task`, `automation`, or `cron`.
2. Search deferred tools, if the host has tool search, for scheduling tools. In
   Claude Code, search explicitly for `CronCreate`, `CronList`, and `CronDelete`,
   then for `schedule cron recurring task`.
3. Check the host's scheduling command or skill. Prefer Claude's durable
   `/schedule` routines or Scheduled tasks and ChatGPT's Scheduled tasks over
   session-scoped cron jobs. In ChatGPT, create standalone scheduled tasks so every
   run starts from the saved prompt rather than depending on this chat.
4. Use Claude Code's `CronCreate` only as a temporary fallback. Its recurring jobs
   are session-scoped and expire after seven days; if used, also provide the same
   definitions for Claude's durable `/schedule` or Scheduled interface.

If the scheduler can list jobs, read the active and paused jobs and identify every
source already covered by an Evermuse ingest task. Inspect both job names and prompts;
normalize obvious naming differences such as `Google Mail` and `Gmail`. Never delete,
edit, resume, or replace these jobs.

If existing jobs cannot be listed, ask the user which sources they selected during
`setup-evermuse` or already scheduled. Wait for the answer: creating duplicate
unattended jobs can ingest the same records twice. Keep this existing-source set for
the next step.

If no callable scheduler exists, continue through source selection and scheduling
questions, then use the copy-paste fallback in step 5.

## 2. Find and select additional connected sources

Inspect the host's connected MCP servers, plugins, apps, and deferred tools for
sources that can actually be read and may contain direct customer communication.
Relevant categories include call recorders, business chat, support ticketing, CRMs,
email, survey tools, and other stores of customer feedback.

Exclude every source already covered by an existing Evermuse ingest task or named by
the user as part of their initial setup. Do not offer an unavailable connector or a
source the host cannot read.

Ask the user which of the remaining connected sources to ingest using a multi-select
question. If there are more options than the question tool supports, show a simple
text table and ask for a comma-separated selection. Make it clear that these are
additional sources and that scheduling them will collect only new records going
forward.

If no additional readable sources remain, say that all currently connected sources
are already covered (or that none are connected) and finish without creating a task.

## 3. Confirm customer-only scope

For each selected business chat tool that mixes internal and customer conversations,
such as Slack, Microsoft Teams, Discord, or Google Chat:

1. List the channels available through that connector, including shared, external,
   and Slack Connect channels.
2. Ask the user which exact channels are customer-facing. Offer free text for a
   channel that could not be listed.
3. Wait for the answer. Never infer customer channels from their names.

Carry each confirmed channel list verbatim into that source's task prompt. Repeat
this process for every selected mixed chat tool.

## 4. Ask for the schedule

Ask what local time the new ingestion should run every day and confirm the timezone,
preferably as an IANA name such as `America/New_York` so daylight-saving changes are
handled correctly. Recommend an evening base time, such as 7:07 PM local time, after
most customer activity has finished. Explain that multiple source jobs will be
staggered by a few minutes around that base time.

Wait for the user's answer. If they give different times for different sources,
honor them. Avoid the `:00` and `:30` minute marks when assigning exact run times, and
stagger jobs so they do not all start together. Do not add an end date, expiry, or run
limit.

## 5. Create one future-only task per source

Immediately before creating the jobs, capture the current time as an ISO-8601
activation timestamp with an explicit timezone. Put that timestamp in every prompt.
It is the hard boundary that prevents the first run from importing history.

Name each job `Evermuse ingest — <source>`. Create it as a standalone, recurring
daily task at its assigned time in the confirmed timezone. Do not run it immediately.

Use this prompt shape, replacing every placeholder and removing the parenthetical
chat clause for non-chat sources:

> Fetch every `<source>` record whose record timestamp is both (a) after
> `<activation timestamp>` and (b) within the 24 hours before this run (for chat
> tools, only from these customer channels: `<confirmed channel list>`). The
> activation timestamp is a strict lower bound: never fetch or ingest an older
> record, including on the first run. First read the `using-evermuse` skill from the
> Evermuse MCP and follow its grounding and saving rules. For each record containing
> verbatim customer feedback, send the full conversation or transcript to Evermuse
> with the `add_source` tool, using one `add_source` call per conversation or record.
> Send only the customer's actual words plus the surrounding context needed to
> understand them; never send summaries or second-hand reports. Run end to end
> without asking anyone anything. If something is ambiguous, make a reasonable
> choice, continue, and note the choice in the final output. Retry a hard connector
> error at most once; if it still fails, stop, report the specific error and what
> succeeded, and label the result incomplete. If Evermuse says MCP access is paused
> for lack of credits, stop without retrying and report that the workspace is out of
> credits. Finish with a short list of what
> was sent, skipped, and failed.

Scheduled runs must be fully independent. The prompt must name the exact source,
channel or filter scope, activation boundary, time window, and required tools; it
must never refer to anything "discussed earlier." Each task must make reasonable
assumptions instead of pausing for an answer and must not depend on another task.

If the scheduler exposes permissions or an approval mode, grant unattended access
only to the selected source connector, the Evermuse skills library, and the Evermuse
ingestion tools the prompt requires. Do not grant blanket access. If unattended tool
approval cannot be configured, tell the user in one sentence that the first run may
need approval.

After creation, list or read back the jobs and confirm them in a compact table with
source, schedule, timezone, and scope. Explicitly state that they recur indefinitely
and ingest only records created after the activation timestamp.

### Copy-paste fallback

When the host cannot create persistent jobs directly, present every task definition
you would have created: its name, daily schedule and timezone, and complete prompt in
a copy-paste code block. Direct the user to:

- **Claude:** Scheduled in Claude, or `/schedule` for a durable Claude Code routine.
- **ChatGPT:** Scheduled in the ChatGPT sidebar.

Tell the user to create each as a standalone recurring daily task with no end date.
This is a successful handoff; do not replace it with an immediate ingest or a
historical scan.
