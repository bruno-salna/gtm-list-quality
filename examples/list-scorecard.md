# List scorecard: Food producers, data provider export

*Made-up example. Examplia and Otherland are fictional countries, and every number is invented.*

**Grade: C (2% of rows unverified) for outreach. Cleaned file: A, 1,612 usable rows.**
Use the cleaned file for a first test now. The biggest gains left are rows, not grade: add the 1,253 in-market companies the list misses, and find the 285 missing domains.
Source: data provider export, pulled 2026-01-20. Checked against: Examplia market map (register pulled 2026-10-15), matched on 2026-10-20. Otherland: no market map.

## Top fixes
| # | Fix | Rows | Effect | Status |
|---|---|---|---|---|
| 1 | Drop dead and dormant companies | 166 | Alive C → A; list C → **B** | Done in `list-clean.csv` |
| 2 | Remove duplicates | 105 | Unique C → A; list stays B | Done in `list-clean.csv` |
| 3 | Drop rows outside the market | 117 | In scope C → A; list stays B | Done in `list-clean.csv` |
| 4 | Set aside rows with no company domain | 285 | Complete B → A; list B → **A** | Done in `list-clean.csv` |
| 5 | Set aside rows not found in the map | 115 | Identified B → A; list stays A | Done in `list-clean.csv` |
| 6 | Pull a fresh export: the source is 9 months old | all | Fresh C → A; list stays A | To do |
| 7 | Review the flagged display names | 60 | Names B → A; list stays A | To do |
| 8 | Add the in-market companies the list misses, from the market map | 1,253 | Up to +1,253 rows; coverage 60% → up to 100% | To do |
| 9 | Find domains for the rows set aside in fix 4 | 285 | Up to +285 usable rows | To do |
| 10 | Resolve rows not found in the map (ask the provider for IDs, or check by hand) | 115 | Up to +115 usable rows | To do |

Fixes 1–5 lift the grade by removing rows, which is why the cleaned file reaches A with 1,612 of 2,400 rows. Fixes 8–10 give rows back.

## What happened to the rows
```
Stage                      Rows in   Removed   Rows out
Duplicates                 2,400     105       2,295
Out of scope               2,295     117       2,178
Dead or dormant            2,178     166       2,012
Not found in the map       2,012     115       1,897
Missing required field     1,897     285       1,612
Usable                                         1,612
```
- Duplicates: 61 certain, 35 likely, 9 possible (confirmed by the user)
- Out of scope: 46 rows from Otherland, 71 outside the market in the Examplia map
- Dead or dormant: 41 inactive, 37 in liquidation, 88 dormant
- Not found: 63 with an ID that isn't in the map, 52 with no ID and no city to match on
- Missing required field: 285 with no company domain

## Coverage of the market
Examplia market map: food producers, 3,150 in-market companies.

| Segment | In the map | On the list | Coverage | Missing |
|---|---|---|---|---|
| Bakery | 1,320 | 812 | 61.5% | 508 |
| Dairy | 760 | 455 | 59.9% | 305 |
| Meat processing | 1,070 | 630 | 58.9% | 440 |
| **Total** | **3,150** | **1,897** | **60.2%** | **1,253** |

The provider's list holds about 60% of the market. The other 1,253 companies are in the market map and can be added from it.

## Is the list big enough?
Planned use: an outreach test, 3 segments × 2 message versions, expecting about 3% replies.

| Segment | Usable rows | Per version | Expected replies per version |
|---|---|---|---|
| Bakery | 694 | 347 | about 10 |
| Dairy | 388 | 194 | about 6 |
| Meat processing | 530 | 265 | about 8 |

Verdict: too small to compare message versions within each segment. Test the two versions across all segments combined (806 rows each, about 24 replies each), or use one version per segment. Adding the missing companies from fix 8 would change this.

## Decisions made
- Preset: outreach, unchanged
- Dormant: active in the register, but no accounts filed for two or more years
- 22 possible duplicate pairs reviewed: 9 confirmed, 13 kept as separate companies
- 14 group companies sharing a domain kept as separate rows
- Otherland: no market map, rows unverified; out of scope anyway
- Size figures up to two years old are accepted

## Caveats
- The headline grade describes the list as received. `list-clean.csv` already has fixes 1–5 applied.
- Status is as of the market map's pull date. Companies change status every week.

**User's decision (2026-10-21):** use `list-clean.csv` for a first test, with the two message versions compared across all segments combined. Meanwhile, add the missing companies from the market map (fix 8) and find the 285 domains (fix 9) before the full run.

## How the grade was worked out
Preset: outreach.

| Dimension | Measure | Base | Score | Grade | Weight |
|---|---|---|---|---|---|
| Alive | active confirmed | 2,249 unique Examplia rows | 87.5% | C | ×2 |
| Unique | duplicates | 2,400 rows in | 4.4% | C | ×2 |
| Identified | ID confirmed or matched | 2,249 unique Examplia rows | 94.9% | B | ×1 |
| Names | no review needed | 2,295 unique rows | 97.4% | B | ×1 |
| In scope | inside agreed scope | 2,295 unique rows | 94.9% | C | ×1 |
| Complete | domain present | 1,897 in-scope, alive rows | 85.0% | B | ×1 |
| Fresh | age of source data | | 9 months | C | ×1 |

As received: 21 / 9 = 2.33 → **C**. No gate applied. 46 of 2,295 unique rows (2%) are from Otherland and weren't checked.
Cleaned file (fixes 1–5): 33 / 9 = 3.67 → **A**.
