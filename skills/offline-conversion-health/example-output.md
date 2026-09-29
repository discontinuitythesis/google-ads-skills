# Offline conversion health: Example Roofing Ltd, July to September 2026

*Fictional account. Figures invented to show the format.*

**Chain:** 412 paid leads → 318 captured (77%) → 104 qualified → 96 uploaded → 91 accepted (95%)
**Biggest leak:** Capture. 94 Google Ads leads reached the CRM with no click ID, almost all from the quote form on /flat-roofs.

| Link | Check | Result | Figure | Note |
|---|---|---|---|---|
| Capture | Capture rate | Warn | 77% | /flat-roofs quote form 31%, all other forms 96% |
| Capture | Auto-tagging | Pass | On | |
| Capture | gbraid / wbraid | Fail | 0 captured | 22% of paid clicks are iOS; form has no field for them |
| CRM | Leads with no status after 28 days | Warn | 37 | Mostly July leads never updated |
| CRM | Qualified with no value | Pass | 2 of 104 | |
| Upload | Freshness | Pass | Daily, last 23 Sep | |
| Upload | Outside click window | Warn | 8 of 104 | Action window 30 days; median click to survey 19 days, tail to 70 |
| Upload | Time zone | Pass | Header row | |
| Upload | Order ID | Fail | Missing | Rows re-sent in August with an updated time double counted 11 conversions |
| Upload | Consent columns | Warn | Always "Granted" | Hard-coded; banner acceptance on the site is about 64% |
| Match | Success rate | Pass | 95% | 5 rejected rows were test leads with made-up click IDs |
| Use | Offline action primary | Pass | Yes | |
| Use | Double counting | Fail | Form submit also primary | Each qualified lead counts twice |
| Use | Enhanced conversions for leads | Warn | Not set up | Would recover some of the 94 |

## Fix first
1. Add the hidden click ID fields to the /flat-roofs quote form and capture gbraid and wbraid on every form. Recovers most of the 94 missing leads. Site developer, under an hour.
2. Set "Form submit" to secondary. Stops every qualified lead counting twice in bidding. Account manager, five minutes, but expect reported conversions to drop; brief the client first.
3. Add Order ID to the upload and pass real consent state from the form. Upload owner.
4. Extend the survey action's click-through window to 90 days.

## What this check could not see
Paid lead count is estimated from UTM source and Google Ads form conversions, which may miss leads that arrived by phone. Call leads were not in the CRM export.
