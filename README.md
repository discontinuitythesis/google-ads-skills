# Google Ads skills for Claude

Four Claude skills for running a lead generation or ecommerce Google Ads account the way I run them: the machine bids, an agent watches, a person decides.

Built by [Ben Luong](https://firepixel.co.uk/about) at [Fire Pixel](https://firepixel.co.uk). MIT licensed. Take them, change them, tell me where they are wrong.

| Skill | What it does | When to run it |
|---|---|---|
| [search-term-review](skills/search-term-review/SKILL.md) | Finds wasted spend in the search terms report without cutting terms whose qualified leads have not come back from the CRM yet | Weekly or fortnightly |
| [weekly-exception-note](skills/weekly-exception-note/SKILL.md) | Writes the Monday note: what moved outside its normal range, why, and what needs a decision | Every Monday |
| [target-box-check](skills/target-box-check/SKILL.md) | Works out whether budget or target is the real constraint on each campaign, and whether the number in the target box came from anywhere | Monthly, and before any target change |
| [offline-conversion-health](skills/offline-conversion-health/SKILL.md) | Checks the chain from form fill to click ID to CRM to upload to matched conversion | Weekly, and after any site or CRM change |

## The rule all four follow

None of them change the account. Each one ends with a list of proposed changes for a person to approve, and a note of what it could not see.

That is deliberate. An agent that applies negatives from seven days of "zero conversions" will remove the searches that produce your best leads, because in lead generation the qualified lead arrives through an offline upload weeks after the click. Google Ads reports that conversion against the click date, so the last few weeks always look worse than they are. Every skill here knows about that lag and says how it handled it.

## Data in

The skills do not need a connector. Each one lists the GAQL queries it wants in a `queries.gaql` file beside the SKILL.md. Run them however you already pull data (the Google Ads API, a script, a warehouse, n8n, or Ads Editor and report exports) and give Claude the results as CSV. If you do use a Google Ads connector, the skill will use the same queries through it.

Where a skill needs the CRM (lead date, click ID, status, value), it says which columns and works without them at lower confidence.

## Validation

On 29 September 2026, all 79 referenced field names were checked against the Google Ads API v25 field metadata service. This confirms that the fields exist, rather than proving that every query combination runs against a customer account. No customer account data was accessed. Run the queries against your own account before relying on the results; the example outputs use fictional data.

## Install

Claude apps: Settings, Capabilities, Skills, upload the skill folder as a zip. Claude Code: copy the folder into `~/.claude/skills/` (personal) or `.claude/skills/` in a project.

Install one skill per job. If you already have a search term skill or an audit skill that overlaps, remove one of them; with two competing skills Claude can pick either, and you lose the consistency a skill is for.

## Each skill folder

```
skills/<name>/
  SKILL.md            instructions, checks, output format
  queries.gaql        the data it wants
  example-output.md   what a run looks like (fictional account)
```

## Related

- [The weekly loop](https://firepixel.co.uk/advanced-google-ads/the-weekly-loop): the process these skills sit inside
- [There is no right number for the target box](https://firepixel.co.uk/advanced-google-ads/no-right-number-for-the-target-box): the reasoning behind target-box-check
- [CRM feedback loop](https://firepixel.co.uk/crm-feedback-loop): why offline conversions change what "zero conversions" means
- [Budget / Target Constraint Checker](https://firepixel.co.uk/tools/target-box-checker): the same check as a free web tool
- [Click ID Checker](https://firepixel.co.uk/chrome-click-id-checker) (Chrome) and [Click ID & Referrer Capture for CF7](https://firepixel.co.uk/wordpress-click-id-capture) (WordPress): the capture end of the chain offline-conversion-health checks

Issues and pull requests welcome.
