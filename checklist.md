# Definition of Done

Every item passes, or the scorecard explains why it doesn't.

## Intake
- [ ] The original file is saved in `input/`, unedited
- [ ] Purpose preset, scope and required fields are written down and signed off by the user
- [ ] For outreach: the campaign plan (splits, expected rate) is written down
- [ ] Every country has a step 4 path (A, B or C), and every market map used is named with its date
- [ ] Every input row has a unique `quality_row_id`
- [ ] Every input column is either mapped to the schema or kept with the `src_` prefix

## Naming rules
- [ ] Every country in scope has country notes, with every answer linked

## Rows
- [ ] `list-full.csv` holds every input row, and no more
- [ ] `list-clean.csv` holds exactly the rows with `quality_usable = true`
- [ ] Both files follow [schema.md](schema.md), with no renamed or missing columns
- [ ] Every changed value has its original kept in a `src_` column
- [ ] Every row has `quality_display_name`, `quality_id_check`, `quality_status_check` and `quality_in_scope` set
- [ ] Every duplicate points to a surviving row that isn't itself a duplicate
- [ ] No two rows with different register IDs are marked as duplicates of each other
- [ ] No row left unmatched with several candidates is linked as a duplicate
- [ ] No duplicate whose ID came from a name match is marked `certain`
- [ ] Every domain taken from an email address has a note; no free-mail, social, directory or site-builder domain is used as a company domain
- [ ] No more than about 1 name in 10 is flagged for review, or the scorecard says why
- [ ] Every row from a country without a market map is marked `not_checked` and `unverified`
- [ ] No field was filled with a guess
- [ ] Names with accents or non-Latin characters display correctly

## Scorecard
- [ ] The grade as received, the cleaned-file grade with its row count, the preset and the verdict are in the first lines, with "unverified" in the grade if any rows weren't checked
- [ ] Top fixes come straight after the grade, ranked by grade lift, each with rows affected, its effect, and done / to do
- [ ] Every dimension shows its measure and the base it was calculated on, under "How the grade was worked out"
- [ ] The row funnel adds up: rows in − removed = usable, at every stage
- [ ] Coverage of the market is reported for every country matched against a market map
- [ ] The size check has a result, or a reason it was skipped
- [ ] Freshness states the age of the oldest records (not just the export date), for the list as received and for the cleaned file
- [ ] The unverified share in the grade counts in-scope rows only
- [ ] The preset, any changes to it, and every judgment call agreed with the user are recorded under "Decisions made"
- [ ] The user's decision on using the list is recorded
