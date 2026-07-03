---
name: strategy-red-team
description: "Adversarially attack a strategy, PRD, or roadmap by hunting the customer counter-evidence that falsifies its claims — 'you assume X, but three customers said the opposite'. Includes a pre-mortem mode. Use when the user says 'red-team this', 'poke holes in this', 'stress-test our strategy', 'pressure-test this plan', 'what could go wrong', or 'run a pre-mortem'. Trigger terms: red team, stress test, pressure test, challenge assumptions, poke holes, pre-mortem, kill criteria. Not for polishing or copy-editing a doc."
---

# Strategy Red-Team (attack the assumptions with counter-evidence)

Be a sharp, fair adversary against a strategy/PRD/roadmap — but a *grounded* one. The differentiator over a generic red-team: every attack points to **real customer counter-evidence** where it exists. Not "this might be risky" but "the doc assumes X; three customers said the opposite [^n]." Your grounding searches are designed to **falsify** the document's claims, not confirm them.

## Step 0 — Relevance & availability
Confirm there's a concrete document to attack and Evermuse is connected. If not connected, run a framework-only red-team labeled **⚠ ungrounded — Evermuse not connected** (assumption logic only, no counter-evidence) and tell the user to authorize the MCP. See `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md` Step 0.

## Mode
- **Red-team (default):** attack load-bearing assumptions *now*, while the cheapest test is still available.
- **Pre-mortem mode:** if the user asks to "run a pre-mortem" or imagine the launch already failed, use `references/pre-mortem-template.md` (Tigers / Paper Tigers / Elephants → launch-blocking / fast-follow / track). Same counter-evidence grounding applies.

## Evermuse Grounding (required — searches must try to FALSIFY the doc)
Follow `${CLAUDE_PLUGIN_ROOT}/skills/using-evermuse/SKILL.md`. For this skill:
- **Ground:** verify the product. First list the doc's **load-bearing claims** about the user/market/mechanism/timeline. Then run **2–3 `evidence` searches worded to contradict them** — for a claim "users want fewer steps," search "users who wanted more control / more options" and "complaints about oversimplified flows." Pull the sharpest opposing quotes with `find_supporting_quotes("<the opposite of the claim>", limit: 6–8)`. Add **1 `context` search** to test market claims ("competitor already does X", "market moving away from Y").
- **Work:** steelman each load-bearing claim, then attack the steelman — anchoring the attack in the counter-evidence you found. Rank by impact × likelihood-wrong × cheapness-to-test.
- **Cite:** every counter-evidence attack carries a source badge and the opposing quote (see `citations.md`). If a claim genuinely holds up under a falsifying search, **say so plainly** — a red-team that manufactures doubt is as useless as one that rubber-stamps. Never fabricate a weakness the evidence doesn't support.
- **Save (nature=guidance):** after confirmation, `add_source(nature: "guidance", source_type: "document", tags: ["evermuse-plugin","red-team","<red-team|pre-mortem>"])`.

## Instructions

You are a sharp, fair adversary reviewing **$ARGUMENTS**.

1. **Extract every claim.** List what the doc asserts as true. Separate **load-bearing** (if false, the plan dies) from cosmetic. Only load-bearing claims are worth attacking.
2. **Design falsifying searches.** For each load-bearing claim, phrase an Evermuse search that would surface the *opposite* — the customers who'd disagree. This is the core move.
3. **Steelman, then attack.** State the strongest version of the claim; then attack *that*, anchored in the counter-evidence found. No strawmen.
4. **Write each failure mode as "Fails if ___."** Concrete and falsifiable. "Fails if activation isn't actually the constraint" beats "execution risk."
5. **Rank by (impact if wrong) × (likelihood wrong) × (cheapness to test).** Surface the top of the list — the highest-impact, plausibly-wrong, cheap-to-check assumption is what to test *this week*.
6. **For each surviving kill-assumption, give the operator something to do:** *Fails if* · *Counter-evidence* (the opposing quotes, cited) · *Evidence to get this week* · *Kill criterion* (the threshold to stop/change) · *Cheapest test*.
7. **Self-refute, don't fabricate.** Default to "this risk is real" unless a falsifying search comes back empty or the doc already cites evidence against it — then say the claim holds.

Where the source method reaches for "web search / market research," use Evermuse `evidence` (falsifying customer voice) as the primary source and `context` (market) as secondary.

## Deliverable format

```markdown
## Red-Team: [plan in one line]

### Top Kill-Assumptions (ranked)
For each (3–5 max):
- **Claim:** [the load-bearing assertion in the doc]
- **Counter-evidence:** > "[opposing quote]" — [attribution] [^n]  (you assume X; [N] customers said the opposite)
- **Fails if:** [concrete, falsifiable condition]
- **Evidence to get this week:** [specific]
- **Kill criterion:** [threshold]
- **Cheapest test:** [smallest experiment]

### What's Well-Reasoned
[Claims that survived a falsifying search — state why, cite the confirming evidence.]

### What I Couldn't Assess
[Gaps where neither the doc nor Evermuse gave enough to judge.]
---
## Sources
[^n]: …
```

End with what to *do*, not just what to fear — the emotional job is relief from shipping the wrong bet.

---
### Further reading
- Adapted from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT), merging its `strategy-red-team` and `pre-mortem` skills; grounding is Evermuse-native.
- [Assumption Prioritization Canvas](https://www.productcompass.pm/p/assumption-prioritization-canvas)
- [How Meta and Instagram Use Pre-Mortems](https://www.productcompass.pm/p/how-to-run-pre-mortem-template)
