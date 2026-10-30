# Step 7: Grade and Output

## Goal
Give the list one letter grade for its purpose, rank the fixes by how much they lift it, and deliver the cleaned list, the full list and a one-page scorecard. The grade and the top fixes are the headline. The grade for each dimension is kept at the bottom of the scorecard for anyone who wants to check the maths.

## Why it matters
A grade turns many small findings into one decision:

| Grade | Meaning |
|---|---|
| **A** | Use the list as it is |
| **B** | Usable after small fixes |
| **C** | Fix it before using it |
| **D** | Don't use it; get a better source |

The fixes say where the effort goes.

## How
### The dimensions
Score each dimension as a share of unique rows, unless stated otherwise.

| Dimension      | Measures                                                     | A         | B         | C          | D                |
| -------------- | ------------------------------------------------------------ | --------- | --------- | ---------- | ---------------- |
| **Alive**      | share `active_confirmed` (checked rows only)                 | ≥95%      | ≥90%      | ≥80%       | below            |
| **Unique**     | share of all rows that are duplicates                        | ≤1%       | ≤3%       | ≤8%        | above            |
| **Identified** | share with a `confirmed` or `matched` ID (checked rows only) | ≥95%      | ≥85%      | ≥70%       | below            |
| **Names**      | share with no name review needed                             | ≥98%      | ≥95%      | ≥90%       | below            |
| **In scope**   | share `quality_in_scope`                                     | ≥98%      | ≥95%      | ≥85%       | below            |
| **Complete**   | share of in-scope, alive rows with every required field      | ≥90%      | ≥75%      | ≥50%       | below            |
| **Fresh**      | age of the source data (see step 6: the list's date as received, the oldest source of the required fields for the cleaned file) | ≤3 months | ≤6 months | ≤12 months | older or unknown |

### The purpose presets
The user picked a preset in step 1. It sets the required fields and how much each dimension counts. These presets are this guide's working baseline. The user can change a weight, a band or a required field, but every change is recorded under "Decisions made".

| | Outreach | CRM import | Market sizing |
|---|---|---|---|
| Required fields | display name, domain | display name | none beyond identity |
| Alive | ×2 | ×1 | ×2 |
| Unique | ×2 | ×3, stricter bands: A 0%, B ≤0.5%, C ≤2% | ×2 |
| Identified | ×1 | ×2 | ×2 |
| Names | ×1 | ×1 | not graded |
| In scope | ×1 | ×1 | ×2 |
| Complete | ×1 | ×1 | not graded |
| Fresh | ×1 | ×1 | ×1, bands doubled (A ≤6 months … D older than 24) |

Why they differ: outreach is hurt most by dead companies and duplicates. A CRM import is hurt most by duplicates, which are hard to undo once records exist, and by rows with no firm identity. Market sizing needs identity and scope right, and doesn't care about websites or display names.

### Mark each row usable
`quality_usable = true` when all of these hold:
- not a duplicate (or it's the surviving row)
- `quality_in_scope` is `true`
- `quality_status_check` is `active_confirmed`, or `not_checked` on a path C country
- it has every required field from the preset

### Combine into one grade
1. Give each graded dimension points: A = 4, B = 3, C = 2, D = 1.
2. Take the weighted average, dividing by the sum of the weights used.
3. Grade: **A** ≥ 3.5, **B** ≥ 2.75, **C** ≥ 2.0, **D** below.
4. **Gate:** if Alive or Unique is a D, the list can't be graded above C, whatever the average.
5. **Use rule:** by default, a list below B isn't used until the top fixes are done. Agree the rule with the user.

### Unverified lists
Rows from path C countries weren't checked against official records, so Alive and Identified can't be measured for them. Count the unverified share on in-scope rows only: unverified rows that are out of scope anyway don't affect the label, so mention them under Caveats instead.
- **Every row is path C:** leave Alive and Identified out of the grade, and label it, e.g. **"B (unverified)"**. Don't mark the list down for a closed register. Say plainly that the grade covers form, not existence.
- **Some rows are path C:** measure Alive and Identified on the checked rows only, and label the grade with the share not checked, e.g. **"B (12% of rows unverified)"**.
- **More than half the rows are path C:** treat the whole list as unverified.

### Rank the fixes
A fix is anything that raises a dimension or gives back usable rows. Each fix is either **done**, meaning already applied in `list-clean.csv` (dropping duplicates and dead, out-of-scope, unmatched or incomplete rows), or **to do**, meaning it needs work or a new source (finding missing domains, pulling a fresh export, adding companies from the market map).

1. **Work out the grade after each fix.** Re-score the dimensions the fix touches as if it were done, then recombine. Count fixes cumulatively in ranked order, so each "grade after" includes every fix above it.
2. **Rank done fixes first, then to-do fixes.** Within each group, rank by grade lift (weighted points gained), then by rows affected.
3. **Fixes that give rows back come last.** Some fixes don't move the grade but recover usable rows or coverage of the market: finding domains for rows set aside, or adding the in-market companies the list misses. Rank them last among the to-do fixes, by rows gained, and show the rows gained instead of a grade change.

The grade after the last done fix is the **cleaned-file grade**: what `list-clean.csv` is worth as it stands. Report it in the first lines, next to the grade as received and the number of usable rows.

### Outputs
- **`list-clean.csv`:** usable rows only, following [../schema.md](../schema.md), sorted by `country`, then `quality_display_name`. This is the file the next guide picks up.
- **`list-full.csv`:** every input row, with all `quality_` columns and the `src_` columns. Sorted by `quality_row_id`.
- **`list-scorecard.md`:** in this structure:

```
# List scorecard: <list name>

Grade: <letter as received> <(unverified) if any> for <preset>. Cleaned file: <letter>, <n> usable rows.
<one-sentence verdict: what to do next>
Source: <source>, pulled <date>. Checked against: <market maps and dates, or "no market map">.

## Top fixes
<ranked table: fix — rows — effect (dimension and list grade after, or rows gained) — done or to do>

## What happened to the rows
<funnel: rows in → duplicates → out of scope → dead or dormant → not found → missing required field → usable>

## Coverage of the market
<path B countries only: in-market companies in the map — on the list — coverage % — missing>

## Is the list big enough?
<groups, expected outcomes, verdict>

## Decisions made
- <the preset, any changes to it, and each judgment call agreed with the user>

## Caveats
- <unverified countries, limits of the check>

## How the grade was worked out
<table: dimension — measure — base — score — grade — weight>
<weighted points → grade; whether the gate applied; share of rows unverified>
```

See [../examples/](../examples/) for a made-up filled-in version.

## Checkpoint
Show the user the scorecard. They decide whether to use the list, fix it first, or drop it. Record the decision in the scorecard.

## Pitfalls
- **Grading only the cleaned file.** The headline grade describes the list as it came in. The cleaned-file grade sits next to it, always with its row count: removing rows always lifts the grade, so a cleaned A on a list half the size isn't the same as an A as received.
- **Hiding unknowns.** An unverified grade must say so in its first line, not in a caveat at the bottom.
- **One grade for a mixed list.** If a list merges very different sources, grade each source block as well, so the user knows which source to stop using.

## Done when
All three files exist, the user has seen the scorecard and decided, and [../checklist.md](../checklist.md) passes.
