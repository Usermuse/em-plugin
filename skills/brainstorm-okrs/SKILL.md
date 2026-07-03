---
name: brainstorm-okrs
description: "Draft team-level OKRs — inspirational objectives with measurable key results — where each objective maps to an evidenced customer outcome and each key result measures reduction of a real, stated customer pain, aligned to company strategy via Evermuse. Use when the user wants to 'set OKRs', 'draft quarterly objectives', 'write key results', 'align team goals with strategy', or 'brainstorm OKRs for [team]'. Trigger terms: OKRs, objectives and key results, quarterly goals, key results, KPIs, north star metric. Not for a full roadmap — use outcome-roadmap for that."
---

# Brainstorm Team OKRs (grounded in customer outcomes)

Generate ambitious, measurable OKRs for the team working on $ARGUMENTS. The difference from a generic OKR exercise: each **Objective maps to a customer outcome the evidence actually supports**, and each **Key Result measures the reduction of a stated pain** — not vanity output like "ship 5 features."

## Step 0 — Relevance & availability
Confirm this is product/team-goal work and Evermuse is present. If the MCP isn't connected, produce OKRs from the framework but label them **⚠ ungrounded** and tell the user to authorize the MCP. (See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`, Step 0.)

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For OKRs, grounding is two-sided:

- **Ground (company direction).** Run **1–2 `guidance` searches** for company objectives, strategy, and north-star metric — the OKRs must ladder up to these. Treat the roadmap / strategy docs as the alignment target.
- **Ground (customer outcomes).** Run **2–3 `evidence` searches** on the pains and desired outcomes in this team's area, plus `find_supporting_quotes(topic, limit: 6)` for the voice behind each candidate objective.
- **Work.** Generate **three distinct OKR sets** (below). Each objective names the customer outcome; each KR names the metric that moves when a *stated* pain shrinks.
- **Cite.** Objectives and KRs that rest on customer signal carry source badges; the `guidance` alignment is cited separately. Preserve `[^n]` into a Sources footer (see `citations.md`).
- **Save.** After confirmation, `add_source(nature: "guidance", source_type: "document", title: "OKRs — <team>/<quarter>", tags: ["evermuse-plugin","okrs"])`. OKRs are company direction → **guidance**.

## Instructions

1. **Domain model (keep these distinct).**
   - **Objective** (Why/What/When): qualitative, inspirational, time-bound (usually quarterly), SMART.
   - **Key Results** (How much): ~3 quantitative metrics + target values.
   - **KPIs / NSM** are interconnected, not alternatives — a KR can *be* a KPI; the NSM is a single customer-centric KPI a KR can express expected change in. Never table them as rivals.
2. **Map objectives to evidenced outcomes.** For each candidate objective, point to the customer outcome it delivers and the quotes behind it. Replace the source skill's "web search for benchmarks" with **Evermuse evidence as the primary source** for what outcomes matter; use general benchmarks only to sanity-check target ambition.
3. **Make KRs measure pain reduction.** Prefer outcome metrics tied to a stated pain (e.g. "cut time-to-first-value from 20→8 min" when customers complained onboarding was slow) over output metrics. Each KR independently measurable, 60–70% confidence (ambitious, not safe).
4. **Generate three credible sets.** Present all three with equal weight — none obviously best — to spark strategic discussion. For each: objective, exactly 3 KRs, and a one-line rationale linking it to both the company strategy and the customer evidence.
5. **Flag data gaps.** If a KR's metric isn't currently instrumented, say so.

## Output format
```
Objective: [inspiring, customer-outcome-oriented statement]   [^evidence]
Key Results:
- [metric → target]        [^evidence / instrumentation note]
- [metric → target]
- [metric → target]
Rationale: [ladders up to <company objective> [^guidance]; addresses <stated pain> [^evidence]]
```
Three sets, then a Sources footer.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT).
- [OKRs 101](https://www.productcompass.pm/p/okrs-101-advanced-techniques) · [OKR vs KPI](https://www.productcompass.pm/p/okr-vs-kpi-whats-the-difference) · [Business vs Product vs Customer Outcomes](https://www.productcompass.pm/p/business-outcomes-vs-product-outcomes)
