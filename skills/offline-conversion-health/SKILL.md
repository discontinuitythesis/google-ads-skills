---
name: offline-conversion-health
description: Check the Google Ads offline conversion chain end to end, from click ID capture on the form through the CRM and the upload to matched conversions, and report where leads are being lost. Use when offline conversion imports look low, after a site, form or CRM change, or as a weekly check on a lead generation account.
---

# Offline conversion health

Offline conversions are how a lead generation account tells Google which leads were worth having. The chain has five links and each one leaks quietly:

1. **Capture**: the landing URL carries a click ID (gclid, or gbraid or wbraid from iOS), the site stores it, and the form sends it with the lead.
2. **CRM**: the lead arrives with its click ID intact and gets a status and value as it progresses.
3. **Upload**: qualified leads are sent to Google Ads with click ID, conversion action, time, value, currency, order ID and consent.
4. **Match**: Google accepts and attributes the row.
5. **Use**: the conversion action is primary where bidding should use it, and not double counted with the form fill.

You check each link, quantify the loss at each, and say which to fix first. You do not change the account, the site or the CRM.

## Inputs

1. **Upload summaries** (recommended): `queries.gaql` queries 1 and 2.
2. **Conversion actions** (required): query 3.
3. **CRM export** (required for links 1 and 2): one row per lead with created date and time, source or UTM fields, landing page if stored, click ID fields, status, status date, value.
4. **Upload log or file** (recommended): the last few upload files or the log from whatever sends them (a script, n8n, Zapier, the CRM's own integration, or a CSV export such as the one from the Click ID & Referrer Capture plugin for Contact Form 7).
5. **Paid lead count from another source** (optional): Google Ads form conversions or GA4 key events from paid search for the same period, as a cross-check on capture.

## Checks

Report each check as pass, warn or fail, with the figure.

### Link 1: capture
- **Capture rate**: of CRM leads that came from Google Ads (by UTM, by landing URL, or matched to Google Ads form conversions by time), the share with a gclid, gbraid or wbraid. Pass at 90% or more, warn 75 to 90%, fail below 75%.
- **Common causes to name if it fails**: a redirect or consent banner stripping the query string; auto-tagging off (query 4 checks it); hidden field missing on one form; click ID stored only for the landing page and lost on the second page; cookie blocked before consent with no fallback; iOS traffic arriving with gbraid or wbraid that the form does not capture.

### Link 2: CRM
- Leads with a click ID but no status after the lag window.
- Leads with a status but no value, where values are used for bidding.
- Duplicate leads for the same person that will create duplicate uploads.

### Link 3: upload
- **Freshness**: date of the last upload and the gap between status change and upload. Warn if uploads are weekly or less often; fail if the last upload is older than 7 days.
- **Window**: share of qualified leads whose click is older than the conversion action's click-through window (query 3), which Google will reject. Long sales cycles need the window set to the maximum (90 days) or an earlier-stage conversion.
- **Format**: conversion time includes a time zone (either a `Parameters:TimeZone=` header row or an offset on each time); currency present when values vary; one conversion action name exactly as in the account.
- **Deduplication**: an Order ID (or transaction ID) on every row, so re-sending the same lead does not double count.
- **Consent**: for users in the EEA and UK, the upload should carry Ad User Data and Ad Personalization consent. Check the columns exist and that the share marked denied is roughly in line with the site's consent banner acceptance. A column that is always granted, or always blank, is a warn.

### Link 4: match
- From query 1 and 2: success rate, total vs successful events, and any alerts Google lists, by conversion action. Pass at 90% or more of rows accepted.
- Name the top error reasons if the summaries or the upload log show them.

### Link 5: use
- The offline qualified-lead action is primary if bidding should optimise for it.
- The form submit and the offline action are not both primary for the same goal unless that is intended and understood (it double counts each qualified lead).
- Values are set on upload, not left to a default value of 1.
- Enhanced conversions for leads: note whether it is set up. It matches on hashed email or phone rather than click ID and recovers some of the leads that lost their click ID; it is a complement, not a replacement for fixing capture.

## Output

```
# Offline conversion health: <account>, <period>

**Chain:** <paid leads> leads → <with click ID> captured (<x>%) → <qualified> qualified → <uploaded> uploaded → <accepted> accepted (<y>%)
**Biggest leak:** <link and one sentence>

| Link | Check | Result | Figure | Note |

## Fix first
1. <the single most valuable fix, what it recovers, who does it>
2. <next>
3. <next>

## What this check could not see
<one to three sentences>
```

## Rules

- Do not change conversion actions, upload anything or edit site code.
- Do not include personal data from the CRM in the output. Counts only.
- Say which figures are estimates (for example, the paid lead count when there is no UTM data).
