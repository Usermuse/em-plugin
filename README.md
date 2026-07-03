# Evermuse for Claude Code

**Make your AI agent speak with the voice of your customers.**

Evermuse is a Claude Code plugin that turns everyday product work — specs, dev plans, research, roadmaps, gap analyses, PR reviews — into deliverables grounded in **real customer evidence**. Instead of plausible-sounding generic answers, you get verbatim quotes, real needs, and actual pain points, each with a source link back to the conversation it came from. And the important things you produce get saved back into Evermuse, so your knowledge base compounds.

It combines proven PM/research methods (adapted from [pm-skills](https://github.com/phuryn/pm-skills)) and spec-driven-development discipline (adapted from [spec-kit](https://github.com/github/spec-kit)) with the [Evermuse MCP](https://evermuse.com), which searches your recorded calls, interviews, and feedback.

## What changes after you install it

Ask *"what do customers think about our onboarding?"* and instead of a guess you get:

> **Short answer:** Onboarding friction centers on manual data entry — 7 mentions across 5 accounts, mostly frustrated.
>
> ### Theme 1 — Re-keying data by hand (7 mentions / 5 accounts)
> > "We lose half a day every week re-keying this into the spreadsheet." — Dana K., Acme onboarding call, 12 May 2025 · [View in Evermuse](#) [^1]

Every spec gets a Customer Evidence section. Every dev plan opens with the customer quotes that justify it. Every PR review checks the diff against what customers literally asked for. That's the difference.

## Install

```bash
# Add this repo as a plugin marketplace, then install the plugin
claude plugin marketplace add evermuse/em-plugin
claude plugin install evermuse@evermuse
```

Or point at a local checkout during development:

```bash
claude plugin marketplace add /path/to/em-plugin
claude plugin install evermuse@evermuse
```

## Authenticate the MCP

The plugin bundles the Evermuse MCP server (`https://api.evermuse.com/api/mcp`). Before the skills can ground anything, connect it:

- **OAuth (recommended):** run `/mcp` in an interactive Claude Code session and authorize **evermuse**.
- **API key:** add an `Authorization` header for the `evermuse` server in your MCP settings.

Then run **`/evermuse:setup`** to pick your Product and confirm grounding works. (A **Product** is required for every Evermuse call; a **Project** like Discovery/Support/Sales is optional and only needed when you're focused on one research effort.)

## Quickstart

```
/evermuse:setup                      # connect + pick your product
/evermuse:research what do customers think about pricing?
/evermuse:spec bulk CSV export for the reporting page
/evermuse:dev-plan specs/bulk-csv-export/spec.md
/evermuse:review-pr 482
```

## The doctrine (how grounding works)

Every skill follows **Ground → Work → Cite → Save**:

1. **Ground** — verify the right Product, then run 2–4 differently-worded searches (declaring a *nature*: `evidence` = customer voice, `context` = market, `guidance` = company strategy) plus verbatim quotes.
2. **Work** — apply the skill's method, shaped by what the evidence says.
3. **Cite** — every customer-derived claim carries a source badge and a `[^n]` footnote.
4. **Save** — offer to persist the deliverable back into Evermuse via `add_source`.

The full doctrine lives in the `using-evermuse` skill, which every other skill defers to.

## Commands

Flagship: `/evermuse:setup` · `spec` · `dev-plan` · `gap-analysis` · `review-pr` · `research` · `release-notes` · `save`

Discovery & execution: `discover` · `brainstorm` · `triage-requests` · `interview` · `write-prd` · `write-stories` · `plan-okrs` · `transform-roadmap` · `test-scenarios` · `pre-mortem` · `red-team-prd` · `meeting-notes` · `ship-check`

Strategy, research & GTM: `strategy` · `business-model` · `value-proposition` · `market-scan` · `pricing` · `research-users` · `competitive-analysis` · `analyze-feedback` · `plan-launch` · `growth-strategy` · `battlecard` · `market-product` · `north-star`

Skills also **auto-activate** when you describe the task in plain language ("write a spec for…", "what are customers saying about…", "review this PR") — you don't have to remember command names.

## Skills catalog

- **Foundation:** `using-evermuse` (the doctrine + MCP mechanics every skill uses)
- **Flagships:** `write-feature-spec`, `development-plan`, `gap-analysis`, `review-pr`, `customer-research`
- **Discovery:** `brainstorm-ideas`, `assumptions`, `prioritize-features`, `analyze-feature-requests`, `opportunity-solution-tree`, `interview-script`, `summarize-conversation`
- **Strategy:** `product-strategy`, `product-vision`, `value-proposition`, `business-model`, `pricing-strategy`, `strategy-frameworks`, `strategy-red-team`
- **Execution:** `create-prd`, `user-stories`, `brainstorm-okrs`, `outcome-roadmap`, `release-notes`, `test-scenarios`, `shipping-artifacts`
- **Market research:** `user-personas`, `segmentation`, `customer-journey-map`, `market-sizing`, `competitor-analysis`, `sentiment-analysis`
- **GTM & growth:** `gtm-strategy`, `beachhead-segment`, `ideal-customer-profile`, `competitive-battlecard`, `positioning-and-messaging`, `north-star-metric`

## Tips

- The plugin works best when your project's `CLAUDE.md` mentions the product name — it helps the agent pick the right Evermuse Product automatically. (Optional.)
- If Evermuse isn't connected, skills still run but produce **⚠ ungrounded** output and tell you to authorize the MCP — they never fake evidence.
- Secondary Evermuse assets (the AI-generated roadmap, shaping notes, competitor list, research questions) are used sparingly and always labeled — customer conversations are the source of truth.

## Credits

Built on [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (MIT) and [github/spec-kit](https://github.com/github/spec-kit) (MIT). See [ATTRIBUTION.md](ATTRIBUTION.md). Licensed MIT — see [LICENSE](LICENSE).
