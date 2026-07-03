---
name: gtm-strategy
description: "Build a go-to-market strategy — messaging, channels, segment focus, GTM motions, metrics, and a launch timeline — where every choice is anchored in real customer evidence, not guesswork. Use when the user says 'plan the launch', 'build a GTM plan', 'how do we take this to market', 'what channels should we use', 'inbound or outbound', 'go-to-market strategy', or 'launch strategy'. Trigger terms: go-to-market, GTM, launch plan, GTM motions, marketing channels, launch strategy, distribution. Not for a single feature spec or an internal roadmap — this is how the product reaches its market."
---

# GTM Strategy

Produce a go-to-market strategy — target segment, messaging, channel/motion mix, success metrics, and a phased launch plan — where the segment choice, each message, and each channel is justified by what customers actually said and the market context around them. Grounding turns a plausible launch deck into one you can defend with quotes.

## Step 0 — Relevance & availability
Confirm this is a launch / go-to-market task and Evermuse is connected. If the tools aren't present, produce a framework-only GTM plan labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product (`get_products`/`switch_product`). Then run **2–3 `evidence` searches** for the pains and desired outcomes that drive messaging ("biggest pain the product relieves", "outcome customers want", "why they chose / almost didn't"), **1 `context` search** for the competitive/market landscape ("alternatives customers compare us to", "market shifts"), and **1 `guidance` search** for any existing launch/positioning direction so you extend it. Pull verbatim voice with `find_supporting_quotes(topic, limit: 6–8)`. Use `get_meetings(attendee_domain)` to see which segments show up most in the corpus when the target segment is unsettled.
- **Work:** fill the strategy below. The **target segment** is where evidence shows the strongest, most-repeated pain; **messaging** is written in the customer's own words (each message maps to a quote); **channel/motion choice** is grounded in where evidence shows these customers actually discover and buy tools like ours.
- **Cite:** every segment, message, and channel rationale carries a source badge (see `citations.md`). Keep `evidence` (customer voice) separate from `context` (market) in the output.
- **Save (nature=guidance):** after the user confirms, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","gtm","go-to-market"])`.

## Instructions

You are planning the go-to-market for **$ARGUMENTS**. Where the source method reaches for "web search / user-provided data," use Evermuse `evidence` (customer voice) as the primary source and `context` (market) as secondary.

### 1. Target segment & positioning
Name the segment you launch into and why — the strongest, most-cited burning pain wins. Anchor it in an `evidence` quote. (If the beachhead is genuinely undecided, run `/evermuse:beachhead-segment` first and feed its result here.) State the one-line value proposition in the customer's language.

### 2. Messaging — mined, not invented
Develop messaging where **every message maps to a verbatim quote that proves it resonates**:
- Core value proposition for the segment (the "what after" they described).
- 3–5 key differentiators — each tied to a pain quote (evidence) or a gap in an alternative (context).
- Proof points: the sharpest customer quotes, reusable as social proof.
- Channel-specific variations only where the audience differs.
Reject any message you can't back with a quote — that is the tell of a generic deck.

### 3. Channels & GTM motions
Choose the acquisition mix. Score the **7 GTM motions** for this product (below), ground the top choices in where evidence shows customers actually found tools like ours, then commit to a **motion stack of 2–4**: one primary, the rest complementary, in a sequence.

**The 7 GTM motions** (pick the fit, don't run all seven):
- **Inbound** — content, SEO, webinars, thought leadership. Best for B2B SaaS, technical buyers, long cycles. Slow to compound but builds authority.
- **Outbound** — cold email, LinkedIn, ABM-style prospecting. Best for enterprise, high ACV, niche lists. Predictable but resource-heavy.
- **Paid digital** — search/social/display ads, retargeting. Fast and measurable; expensive and needs constant tuning.
- **Community** — forums, Slack/Discord, user groups, ambassadors. Low CAC, high loyalty; slow to reach critical mass.
- **Partners** — integrations, co-marketing, marketplaces, resellers. Borrowed reach and credibility; alignment overhead.
- **ABM** — high-value accounts treated as individual markets. Higher conversion and deal size; not SMB-scalable.
- **PLG** — free trial/freemium, in-app onboarding, self-serve. Low CAC and strong PMF signal; demands an excellent product experience.

Match to product reality: ACV, sales-cycle length, buyer type, complexity, and where the evidence shows these customers already hang out. Most launches win with 2–4 complementary motions (e.g. inbound for brand + outbound for pipeline; PLG to lower CAC + paid to accelerate a proven channel).

### 4. Success metrics
Define KPIs across the funnel: awareness (reach), engagement (CTR, activation), conversion (signups, demos, trials), revenue (MRR, CAC, LTV), and market (segment penetration). Establish baselines before launch. For a formal North Star + input-metric tree, hand off to `/evermuse:north-star-metric`.

### 5. Launch plan (90 days)
Phase it: pre-launch (assets, channel setup, baseline metrics) → launch (announcement, activities) → post-launch momentum (content, partnerships, community) → measure & optimize cadence, with go/no-go criteria.

## Deliverable format

```markdown
# GTM Strategy — [product]

**Target segment:** [segment] — burning pain: > "[quote]" — [attribution] [^1]. Why now: [context, cited [^2]].

**Positioning (one line):** [value prop in customer language]

**Messaging**
| Message | Proof quote (evidence) | Maps to differentiator |
|---|---|---|
| [message] | > "[verbatim]" — [attribution] [^n] | [diff] |

**Motion stack:** Primary — [motion] ([why, grounded [^n]]); Secondary — [motion(s)]. Sequence: [order].

**Success metrics:** North Star [metric] · Funnel KPIs [awareness → revenue] · Baselines [values].

**90-day launch plan:** Pre-launch → Launch → Post-launch → Optimize. Go/no-go: [criteria].
---
## Sources
[^1]: …
```

## Save & hand off
After saving, offer next steps: **"Lock the beachhead?"** (`/evermuse:beachhead-segment`), **"Sharpen positioning & campaigns?"** (`/evermuse:positioning-and-messaging`), **"Define the North Star?"** (`/evermuse:north-star-metric`), or **"Build a battlecard for the top competitor?"** (`/evermuse:competitive-battlecard`).

---
### Further reading
- Merged and adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) `gtm-strategy` + `gtm-motions` (MIT); grounding is Evermuse-native.
- [5 GTM Principles You Should Know as a PM](https://www.productcompass.pm/p/5-gtm-principles-with-frameworks-templates)
- [OpenAI's Product Leader's 3-Layer Distribution Framework](https://www.productcompass.pm/p/distribution-framework-ai-products)
- [How to Design a Value Proposition Customers Can't Resist?](https://www.productcompass.pm/p/how-to-design-value-proposition-template)
