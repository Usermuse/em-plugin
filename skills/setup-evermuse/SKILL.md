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

## Steps in Sequence
Please follow these steps in the prototype:

0. Welcome the User
1. Source Selection 
2. Schedule Ingest Tasks
3. Schedule Daily Report
4. Backfill Selection
5. Ingest Sources
6. Post Ingestion View

## 0. Welcome the User

Render the Evermuse setup header as a visual widget (mcp__visualize__show_widget), HTML mode:

- Wrapper: <div style="display:block; width:100%; box-sizing:border-box; text-align:center; padding:1rem 0;">
- Logomark: the SVG below at width="80" height="80", viewBox="0 0 160 160", style="display:block; margin:0 auto". Use the paths verbatim — do not restyle, recolor, or theme them.
- Heading: <h1 style="font-size:22px; font-weight:500; margin:16px 0 0; text-align:center;">Welcome to Evermuse Setup</h1>
- Subtitle: <p style="font-size:16px; font-weight:400; line-height:1.7; color:var(--text-secondary); margin:8px auto 0; max-width:520px; text-align:center;">We'll be selecting sources to process, storage options, and historical import preferences to get you set up.</p>
- Start with <h2 class="sr-only"> describing the header for screen readers.
- No card border, no background, nothing else in the widget.

<svg width="80" height="80" viewBox="0 0 160 160" fill="none" role="img" style="display:block; margin:0 auto"><title>Evermuse logomark</title><desc>Two overlapping petal shapes, left yellow and right pink.</desc><path d="M61.9648 68.3047C50.1805 66.4199 42.6607 66.2318 39.4053 67.7405L36.1416 67.8549C34.0916 67.8549 32.2375 68.4666 30.5791 69.69C28.9206 70.913 27.7747 72.515 27.1412 74.494C26.5078 76.474 26.1911 78.349 26.1911 80.121C26.1911 81.892 26.1422 84.301 26.0443 87.348C25.9465 90.394 26.0105 94.244 26.2363 98.896C26.4621 103.549 27.7395 108.947 30.0685 115.09C32.3975 121.232 36.7321 125.223 43.0725 127.063C49.4128 128.902 56.0515 129.426 62.9886 128.636C69.9256 127.845 75.681 125.185 80.255 120.654C84.829 116.124 87.132 109.895 87.166 101.968C87.2 94.04 85.954 86.919 83.429 80.604C80.904 74.289 73.749 70.19 61.9648 68.3047Z" fill="#FFC921" stroke="#FFC921" stroke-width="4"/><path d="M100.161 69.7441C88.66 67.9039 81.322 67.7202 78.145 69.1932L74.96 69.3049C72.959 69.3049 71.15 69.9022 69.5312 71.097C67.9128 72.291 66.7944 73.855 66.1762 75.787C65.5581 77.72 65.249 79.551 65.249 81.281C65.249 83.01 65.2012 85.362 65.1058 88.336C65.0103 91.311 65.0728 95.069 65.2931 99.612C65.5135 104.154 66.7601 109.424 69.033 115.422C71.306 121.419 75.536 125.316 81.724 127.111C87.911 128.907 94.39 129.419 101.16 128.647C107.93 127.875 113.546 125.278 118.01 120.854C122.474 116.431 124.722 110.35 124.755 102.61C124.787 94.871 123.572 87.918 121.107 81.753C118.643 75.587 111.661 71.584 100.161 69.7441Z" fill="#FF4A8E" stroke="#FF4A8E" stroke-width="4"/></svg>

CHAT BODY: Ready to get started?

If they answer positively, then move to the next step. If they answer negatively, ask if they would like to schedule a 1-on-1 session with a Product expert to walk them through the setup. If they reply positively to that, send them this link: https://calendly.com/d/cwbw-zmn-r5k/30-minute-evermuse-live-demo



## 1. Source Selection

First - scan the currently connected MCPs and identify those that might be relevant sources of customer communication. These include call recorders, chat platforms, support ticketing systems, CRMs, email providers, and other sources that may contain customer feedback.
Then output the following title and subtitle in chat:
H1 TITLE: **Hi there, where does your customer feedback come from today?**
SUBTITLE: Please select all that applies.

Then please use the question asking tool (or the show_widget tool if there are more than 4 options) to choose any of the tools you found in a multi-select list. If none are found, skip this step and go straight to asking of other, non-connected tools.

Once they made a selection, acknowledge the selection, and ask if there are any others customer sources they'd like to process. Respond positively to any answer. If there are available connectors for those sources, please help them to connect those sources. (If you have a nice connector card you can pull up please do so.) If there are no ready made connectors on your side, do a quick web search to see if there are any official MCP pages from these vendors and direct them to those. Then ask them if they'd like to hold or skip and proceed with the setup.

(If there are no sources connected, please let them know that much of the value of Evermuse is in processing customer feedback, and that they can always come back to this setup later once they have connected sources. Give them the option to connect sources now, or to cancel the setup and come back later. If they choose to connect sources now, please help them to do so. If they choose to cancel the setup, remind them they can always call the setup-evermuse skill via the Evermuse MCP. Then finish running this skill and exit gracefully.)


## 2. Schedule Ingest Tasks

Output the following title and subtitle in chat:
H1 TITLE: **Great! Let's schedule daily tasks to process your selectedsources.**
SUBTITLE: Relevant records from these sources will be processed for insights and added to my memory on Evermuse.

Then, for each source selected and connected, please schedule a daily evening task to fetch the last 24 hours of records, and add any relevant ones containing customer feedback to Evermuse via the Evermuse MCP's add_source tool. 


## 3. Schedule Daily Report

Ask the user how they'd like to get the daily report: In a Slack channel, a private Slack message, or an email. (Make sure you have access to these tools). You can also allow Other, letting the user choose any channel that is available to you via connectors.

Then ask the user what time of day they'd like to receive the daily report. (Suggest a morning report at 7am) Please schedule the daily report task accordingly, and confirm with the user that it is scheduled. The daily report task should include a search of signals and sources via the Evermuse MCP and a summary of everything that was learned by Evermuse in the past 24 hours.


## 4. Backfill Selection

First output the following title and subtitle in chat:
H1 TITLE: How far back should I scan sources for insights?
SUBTITLE: Please select your backfill preference.

Make sure the title and subtitle landed, then draw an in-chat backfill options plan via a side by side plan comparison.
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
H1 TITLE: Great! Let me process the last [7/30]] days and send what I find to be processed. When I'm done, I can write my initial report.
SUBTITLE: You can leave this thread and come back later.

Then, proceed to launch as many subagents as are required to complete the task of procescsing all selected sources from Step 1, and sending all relevant conversations from the last 7 or 30 days from to Evermuse using the add_source tool.

### Some rules to follow
 1. Each source on Evermuse is one conversation or record: Example one call on Gong, one customer update on a CRM, one chat on Intercom, one day of back and forth messages with a customer in Slack, one ticket on Zendesk. 
 2. Only direct quotes from customers should be sent to Evermuse. Do not summarize 2nd hand reports, automated summaries, or any other non-verbatim content. Only send the actual words of the customer such as a call, a ticket, an email - along with surrounding context as is needed to understand it (for example a full transcript of a call, or the full back and forth of a chat).
 3. There could be hundreds of calls in a 30 day period, so use subagents intelligently to avoid running out of context window space. Do not send the smallest agents to do task that require genuine good judgement but only for technical routine tasks.
 4. Each agent should be asked to read the using-evermuse skill and its references before starting the task, and to follow the grounding mechanics in that skill to find relevant evidence. Each agent should be asked to return a report of what it found, with citations, and to send full transcripts of relevant communications or conversations to Evermuse using the add_source tool.

If you are able at this step - output a custom live-updating progress bar showing a real percentage of ingestion done. Below it please output a message in chat that says "Processing your sources now. This may take a while. I recommend you leave this thread and come back later today."


## 6. Post-Ingestion View

Please output this message in chat with real numbers and sources:

H1 TITLE: **All done! I've processed your sources for the past [7/30] days.**
SUBTITLE: I've scanned **X calls on Gong**, **Y Zendesk tickets**, and **Z Slack Connect channels**. I've learned so much, and I'll remember it with Evermuse!

Here are some things you can do from here:

--

Make sure the title and subtitle landed, then use the show_widget tool to show a stylish single-select one of the following options:
* Get my initial report
* Dive into a topic
* Write a spec
* Write an interview script
* Find the gaps
* See all new capabilities






