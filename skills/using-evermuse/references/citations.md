# Citations & Source Badges

The whole point of this plugin is that customer-derived claims are **visibly** backed by real evidence. A reader should be able to see the quote, who said it, and click through to the source. This file defines exactly how.

## The rule

Every statement in a deliverable that rests on customer input gets a **source badge**. If you wrote "customers want X", a reader must be able to see *which* customers and *where they said it*. No badge → it reads as your opinion, and the plugin has failed its promise.

## Preferred format (when the result carries a resource link)

Evermuse results are gaining **resource links** (deep links back into the app). When a result has one, render an inline quote block:

```markdown
> "We lose half a day every week re-keying this into the spreadsheet."
> — Dana K., Acme onboarding call, 12 May 2025 · [View in Evermuse](RESOURCE_LINK) [^3]
```

For a claim summarizing several items, badge it inline:

```markdown
Re-keying data by hand is the most-cited onboarding pain (7 mentions across 5 accounts). [^3][^5][^9]
```

## Fallback format (no resource link yet)

Resource links are being rolled out; many results won't have one yet. Keep everything except the link — the attribution still makes the claim verifiable:

```markdown
> "We lose half a day every week re-keying this into the spreadsheet."
> — Dana K., Acme onboarding call, 12 May 2025 [^3]
```

Pull attribution from the result fields: `who_said_it`, `meeting_name`, `created_at`/`meeting_start` (format as a human date). If a field is missing, include what you have (e.g. "— enterprise customer, sales call").

## The Sources footer

Every Evermuse result carries a `footnote_marker` like `[^3]` and a `marker_id`. **Preserve these markers** — reuse the exact number the tool gave you rather than renumbering. At the bottom of the deliverable, collect them:

```markdown
---
## Sources

[^3]: Dana K. — Acme onboarding call, 12 May 2025 · [View in Evermuse](RESOURCE_LINK)
[^5]: Miguel R. — Beta feedback, 3 Jun 2025
[^9]: Support ticket #4821, 18 Jun 2025
```

Include the link in the footer entry when present; omit it when not.

## Sentiment & strength

`find_supporting_quotes` returns `sentiment_analysis` and `emotion`. When it sharpens the point, surface it: "voiced with clear frustration", "an enthusiastic ask". When you make a demand-strength claim ("most-cited", "7 mentions"), it must reflect the actual count of distinct items/accounts you saw — never inflate.

## What not to cite as customer voice

Shaping notes, research questions, the competitor list, and the updated roadmap are internal/AI-generated assets. If you reference them, label them as such ("per the team's draft roadmap (AI-generated)") — never dress them up as a customer quote.

## Hard rules

- Never fabricate a quote, speaker, meeting, link, or footnote number. If you don't have a real quote, say the point is a hypothesis and mark it — don't invent evidence.
- Never renumber or collide footnote markers — carry through the `[^n]` the tool assigned.
- Verbatim means verbatim: quote the customer's actual words (light trimming with `…` is fine; paraphrase inside quote marks is not).
