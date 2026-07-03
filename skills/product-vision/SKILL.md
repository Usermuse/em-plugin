---
name: product-vision
description: "Craft an inspiring, achievable product vision anchored in customers' own words about the future they want — not internal aspiration alone. Use when the user says 'write a product vision', 'vision statement', 'what's our north star', 'align the team on direction', 'where are we headed', or 'refine our vision'. Trigger terms: product vision, vision statement, north star, aspirational direction, team alignment, future state. Not for quarterly goals or roadmaps — this is the long-horizon why."
---

# Product Vision

Brainstorm a product vision that is inspiring, achievable, and emotional — and grounded in the future customers themselves describe wanting. A vision built from customer-desired-outcome quotes lands as credible and shared, not as a slogan invented in a room.

## Step 0 — Relevance & availability
Confirm this is product-direction work and Evermuse is connected. If not, produce a framework-only vision labeled **⚠ ungrounded — Evermuse not connected** and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Evermuse Grounding (required)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground (guidance first):** verify the product. Run **1 `guidance` search** for any existing vision/values/objectives ("company vision and values", "long-term objectives") so the new vision extends the real one. Then **2 `evidence` searches** worded to surface the *future customers want* — their desired end state, not just today's pain ("what customers wish were possible", "the outcome they're really after / 'if only we could…'"). Add **1 `context` search** for where the market is heading ("where the market is moving", "emerging expectations"). Use `find_supporting_quotes("desired future outcome", limit: 6–8)` for the verbatim aspiration.
- **Work:** distill recurring customer-desired outcomes into the vision. The vision must echo language customers actually used.
- **Cite:** each aspirational claim carries a source badge tying it to a customer quote about the future they want (see `citations.md`).
- **Save (nature=guidance):** after the user confirms, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","vision","product-vision"])`.

## Instructions

You are a veteran product leader developing a vision for **$ARGUMENTS**. A great vision is memorable, communicable in one sentence, and makes people *feel* something.

The vision must be:
1. **Inspiring** — motivates the team to commit.
2. **Achievable** — credible given resources, market, and capabilities.
3. **Emotional** — creates meaning and connection.

### Process
1. Read any existing vision/values from the `guidance` search — evolve it, don't discard it.
2. Identify the **core problem** and, more importantly, **the future customers say they want** — pulled verbatim from evidence. This is the anchor: the vision describes the world customers are asking for, in their words.
3. Draft **3–5 vision variations**, each traceable to clustered customer-desired-outcome quotes.
4. Select the strongest; explain the rationale in terms of the customer aspiration it captures and the market shift (context) that makes it timely.
5. Show how it aligns with company values (guidance) and the market opportunity (context).

Where the source reaches for "files from the workspace," use Evermuse `evidence` — customers' own words about the future they want — as the primary source.

## Deliverable format

```markdown
# Product Vision — [product]

**Vision (recommended):** "[one memorable sentence in customers' own register]"

*Why this one:* it captures the future customers keep describing —
> "[verbatim quote about the outcome they want]" — [attribution] · [View in Evermuse](LINK) [^1]

**Alternatives considered:** [2–4 short variants, each tagged to the evidence cluster it came from]

**Alignment:** values [guidance ^n] · market tailwind [context ^m]
---
## Sources
[^1]: …
```

Keep it honest: if evidence for a desired future is thin, say so and mark the vision as a hypothesis worth testing in interviews (`/evermuse:interview` if available).

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT); grounding is Evermuse-native.
- [Product Vision vs Strategy vs Objectives vs Roadmap](https://www.productcompass.pm/p/product-vision-strategy-goals-and)
