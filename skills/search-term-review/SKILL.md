---
name: search-term-review
description: Review a Google Ads search terms report for wasted spend and new keyword opportunities, allowing for offline conversion lag so terms whose qualified leads have not been uploaded yet are not cut. Use when asked to mine search terms, find negative keywords or review search query reports.
---

# Search term review (lag-aware)

You review a Google Ads search terms report and propose negative keywords and new keywords. You never apply changes. You produce a proposal for a person to approve.

The failure this skill exists to prevent: a rule like "last 7 days, spend over X, zero conversions, add as exact negative" cuts the best terms in any account that imports offline conversions. Google Ads reports an uploaded conversion against the date of the click, so a lead that qualifies in the CRM three weeks after the click only appears against that click once the upload runs. Until then the term looks like waste.

## Inputs

Ask for whichever of these the user has not supplied. Work with what you get and say what was missing.

1. **Search terms** (required): `queries.gaql` query 1, or an interface export with campaign, ad group, search term, match type or status, clicks, cost, conversions, conversion value. At least 90 days if possible.
2. **Conversion lag** (strongly recommended): query 2 (lag buckets by conversion action), or the CRM export below, or the user's own answer to "how many days from click to a lead being marked qualified, for most leads?"
3. **CRM leads** (optional): lead created date, click ID (gclid, gbraid or wbraid), current status, and status date. Plus query 3 (click_view) for the same days so click IDs can be joined to campaign, ad group and keyword.
4. **Existing negatives** (optional): query 4, and any shared negative lists the user exports.
5. **Account context** (ask once): what the business sells, what a qualified lead or a good sale is, brand terms, competitor terms they do or do not want, services they do not offer, locations they do not serve.

## Method

### 1. Work out the lag window

Take the conversion actions used for bidding (primary). For each, find the number of days by which about 90% of conversions have been recorded (the "p90 lag"). From query 2, sum conversions by lag bucket and read where the cumulative share passes 90%. From the CRM, use the days between lead created and qualified status.

If the primary actions are only online (form submit, purchase with a tag), the lag is usually a few days; say so and use 7 days.

If you cannot measure it, ask the user. If they do not know, use 30 days for lead generation and 7 for ecommerce, and flag the assumption at the top of the output.

Call this `L`.

### 2. Split the period

- **Mature window**: clicks older than `L` days. Conversion data here is close to complete.
- **Immature window**: the last `L` days. Conversions here are still arriving.

Judge waste on the mature window only. Use the immature window for spend trend and for spotting brand new terms, never as evidence of zero conversions.

If the mature window is shorter than 30 days because the data pulled does not go back far enough, say so and ask for a longer pull before proposing negatives on anything other than clear irrelevance.

### 3. Classify every term with spend

Put each term in exactly one class:

- **Irrelevant**: the searcher cannot be a customer (jobs, free, DIY where the business is a service, a service not offered, a location not served, a different product with the same word). These can be negated whatever the conversion data says, because relevance does not depend on lag.
- **Competitor**: follow the user's instruction. If none, list separately and do not negate.
- **Brand**: never negate. Note brand terms appearing in non-brand campaigns as a structure issue.
- **Relevant, converting**: conversions in the mature window. Candidates to add as keywords if not already one.
- **Relevant, no conversions yet**: relevant, but zero conversions in the mature window. Candidate for negation only if mature-window cost exceeds the threshold below.
- **Relevant, too early**: relevant, and most of its clicks are in the immature window. Never negate. List them so they get checked next run.

Threshold for "no conversions yet": mature-window cost greater than 2 × the account's mature-window CPA for the relevant conversion action. If the account uses value-based bidding, use 2 × cost per unit of value at the account's average value per conversion. Tell the user the threshold you used.

### 4. Hold anything with open leads

If you have CRM data joined to click_view: for each ad group, count leads from the mature and immature windows whose status is still open (not qualified, not lost). click_view gives the campaign, ad group and keyword for a click ID, not the search term, so this hold works at ad group level.

Any "no conversions yet" term in an ad group with open leads from the period moves to **Hold: leads pending** rather than a negative. State how many open leads the ad group has.

### 5. Choose match type and level for each proposed negative

- Exact match negative for a single bad query that shares words with good ones.
- Phrase match negative for a bad concept that will recur ("jobs", "salary", "free", "course").
- Campaign level if the term is only wrong for that campaign; account-level or shared list if wrong everywhere.
- Check against existing negatives and do not propose a duplicate.
- Check every proposed phrase or broad negative against the converting terms list. If it would block a converting term, say which and downgrade it to exact.

### 6. New keyword candidates

Relevant, converting terms that are not already keywords (search term status `NONE` or `EXCLUDED` in query 1), with at least 2 conversions in the mature window. Propose the ad group and match type.

## Output

Use exactly this structure. Tables in Markdown. Currency in the account currency with two decimals.

```
# Search term review: <account name>, <date range>

**Lag window used:** <L> days (<how it was measured, or "assumed">)
**Mature window:** <start> to <end> · **Immature window:** <start> to <end>
**Waste threshold:** <amount> mature-window cost with zero conversions (<how derived>)
**Data missing:** <list, or "none">

## Proposed negatives (needs approval)
| Term | Negative | Match | Level | Mature cost | Clicks | Reason |

## Hold: leads pending
| Term | Ad group | Mature cost | Open leads in ad group | Recheck on |

## Too early to judge
| Term | Cost (immature) | Clicks | First seen |

## Proposed new keywords (needs approval)
| Term | Ad group | Match | Conversions | Conv. value | CPA |

## Competitor and brand notes
<short prose or "none">

## Totals
Proposed negatives would have removed <amount> of mature-window spend (<share>% of total). <amount> is on hold pending leads.

## What this review could not see
<one to three sentences>
```

Every proposed negative carries a reason a person can check in five seconds. If there is nothing worth proposing, say so; an empty review is a result.

## Rules

- Do not apply, upload or schedule any change, even if a tool that can do so is available, unless the user explicitly approves a specific list in this conversation.
- Do not treat conversions from the immature window as complete, ever.
- Do not negate brand terms.
- Do not invent conversion lag. Measured, stated by the user, or assumed and flagged.
- Performance Max search term insights are category-level and cannot take a negative in the same way; if PMax data is supplied, report it separately and do not propose keyword negatives from it.
