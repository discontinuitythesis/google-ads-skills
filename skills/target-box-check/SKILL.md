---
name: target-box-check
description: Check each Google Ads campaign's target CPA or target ROAS against its budget, its actual performance and the business's unit economics, and say whether budget or target is the real constraint. Use before changing a bidding target, when campaigns are limited by budget, or when asked whether a tCPA or tROAS is right.
---

# Target box check

You work out, campaign by campaign, what is actually constraining delivery, and whether the number in the target box came from anywhere. You recommend; you do not change bidding.

## Background you must get right

- A target CPA is an average across conversions, not a per-conversion cap. Target ROAS likewise.
- On 17 August 2026 Google began a gradual rollout for campaigns that are limited by budget and use target-based bidding. Google says affected campaigns will perform more consistently toward their target. Its example: a campaign with a $10 target CPA delivering around $5 because the budget ran out first may move closer to $10. Scope and status of the rollout are in Google's help pages (answer 17061251 and 17125145); check them if the user's date is well after September 2026.
- Consequence at fixed spend: if actual CPA rises toward the target, conversion volume falls. CPA = spend / conversions. That is arithmetic, not a claim about auction mechanics.
- Google documents several responses, including reviewing budgets and targets, its Bid Target Adjustment Tool, and switching to Maximise conversions or Maximise conversion value without a target to seek the most volume or value within a set budget. It documents these as options, not guaranteed outcomes.
- Changing the strategy can start a learning or settling period. Google recommends allowing one or two conversion cycles before judging.

Do not claim more than this. Do not say CPC will rise, or that every campaign will land on its target.

## Inputs

1. **Campaigns** (required): `queries.gaql` query 1 for the last 30 days, and again for the 30 days before 17 August 2026 if the user wants a before and after.
2. **Portfolio strategies** (if used): query 2.
3. **Recent changes** (recommended): query 3.
4. **Unit economics** (ask; this is the most important input). For lead generation: lead to sale rate, average gross margin per sale, and what share of that margin the business is willing to spend to win it. For ecommerce: average gross margin percentage after returns, and the share willing to spend. Also: is the monthly budget a hard cap, and does the business want maximum volume from that budget or a minimum efficiency above all?
5. **Where each target came from** (ask): unit economics, a trailing average, the Google rep, the Bid Target Adjustment Tool, or nobody knows.
6. **Primary conversion actions** (recommended): query 4, so you can check the target is steering towards something real.

## Method

### 1. Derive the break-even target from unit economics

- Lead generation: `max CPA = lead-to-sale rate × gross margin per sale × share willing to spend`. Example: 20% × £1,500 × 40% = £120.
- Ecommerce: `min ROAS = 1 / (gross margin % × share willing to spend)`. Example: margin 40%, willing to spend 50% of margin: 1 / 0.2 = 500%.
- If conversion values are used for bidding, check that they reflect margin or expected contribution, not revenue or Google's default value. If not, say so; the target cannot be better than the values under it.

If the user cannot give unit economics, say plainly that there is no defensible target to check against, and continue with the constraint analysis only.

### 2. Classify each campaign's constraint

For each enabled campaign with a target (campaign-level or portfolio):

- **Budget-limited**: search budget lost impression share above 10%, or spend at or above 95% of budget on most days, or the user confirms it is marked Limited by budget.
- **Headroom**: actual CPA at or below 80% of target (or actual ROAS at or above 125% of target). Under the 17 August change this is the group most likely to see delivery move toward the target.
- **Target-bound**: not budget-limited, actual CPA close to target, impression share lost to rank rising. The target is doing the limiting.
- **Too soon**: a bidding strategy, target or budget change within the last two conversion cycles (use the conversion lag if known, otherwise 30 days for lead generation, 14 for ecommerce). Do not recommend another change; say when it can be judged.
- **Signal problem**: primary conversion actions include weak or duplicated signals (page views, clicks on phone numbers counted as leads, short calls, both a form submit and the offline qualified lead as primary). Fix the signal before touching the target.
- **Shared scope**: shared budget or portfolio strategy. Report at the shared level, not per campaign.

A campaign can be in more than one class; "Too soon" and "Signal problem" take priority.

### 3. Recommend

Match the recommendation to the business objective the user gave, not to a general rule:

- Budget-limited, objective is maximum volume or value from a fixed budget, signal is sound: **test** Maximise conversions or Maximise conversion value without a target (a documented Google option), as an experiment or with a clear before and after window of at least two conversion cycles. Explain that a target on top of a binding budget is a second constraint that cannot raise the theoretical maximum and can remove auctions, and that the live system may still behave differently.
- Budget-limited, but the business has a hard efficiency floor (cashflow, contract, margin): keep a target, set it from the unit economics, and state the expected volume trade-off.
- Target-bound and target is above break-even: the target is costing volume the business would pay for; propose moving it toward break-even.
- Target-bound and target is below break-even: the target is protecting margin; keep it.
- Headroom and not budget-limited: the target is probably inactive; note it and leave it.
- Target came from a trailing average or the rep: say that a trailing CPA mixes seasonality, lag, auction conditions and goal quality, and does not answer what the business wants.

## Output

```
# Target box check: <account>, <date>

**Unit economics:** <break-even CPA or ROAS and how derived, or "not supplied">
**Objective stated:** <maximum volume from fixed budget / efficiency floor / not stated>

| Campaign | Strategy | Target | Actual (30d) | Budget lost IS | Constraint | Target source | Recommendation |

## Detail
### <Campaign>
<three to six sentences: the evidence, the constraint, the recommendation, what to watch, when to judge it>

## Fix before you change any target
<signal problems, or "none found">

## Limits of this check
<one to three sentences>
```

The free web version of this check is at https://firepixel.co.uk/tools/target-box-checker.

## Rules

- Never change a strategy, target or budget. Recommend and let a person decide.
- Never present Google's documented options as guaranteed outcomes.
- Never recommend a change on a campaign in the "Too soon" class.
- Before and after comparisons around 17 August are evidence worth monitoring, not proof of cause, unless the user ran a controlled test.
