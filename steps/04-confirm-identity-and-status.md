# Step 4: Confirm Identity and Status

## Goal
Match every row to a company in the market map, find out whether that company is still trading, and measure how much of the market the list covers.

## Why it matters
Most lists contain companies that no longer exist: liquidated, struck off, or alive on paper but not trading. Data providers update slowly, and a list someone built a year ago is older than it looks. Matching against the official record is the only way to know, and it gives rows without an ID a firm identity, which makes step 5 far more reliable.

This guide doesn't fetch registers. That's the [Market map](https://github.com/bruno-salna/gtm-market-map) guide's job. Here you match against the `market-map-full.csv` the user supplies, following the path agreed for each country in step 1.

## How
### Path A: the list came from a market map
1. Identity is already confirmed: set `quality_id_check = confirmed` and take `status_normalised` from the map.
2. Check the age of the map. If it's older than the preset allows (see step 7), suggest the user reruns the Market map guide for a fresh copy, rather than checking rows one by one.
3. Apply the dormancy rule below.

### Path B: the list came from elsewhere, and there's a market map
Match against `market-map-full.csv`, not `market-map.csv`. The full file holds every registered company, including inactive ones and those outside the market, so a liquidated company on the list is found and marked dead instead of "not found".
1. **Rows with a valid ID:** look it up in the map. Found → `quality_id_check = confirmed`. Not found → treat the row as one without an ID (step 2 below); if that finds nothing either, set `unmatched` and `quality_status_check = not_found`.
2. **Rows without a usable ID:** no ID, an ID cleared in step 3 as `invalid_register_id` (failed check digit, scientific notation), or a valid ID that isn't in the map. Search the map by name and place.
   - Accept a match only when the display name and at least one more field (city, legal form, domain) agree, and there's only one candidate. Set `matched` and fill `register_id`.
   - Where the damaged ID still holds information (the first digits of a scientific-notation ID), the match must agree with it too.
   - Two or more candidates, or a weak match: leave `register_id` empty, set `unmatched` and `not_found`, and note the candidates in `quality_notes`.
3. **Fill gaps from the map, never overwrite silently.** Where the map has a legal name, legal form, status, industry code or size the list lacks, fill it in. Where they disagree with the list, keep the map's value and the list's value in `src_`.
4. **Measure coverage.** Count how many in-market companies in the map (`in_market = true`) appear in the list, as unique, alive rows. That share is the list's coverage of the market. Report it per segment if the map has segments, and report how many in-market companies the list is missing.

### Path C: no market map
1. Set `quality_id_check` and `quality_status_check` to `not_checked`, and add the issue code `unverified`.
2. Note why in `quality_notes`: no full map is possible in that country, or the user chose not to build one.
3. Don't try to confirm rows another way. A half-check looks like a check and isn't one.

### Status (paths A and B)
Set `quality_status_check` from the map's `status_normalised`:
- `active_confirmed`: active, and no sign of dormancy
- `inactive` or `in_liquidation`: as the map says
- `dormant_suspect`: active on paper, but matching the dormancy rule
- `not_found`: no match in the map

**Dormancy rule.** A register can show "active" for years after trading stops. Use what the map holds: a company that must file accounts but whose latest `financials_year` is two or more years old, or whose filed accounts show no revenue and no staff, is a `dormant_suspect`. Agree the rule with the user, apply it the same way to every row, and record it.

Set `quality_checked_date` on every matched row, and mark rows `branch_or_foreign_entity` where the map shows it.

Report the result per country:

```
                    Rows
active_confirmed    …
inactive            …
in_liquidation      …
dormant_suspect     …
not_found           …
not_checked         …   (path C)
```

## Pitfalls
- **Renamed companies.** A company found by ID but with a different name has usually been renamed, or the ID is wrong. Note it, and match on the ID only if the city or legal form also agrees.
- **Look-alike matches.** Common names produce many candidates. One weak match is worse than no match, because it attaches the wrong company's data to the row.
- **Re-registered businesses.** A business can close one legal entity and carry on under a new one with a similar name. The old row is dead; the new entity may not be on the list at all. Note it if you see it.
- **Coverage on the wrong base.** Coverage counts in-market companies from the map that the list holds. Companies on the list but outside the market don't raise it.

## Done when
Every row has `quality_id_check` and `quality_status_check` set, the dormancy rule is written down, status counts are reported per country, and coverage is reported for every path B country.
