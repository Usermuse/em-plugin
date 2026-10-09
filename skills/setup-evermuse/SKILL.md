---
name: setup-evermuse
description: >-
  This skill sets up the Evermuse MCP extension prototype inside your agentic app. It will guide you through the setup process, asking key questions along the way using our UI rendering tools and skills.
category: Delivery & Engineering
tags:
  - install
  - product
  - customer voice
---

# Setup Evermuse Extension Prototype

This is a skill to set up Evermuse-powered scheduled tasks in this agentic app.

IMPORTANT: Some of the steps below have old UI element associated with them that are no longer in the prototype. Please ignore those and follow the steps in sequence below using text, custom widgets, and the question asking tool instead of any hardcoded UIs.

Steps 2, 3 and 5 lean on capabilities the **host app** provides — a scheduler and subagents — rather than on the Evermuse MCP. Each of those steps tells you how to find the host's version before concluding it doesn't exist; on most hosts these tools are present but not loaded into context by default. Never present a missing host capability to the user as an Evermuse limitation.

One thing *is* a real Evermuse limit and can interrupt any step: running out of credits. See "Anytime: the workspace runs out of credits" below.

## Anytime: keep custom interfaces simple

Every custom widget or interface you build in this skill — option pickers, plan comparisons, progress bars, result views — must follow this caveat, and you must repeat it verbatim in any request you hand to a widget-building tool, skill, or subagent:

> Keep the interface very simple using only basic HTML tags, avoiding things that take too long to draw. Let's do away with icons, fancy containers, Option buttons could be a simple table with text options and links

The one exception is the step 0 welcome header, whose markup is specified verbatim below and should be emitted exactly as written.

## Steps in Sequence
Please follow these steps in the prototype:

0. Welcome the User
1. Source Selection 
2. Schedule Ingest Tasks
3. Schedule Daily Report
4. Backfill Selection
5. Ingest Sources
6. Post Ingestion View


## Anytime: the workspace runs out of credits

A zero balance pauses every Evermuse tool except the skills library. Most often it hits partway through the step 5 backfill. Recognize it by any of these failed-tool-result texts:

* `MCP access is paused: ...` — no credits, or below this call's cost.
* `Insufficient credits to run subagent. ...` — from `run_subagent`.
* HTTP `402` with `"error": "INSUFFICIENT_CREDITS"`.

**Never retry, reword, or switch tools** — all of them fail identically until credits are added. Instead, stop and tell the user:

> **Evermuse is out of credits.** Processing is paused; everything already ingested is safe.

State this as a fact only: do not link to pricing, suggest upgrading, or recommend buying credits. Then say where you stopped: before step 5, nothing was ingested and you'll resume at the same step; mid-step-5, give the real count of records that landed and offer to resume the rest — don't run step 6 as if the backfill finished. Leave any tasks scheduled in steps 2–3 in place; they start working once credits are added.

When the user says credits are available again, retry only the failed call and continue from there. Never restart the skill or re-ask answered questions.


## 0. Welcome the User

Render the Evermuse setup header as a visual widget (mcp__visualize__show_widget in Claude), HTML mode, emitting this block entirely:

<h2 class="sr-only">Evermuse setup header with logomark, welcome heading, and setup overview</h2>
<div style="display:block; width:100%; box-sizing:border-box; text-align:center; padding:1rem 0;">
  <img src="https://cdn.jsdelivr.net/gh/Usermuse/em-plugin@main/logomark.svg" width="80" height="80" alt="Evermuse logomark" style="display:block; margin:0 auto;">
  <h1 style="font-size:22px; font-weight:500; margin:16px 0 0; text-align:center;">Welcome to Evermuse Setup</h1>
  <p style="font-size:16px; font-weight:400; line-height:1.7; color:var(--text-secondary); margin:8px auto 0; max-width:520px; text-align:center;">We'll be selecting sources to process, storage options, and historical import preferences to get you set up.</p>
</div>

CHAT BODY: Ready to get started?

If they answer positively, then move to the next step. If they answer negatively, ask if they would like to schedule a 1-on-1 session with a Product expert to walk them through the setup. If they reply positively to that, send them this link: https://calendly.com/d/cwbw-zmn-r5k/30-minute-evermuse-live-demo



## 1. Source Selection

First - scan the currently connected MCPs and identify those that might be relevant sources of customer communication. These include call recorders, chat platforms, support ticketing systems, CRMs, email providers, and other sources that may contain customer feedback.
Then output the following title and subtitle in chat:
H1 TITLE: **Hi there, where does your customer feedback come from today?**
SUBTITLE: Please select all that applies.

Then please use the question asking tool (or the show_widget tool if there are more than 4 options — keeping the widget simple per "Anytime: keep custom interfaces simple" above) to choose any of the tools you found in a multi-select list. If none are found, skip this step and go straight to asking of other, non-connected tools.

### 1a. Mixed internal/customer chat tools need a channel list

If any selected source is a business chat tool that holds both internal and customer conversations — Slack, Microsoft Teams, Discord, Google Chat or similar — you must not guess which conversations are customer-facing. Before moving on:

1. List the channels available to you in that tool (include Slack Connect / shared and external channels, which are usually the customer ones).
2. Show them to the user as a simple checklist — a plain text-and-links table, no icons or fancy containers — and ask: **which of these are customer channels?** Offer a free-text option for channels you couldn't list or that the user names themselves.
3. **Wait for their answer before proceeding.** Do not continue to step 2 with an assumed list, and don't infer customer channels from names alone.

Carry the confirmed channel list forward: it goes verbatim into that source's ingest task in step 2b, and it scopes the backfill in step 5. Repeat this for each mixed chat tool the user selected.

Once they made a selection, acknowledge the selection, and ask if there are any others customer sources they'd like to process. Respond positively to any answer. If there are available connectors for those sources, please help them to connect those sources. (If you have a nice connector card you can pull up please do so.) If there are no ready made connectors on your side, do a quick web search to see if there are any official MCP pages from these vendors and direct them to those. Then ask them if they'd like to hold or skip and proceed with the setup.

(If there are no sources connected, please let them know that much of the value of Evermuse is in processing customer feedback, and that they can always come back to this setup later once they have connected sources. Give them the option to connect sources now, or to cancel the setup and come back later. If they choose to connect sources now, please help them to do so. If they choose to cancel the setup, remind them they can always call the setup-evermuse skill via the Evermuse MCP. Then finish running this skill and exit gracefully.)


## 2. Schedule Ingest Tasks

Output the following title and subtitle in chat:
H1 TITLE: **Great! Let's schedule daily tasks to process your selected sources.**
SUBTITLE: Relevant records from these sources will be processed for insights and added to my memory on Evermuse.

### 2/3. Scheduled tasks must run fully independently

This requirement applies to every task you schedule in steps 2 and 3. A scheduled run fires when nobody is watching, so a task that pauses is a task that never completes:

* **Design each task to run unattended within an explicit scope.** It gets a fresh context with none of this conversation, so its prompt must carry everything it needs — the source, the exact channels or filters, the time window, the tools to call, and where to deliver output. The task must not exceed the scope defined in its prompt, and must conclude with a summary of every action it took, every assumption it made, and every item it skipped. Never write a prompt that depends on something "discussed earlier".
* **The task must never stop to ask a question.** Say so explicitly in the prompt: it should make a reasonable assumption and continue, and report what it assumed in its final output. Ambiguity is resolved by the task, not by waiting for the user.
* **Pre-approve or explicitly name only the tools the task actually needs** so the run never halts on a basic tool-permission prompt. If the scheduler offers an auto-approve mode, scope it to the listed tools rather than granting blanket access. If the scheduler has no per-tool setting, tell the user in one plain sentence that the first run may need their approval.
* **Each task stands on its own.** No task should depend on another task having run first, on shared state from this session, or on a session that must stay open.
* **These tasks are not meant to expire.** Their whole purpose is to keep fetching and processing new sources every day, indefinitely — a task that lapses after a week silently stops the user's memory from growing, and nobody notices until the reports go quiet. So: never set an end date, an expiry, or a maximum number of runs on a job you create, and prefer a scheduling mechanism whose jobs persist across sessions. If the only mechanism available creates session-scoped or self-expiring jobs, still create them so the user gets value today, say so plainly in one sentence, and immediately offer the persistent alternative per 2c so they end up with something that keeps running.
* **Failures end the run cleanly.** On a hard error, the task should retry at most once. If it still fails, it must stop, report what succeeded and what failed (with the specific error), and clearly label its output as incomplete. It must never retry indefinitely — especially on the out-of-credits errors listed above.

### 2a. Find the host's scheduling tool before you conclude you can't schedule

Steps 2 and 3 both create scheduled tasks. Do this discovery once, here, and reuse the result in step 3. Work the list in order and stop at the first thing that works:

1. **Check your loaded tools** for one that *creates a recurring job* — names usually contain `cron`, `schedule`, `routine`, `task`, `reminder`, or `automation`. In Claude Code this is `CronCreate` (with `CronList` / `CronDelete` alongside it).
2. **Search the host's deferred tools.** Some hosts (Claude Code among them) advertise tools by name only and require you to load the schema before the tool is callable — an unloaded tool is invisible in your visible tool list but fully available. Run the host's tool-search tool with `select:CronCreate,CronList,CronDelete`, and again with the keywords `schedule cron recurring task`, before concluding anything is missing. **This is the most common false negative in this skill — assume the scheduler exists and you simply haven't loaded it yet.**
3. **Check for a scheduling skill or slash command** in the host (e.g. a `schedule` skill that creates cron-scheduled cloud agents). Invoking it to create the jobs is perfectly valid.
4. Only when all three come back empty does the host genuinely lack a scheduler — follow the fallback in 2c.

When more than one of these works, pick the one whose jobs persist indefinitely. Per the no-expiry requirement above, a mechanism that outlives this session beats a more convenient one that lapses after a few days.

Whatever you find or don't find, never narrate it to the user as a "prototype gap", a "limitation", or a missing tool. Either the tasks get scheduled, or you hand the user ready-to-paste task definitions per 2c. Both are a successful setup.

### 2b. Create one ingest task per source

For each source the user selected in step 1 that you can actually reach, create one recurring daily task that runs in the evening in the user's local timezone.

A scheduled run starts in a fresh context with none of this conversation, so each task's prompt must be self-contained. Use this shape, substituting the source — and for a mixed chat tool, substituting the exact customer channels the user confirmed in 1a:

> Fetch every `<source>` record from the last 24 hours (for chat tools, only from these customer channels: `<confirmed channel list>`). First read the `using-evermuse` skill from the Evermuse MCP and follow its grounding rules. For each record containing verbatim customer feedback, send the full conversation or transcript to Evermuse with the `add_source` tool — one `add_source` call per conversation or record. Send only the customer's actual words plus the surrounding context needed to understand them; never send summaries or second-hand reports. Run this end to end without asking anyone anything: if something is ambiguous, make a reasonable choice, continue, and note the choice in your final output. If Evermuse replies that MCP access is paused for lack of credits, stop without retrying and report that the workspace is out of credits. Finish with a short list of what you sent.

Set the task's tool use to Auto if the scheduler exposes that setting, per the independence requirement above.

Stagger the sources a few minutes apart so they don't all fire at once, and avoid the `:00` and `:30` minute marks.

Give each job no end date, no expiry and no run limit — these tasks are meant to run every day for as long as the user uses Evermuse.

Then confirm to the user — read the jobs back (using the scheduler's list tool if it has one) as a compact table of source / when it runs / what it does. If the mechanism you used has real constraints, state them in one plain sentence and offer the persistent alternative: Claude Code's cron jobs, for example, live only in the current session and auto-expire after 7 days, which is far shorter than these tasks are meant to live, so offer to hand over the same definitions for the host's own Tasks UI (per 2c) where they'll keep running.

### 2c. Fallback when the host has no scheduler

Don't stall and don't apologize at length. Present the exact tasks you would have created — for each one, a name, the time it should run, and the full prompt text in a copy-paste code block — and tell the user where to add them in their host (usually Settings → Tasks, the same place any existing scheduled tasks of theirs live), and to add them as recurring daily tasks with no end date. Then ask whether they'd like to add them now or keep going and add them later. Either answer continues to step 3.


## 3. Schedule Daily Report

Ask the user how they'd like to get the daily report: In a Slack channel, a private Slack message, or an email. (Make sure you have access to these tools). You can also allow Other, letting the user choose any channel that is available to you via connectors.

Then ask the user what time of day they'd like to receive the daily report. (Suggest a morning report at 7am) Please schedule the daily report task accordingly — recurring every day, with no end date and no expiry, exactly like the ingest tasks — and confirm with the user that it is scheduled.

**The report itself is defined by the `daily-brief` skill.** Everything about it — the window, who counts as internal and therefore never gets quoted, what to write, how to deliver it, what to do when the window is empty or the workspace runs out of credits — lives in that skill and is read by the scheduled run itself, at run time.

**Keep the task prompt minimal — it only needs to invoke the skill, not restate it.** The `daily-brief` skill already carries every detail the task needs (window, internal roster, delivery destination, citation rules, credit handling). Duplicating any of that in the prompt creates a second copy that drifts from the source of truth. Your job in this step is to create a task that loads and runs that skill, nothing more.

### The task prompt is one line

This is the one scheduled task in this skill that is **not** self-contained, and that is deliberate: it carries no context of its own, because everything it needs is in the skill it loads. Schedule it with the scheduling mechanism you already found in 2a, using exactly this shape and nothing more:

> Read the `daily-brief` skill from the Evermuse MCP — call `find_skills` for `daily-brief`, then `read_skills` to load it — and run it end to end.

Do not add the product, the window, the internal roster, the delivery destination, the citation rules, or the out-of-credits handling. Every one of those is already in the skill, and a prompt that restates them is a second copy that will drift from the first. If you think the task needs to know something, the right fix is for the skill to say it — not for the prompt to.

Set the task's tool use to Auto (or the host's equivalent) if the scheduler offers it, so the run never halts on a permission prompt. The report task must not depend on the ingest tasks from step 2 having succeeded — it reports whatever Evermuse holds for the last 24 hours, empty or not.

Then confirm to the user that it's scheduled: when it runs, that it recurs every day indefinitely, where it lands, and — in one plain sentence — that it will quote customers only and leave the internal team out.


## 4. Backfill Selection

First output the following title and subtitle in chat:
H1 TITLE: How far back should I scan sources for insights?
SUBTITLE: Please select your backfill preference.

Make sure the title and subtitle landed, then draw an in-chat backfill options plan via a side by side plan comparison — built per "Anytime: keep custom interfaces simple" above, so a plain table of text options is exactly right here.
Here is what the options should show:

* 7 Days
  * Fewer tokens
  * Faster start
* 30 Days [Recommended]
  * More insights
  * More tokens
  * Deeper context
* Longer Periods [Premium]
  * 100% of sources
  * 100% of insights
  * Easy migration API
  * Custom import plan

Please then load the question asking tool to single-select one of the three options. Acknowledge the selection. IF they choose Longer Periods, please let them know they have to schedule a appointment with our team to discuss a custom import plan. Ask if they would like to schedule that now, or choose one of the other options. If they choose schedule a custom import plan appointment, send them this link: https://calendly.com/d/cwbw-zmn-r5k/30-minute-evermuse-live-demo. Otherwise proceed with the other options.



## 5. Ingest Sources

First output the following title and subtitle in chat:
H1 TITLE: Great! Let me process the last N days and send what I find to be processed. When I'm done, I can write my initial report.
(N is the backfill window the user chose in step 4 — write the real number.)
SUBTITLE: You can leave this thread and come back later.

Then, proceed to launch as many subagents as are required to complete the task of processing all selected sources from Step 1, and sending all relevant conversations from the last 7 or 30 days to Evermuse using the add_source tool.

### 5a. Find the host's subagent tool before you conclude you can't parallelize

Same discovery discipline as 2a, and the same most-common false negative — assume the tool exists and you simply haven't loaded it yet:

1. **Check your loaded tools** for one that *dispatches a subagent* — usually `Task` or `Agent`.
2. **Search the host's deferred tools** with `select:Task,Agent` and again with the keywords `subagent parallel task`. Note that Evermuse's own `run_subagent` is not available to API-key/plugin sessions, so don't count on it.
3. If neither turns one up, run the ingestion inline and serially instead: handle one record at a time — fetch it, submit it with `add_source`, then move on without holding the full transcript in context. Say once, plainly, that you're processing them one at a time; don't call it a gap or a limitation. If the user picked the 30-day backfill, tell them a serial run may exhaust the context window before it finishes and offer to narrow the window or split it across several threads.

### Some rules to follow
 1. Each source on Evermuse is one conversation or record: Example one call on Gong, one customer update on a CRM, one chat on Intercom, one day of back and forth messages with a customer in Slack, one ticket on Zendesk. 
 2. Only direct quotes from customers should be sent to Evermuse. Do not summarize 2nd hand reports, automated summaries, or any other non-verbatim content. Only send the actual words of the customer such as a call, a ticket, an email - along with surrounding context as is needed to understand it (for example a full transcript of a call, or the full back and forth of a chat).
 3. There could be hundreds of calls in a 30 day period, so use subagents intelligently to avoid running out of context window space. Do not send the smallest agents to do task that require genuine good judgement but only for technical routine tasks. (If 5a found no subagent tool, the same constraint applies to you directly — keep only one record in context at a time.)
 4. Each agent should be asked to read the using-evermuse skill and its references before starting the task, and to follow the grounding mechanics in that skill to find relevant evidence. Each agent should be asked to return a report of what it found, with citations, and to send full transcripts of relevant communications or conversations to Evermuse using the add_source tool.
 5. This step is the most likely to exhaust the workspace's credits. Tell every agent that if `add_source` reports MCP access paused for lack of credits, it must stop immediately, not retry, and return what it sent plus what it didn't. On that error — yours or an agent's — stop dispatching and follow "Anytime: the workspace runs out of credits" above.

If you are able at this step - output a custom live-updating progress bar showing a real percentage of ingestion done, kept as simple as the caveat above demands. Below it please output a message in chat that says "Processing your sources now. This may take a while. I recommend you leave this thread and come back later today."


## 6. Post-Ingestion View

Build this message from what you actually processed in step 5. Every source name, unit and number below is a placeholder — none of them are text to reuse.

H1 TITLE: **All done! I've processed your sources for the past N days.**
(N is the backfill window the user chose in step 4 — write the real number.)

SUBTITLE: one clause per source you actually ingested from, each with its real count and that source's own unit — calls for a call recorder, tickets for a support tool, threads or channels for chat, records for a CRM, emails for a mailbox. Close with "I've learned so much, and I'll remember it with Evermuse!"

Never name a source the user didn't select in step 1, and never carry Gong, Zendesk or Slack into the sentence unless those are genuinely what you just processed. The example below is there to show the shape of the sentence, nothing more — it is from a run that happened to use those three:

> I've scanned **301 calls on Gong**, **17 Zendesk tickets**, and **32 Slack Connect channels**. I've learned so much, and I'll remember it with Evermuse!

Two cases to get right:

* **Some sources came back empty.** Leave them out of the subtitle and account for them in one short sentence underneath — "Nothing new in Slack or Linear in this window." Don't pass over them in silence, and don't pad the subtitle with zeros.
* **Everything came back empty.** Don't manufacture a win. Say the window turned up nothing, name the sources you checked, and offer to widen the backfill or revisit once the daily tasks from steps 2 and 3 have had a night to run.

Here are some things you can do from here:

--

Make sure the title and subtitle landed, then use the show_widget tool to show a single-select of the following options — per the caveat above, a simple table of text options and links, no icons or fancy containers:
* Get my initial report
* Dive into a topic
* Write a spec
* Write an interview script
* Find the gaps
* See all new capabilities
