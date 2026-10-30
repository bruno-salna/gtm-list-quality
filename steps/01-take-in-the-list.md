# Step 1: Take in the List

## Goal
Understand where the list came from and what it's for, map its columns onto the schema, find out which countries have a market map to check against, and agree with the user what "good" means for this list.

## Why it matters
Quality only means something against a purpose. A list for market sizing can live without websites. A list for outreach can't. A list for a CRM import must have no duplicates at all. If you skip this step, you'll grade the list against your own idea of good, and the grade won't help anyone.

## What "market map" means in this guide
A **market map** is the output of the [Market map](https://github.com/bruno-salna/gtm-market-map) guide, the first guide in this series. Run for one country, it produces:
- `market-map-full.csv`: every company in that country's official register, with its register ID, legal name, status and (where published) size, in the [shared schema](../schema.md)
- `market-map.csv`: the companies inside the user's market
- a country profile: the register, its access and licence, legal forms and statuses

This guide never fetches registers itself. Identity and status are checked in step 4 by matching the list against `market-map-full.csv`. That's why you need to know now which countries have one.

## How
1. **Save the original.** Put the file in `input/` exactly as received. Never edit it.
2. **Ask the questions** in [../AGENT.md](../AGENT.md): source, date, purpose, required fields, scope, access.
3. **Read the file before trusting it.** Check the encoding (broken accents such as `Ã¤` mean the file was saved in the wrong encoding somewhere), the delimiter, the header row, and whether any rows are blank, totals or notes.
4. **Give every row a `quality_row_id`** in input order (`r00001`, `r00002` …). Every later step refers to rows by this ID.
5. **Map the columns** onto [../schema.md](../schema.md):

```
Input column      →  Schema column      Notes
Company           →  legal_name         includes legal form, some in capitals
Reg. No.          →  register_id        stored as a number: leading zeros lost?
Website           →  domain             full URLs, needs cleaning in step 3
Employees         →  src_employees      provider estimates, not filed accounts
Industry          →  src_industry       free text, not a code
```

   - Map only where the meaning matches. A column of free-text industry labels is not `industry_code`; keep it as `src_industry`.
   - Every column that doesn't map is kept with the `src_` prefix.
   - If a column's meaning is unclear, ask the user rather than guessing.
6. **Count rows per country,** and for each country find out which path step 4 will take:

| Situation | Step 4 path |
|---|---|
| The list *is* a market map, or was built from one | **A:** identity already confirmed; re-check status only if the map is old |
| The list came from elsewhere, and the user has a market map for that country | **B:** match the list against `market-map-full.csv` |
| No market map yet | Suggest running the [Market map](https://github.com/bruno-salna/gtm-market-map) guide first. Its step 2 finds out whether a full map is possible in that country. If it is and the user runs it, take path **B**. |
| No full map is possible (the Market map guide ends with a sizing map), or the user declines | **C:** identity and status aren't checked; the grade is marked "unverified" |

   Path C is a proper result, not a failure. Running the Market map guide is a real piece of work, and the user decides whether it's worth it.
7. **Agree the scope:** which countries, and which market. If a market map is used, its market definition is the scope. Rows outside it will be marked `quality_in_scope = false`, not removed.
8. **Agree the purpose preset** from step 7 (outreach, CRM import or market sizing). It sets the required fields and how strictly each dimension is graded. For outreach, also ask now how the campaign will be split (segments, message versions, channels) and what rate of replies or meetings the user expects. Step 6 needs both for the size check; don't assume them.
9. Write it all down and get the user's sign-off.

## Worksheet
```
List name:
Source and date pulled:
Purpose preset: outreach / CRM import / market sizing
Campaign plan (outreach only): splits, expected reply or meeting rate
Rows received:
Column mapping: (table above)
Rows per country, and step 4 path per country (A / B / C):
Market maps used (country, file, date pulled):
Scope (countries, market definition):
Required fields (from the preset, plus any changes):
Changes to the preset, and why:
Known problems the user mentioned:
Signed off by user: yes / no, date
```

## Pitfalls
- **Spreadsheets damage data silently.** IDs saved as numbers lose leading zeros, long IDs turn into scientific notation (`1.23E+09`), and dates swap day and month. Look for these before mapping.
- **"Country" columns lie.** A data provider's country can be the headquarters, the billing address or the place of the website's server. Note which one it seems to be.
- **One list, several sources.** Lists merged from several exports often have different conventions in different blocks of rows. Look at the top, middle and end of the file, not just the first rows.
- **Grading against the wrong purpose.** If the user isn't sure what the list is for, use the strictest likely preset and say so.

## Done when
The original is saved, every row has a `quality_row_id`, the column mapping is written down, every country has a step 4 path, and the user has signed off on purpose, scope and required fields.
