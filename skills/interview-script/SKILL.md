---
name: interview-script
description: >-
  Write a Mom-Test customer interview guide — warm-up, JTBD exploration, wrap-up
  — but first check the evidence for what's ALREADY known so the questions
  target the open gaps instead of re-asking answered questions. Use when the
  user asks to 'write an interview script', 'prep for a user interview',
  'discovery interview questions', 'interview guide', 'what should I ask
  customers', or 'plan a research call'. Trigger terms: interview script,
  interview guide, user interview, discovery interview, customer interview
  questions, Mom Test, research questions to ask. Not for summarizing an
  interview already done (that's summarize-conversation).
category: Discovery & Research
tags:
  - interviews
  - research
  - scripts
---

# Customer Interview Script (targeted at the gaps)

A Mom-Test interview guide — ask about their life, not your idea; the past, not the future. The Evermuse difference: **before writing a single question, check what the corpus already answers.** No point spending a scarce interview slot re-asking what six customers already told you. The script targets the *open* gaps, so each call buys new knowledge instead of confirming old.

## Step 0 — Relevance & availability
Confirm this is interview prep and Evermuse is connected (see `using-evermuse` Step 0). If disconnected, you can still produce a solid Mom-Test script from the framework — label it **⚠ ungrounded** and note you couldn't check what's already known, so it may re-ask settled questions.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify product. Run **2–4 `evidence` searches** on the research topic $ARGUMENTS to see what customers have *already* said; `find_supporting_quotes(topic, limit: 6–8)` for what's well-established. Then pull **`get_research_questions`** as a **labeled secondary** input — Evermuse's AI-suggested open questions — to cross-reference, never as the primary source of truth.
- **Work:** split known vs. open, then build the script around the open gaps.
- **Cite:** the "already known" list carries quotes + inline citations (linked-number code badges per `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/references/citations.md`) so the user sees *why* those questions are cut.
- **Save:** `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","interview-script","<topic>"])` after confirmation.

## Instructions

1. **Clarify research objectives.** What decision will this research inform? What assumptions need validating?

2. **Split known vs. open (the key step).** From the grounded evidence, list what the corpus **already answers** (with quotes) and what remains **genuinely open**. Write questions only for the open set; briefly show the user what you cut and why. For each open gap, note whether `get_research_questions` (secondary) independently flagged it.

3. **Build the script** — Mom-Test throughout:

   **Opening (2–3 min):** purpose is learning not selling; no right/wrong answers; permission to record; confirm time.

   **Warm-up (5 min):** role, typical day/week, how long they've done the relevant activity. Build rapport, understand context.

   **Core exploration — JTBD (15–20 min), weighted to the open gaps:**
   - *Current behavior (past tense, specific):* "Walk me through the last time you [did the thing]. What happened? What tools? How long? Who else?"
   - *Pain points (observe, don't lead):* "What was the hardest part? What have you tried? What happened?"
   - *Desired outcomes (their words):* "What does 'good' look like? How would you know it's working?"
   - *Willingness to pay (skin in the game):* "How much time/money does this cost you today? Have you looked for a better solution?"

   **Probing:** "Tell me more about that" · gentle "Why?" ×2–3 · "Can you give a specific example?" · "What happened next?" · "How did that make you feel?"

   **Mom-Test rules:** ask about their life not your idea · the past not the future · talk less (80/20) · never pitch · hunt for strong emotion · compliments are noise.

   **Wrap-up (3–5 min):** "Anything I didn't ask that matters?" · "Who else should I talk to?" · thanks · next steps.

4. **Note-taking template** (include it):
   ```
   Participant: [Name / ID]   Date: [Date]
   Key Jobs: […]   Current Solution: […]
   Biggest Pain: […]   Desired Outcome: […]
   Willingness to Pay: […]   Surprise Finding: […]   Follow-up: […]
   ```

## Deliverable
The script (with an up-front **"Already known — not asking"** box citing the corpus, and a **"Targeting these gaps"** list) plus the note-taking template. After the interview, hand off to `/evermuse:summarize` to feed the summary back into the corpus.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT) — interview-script. Method: *The Mom Test* (Rob Fitzpatrick); Product Trio: Teresa Torres, *Continuous Discovery Habits*.
