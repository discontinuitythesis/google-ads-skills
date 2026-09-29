---
name: weekly-exception-note
description: Write the weekly Google Ads exception note for a client or account owner, reporting only what moved outside its normal range, the likely cause, and what needs a decision. Use for Monday account checks, weekly PPC reports or when asked what changed in a Google Ads account last week.
---

# Weekly exception note

You compare last week with its baseline and write a short note that a business owner can read in two minutes. You report exceptions, not everything. You propose actions; you do not make them.

A note that says nothing moved is a valid and useful note. Send it anyway. Silence reads as inactivity.

## Inputs

Ask for what is missing, then work with what you have and say what you did not have.

1. **Campaign weekly data** (required): `queries.gaql` query 1 for the last 5 complete weeks (Monday to Sunday).
2. **Conversions by action** (required): query 2 for the same weeks.
3. **Change history** (recommended): query 3 for the last 14 days.
4. **CRM outcomes** (recommended): leads by week with status (qualified, won, lost, open) and value, and the source of each (Google Ads by click ID or UTM).
5. **Ecommerce only**: Merchant Center disapprovals or out-of-stock counts for the week, if the user has them.
6. **Known events** (ask once): site releases, form or CRM changes, promotions, stock problems, bank holidays, anything the business changed.
7. **Conversion lag**: days until about 90% of qualified leads are recorded. If search-term-review has been run, reuse its figure. Otherwise ask, or assume 30 days for lead generation and 7 for ecommerce and say so.

## Baseline and thresholds

- Baseline: the average of the 4 complete weeks before last week.
- Flag a metric when last week differs from baseline by more than 20% **and** the absolute change matters (at least 5 conversions, or at least 10% of weekly spend, or at least one qualified lead where weekly qualified leads average under 10). Small accounts wobble; do not flag noise.
- Offline conversions for the last `lag` days are incomplete. Compare them only against the same point in previous weeks' maturity, or leave them out and say why.

## The questions to answer

Work through these in order. Each becomes a section of the note only if it produced an exception.

1. **Did lead or order quality move differently from platform CPA?** Compare cost per platform conversion with cost per qualified lead (or per won deal, or gross profit per order) using matured weeks. The classic exception: CPA improved while qualified rate fell, which usually means cheaper, weaker leads or a conversion action counting something new.
2. **Did the conversion signal change rather than the business?** A step change in one conversion action while others hold steady, a new action appearing, an action dropping to zero, or a change in conversions per click on one landing page. Cross-check with change history and known site releases. A tracking break is the most expensive thing this note can catch.
3. **Is a movement real demand, or attribution?** If conversions moved, did impressions and search impression share move with them? Did CRM leads from all sources move the same way? A drop in Google-attributed conversions with flat CRM leads points at attribution or tracking, not demand.
4. **What is each campaign limited by?** For campaigns that under-delivered: budget (search budget lost impression share up, spend at cap), rank (rank lost impression share up), demand (impressions down with impression share steady), inventory (ecommerce: disapprovals or stock). Say which.
5. **Did anyone change anything?** List account changes from change history in the last 14 days that could explain an exception, by who or what made them (interface, API, Google automated). Remember a bidding change needs one or two conversion cycles before it can be judged.
6. **Is spend on track?** Spend against budget for the month to date, if a monthly budget is known.

## Output

Use exactly this structure. Plain language, no jargon the client would not use. Numbers with the previous number beside them.

```
# <Account name>: week of <Monday date>

**Summary:** <one or two sentences. If nothing moved: "Spend, qualified leads and conversion mix within normal range. No account changes made.">

## Exceptions
### <Short title of exception 1>
- **What moved:** <metric, last week vs baseline, change>
- **Evidence:** <the one or two figures that show it>
- **Likely cause:** <cause, with confidence: high, medium or low>
- **Proposed action:** <action, or "watch another week"> · **Needs approval:** yes/no

(repeat for each exception, most costly first, at most five)

## Changes made last week
<list, or "none">

## Not checked this week
<what data was missing, in one line>
```

## Rules

- At most five exceptions. If there are more, keep the five with the most money at stake and list the rest in one line.
- Confidence is honest. "Low" is fine. Never present a guess as a cause.
- Do not call a movement inside the conversion lag window a trend.
- Do not change the account, even if a tool allows it.
- Do not include personal data (names, emails, phone numbers) from the CRM in the note.
