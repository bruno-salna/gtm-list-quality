# Step 6: Measure the List

## Goal
Measure the rest of what makes a list fit for its purpose: whether rows are in scope, complete and fresh, and whether there are enough of them.

## Why it matters
Steps 3–5 found the broken rows. This step looks at the list as a whole. A list can be clean and still too old, too thin on the fields the purpose needs, or too small to learn anything from.

## How
Measure on unique rows: every row that isn't a duplicate.

### Scope
1. Mark each row `quality_in_scope` against the countries and market agreed in step 1. Rows outside it get `out_of_scope`.
2. Where a market map was used, take scope from its `in_market` column for each matched row. Elsewhere, use the list's own fields, and say so.

### Completeness
1. For each required field agreed in step 1, count how many in-scope, alive rows have it.
2. Add issue codes for the gaps: `missing_domain`, `missing_industry_code`, `missing_size`.

```
Field            Required   Filled   Share
domain           yes        …        …%
industry_code    yes        …        …%
employees        no         …        …%
```

### Freshness
1. Note `source_date` for the list (or for each block, if it was merged from several sources). Note the oldest and newest. This is the freshness of the list as received.
   - An export date isn't the age of the data. A CRM export pulled last week can hold records typed in years ago. Ask when the records were created or last updated; if the source can export a created or modified date per row, use it. If not, agree with the user how old the oldest records may be, and grade on that.
2. Note the freshness of the cleaned file: the oldest source its required fields now come from. Fields filled from a market map in step 4 are as fresh as the map's pull date; fields only the list holds, such as domains, are as old as the list. For a CRM import that needs only a display name taken from the map, that's the map's date; for outreach, the domains keep the list's date.
3. Note `financials_year` for size figures. Mark `stale_financials` where they're older than agreed in step 1.

### Is the list big enough?
A list can be too small for its purpose, and small lists are common in niche segments and small countries.
1. Ask the user how the list will be split: by segment, by message, by channel.
2. For each group, estimate the number of outcomes you'd expect: rows in the group × the expected rate (replies, meetings or whatever the user measures).
3. If a group expects fewer than about 10 outcomes, its results will be mostly noise. Say so, and suggest fewer splits or a combined test.
4. For market sizing or a CRM import, skip this check and say why.

## Pitfalls
- **Measuring on the wrong base.** Shares calculated on all rows, including duplicates and dead companies, look better or worse than they are. State the base for every number.
- **Treating "required" as fixed.** If a required field is missing on most rows, the user may decide it can be added later by enrichment. That's their call. Record it.
- **Ignoring the size check because the list "looks big".** A list of 2,000 split into 4 segments and 2 message versions is 8 groups of 250.

## Done when
Every row has `quality_in_scope` set, completeness and freshness are reported on a stated base, and the size check has a result or a stated reason it was skipped.
