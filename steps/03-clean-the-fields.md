# Step 3: Clean the Fields

## Goal
Put identifiers, domains and countries into one consistent format, and give every row a display name a person would actually say.

## Why it matters
Matching against a market map (step 4) and finding duplicates (step 5) only work on clean fields. `0012345678` and `12345678` are the same company; a matching rule won't know that until the ID is repaired. And a name like `SUNFIELD BAKERY LTD` in an email or a report reads like a database talking.

## How
Work on a copy. Whenever you change a value, keep the original in a `src_` column.

### Identifiers
1. Strip spaces, dashes and stray prefixes, following the country notes.
2. Restore leading zeros only where the country's format is fixed-length and the result is unambiguous. Mark the row `id_repaired`.
3. If the format has a check digit, test it. A failed check means `invalid_register_id`; don't try to correct it.
4. IDs that don't fit the format at all, including IDs a spreadsheet turned into scientific notation (`1.23E+09`): `invalid_register_id`, leave the value in `src_`, and clear `register_id`.

Step 4 tries to find every row with a cleared ID by name and city, so an ID that can't be repaired isn't a lost row.

### Domains
1. Reduce to the bare domain: no `https://`, no `www.`, no path, lowercase.
   - If the website field holds an email address (`info@example.com`), take the domain after the `@` and note it in `quality_notes`. That's copying a value the row already has, not guessing.
2. Flag `domain_not_company` for free email providers (global and local ones), social media profiles, directory or marketplace listings, and website builders' default addresses.
3. Don't check whether the site is live yet; that's an optional signal in step 4.

### Country
Use ISO codes (`DE`, `PL` …). Where the country was worked out rather than given, mark `country_inferred`.

### Display name
Build `quality_display_name` from `legal_name`:
1. Remove the legal form, using only the forms listed in the country notes, and only at the start or end of the name.
2. Remove quotation marks that wrap the name.
3. Remove taglines and descriptions after separators such as ` - `, ` | ` or `,` when they clearly aren't part of the name.
4. Fix all-capitals names to normal casing. Keep acronyms in capitals (`DHL`, not `Dhl`) and keep deliberate styling where the company uses it. When you can't tell an acronym from a word, apply one fixed rule without flagging the row: words of up to three letters, or with no vowels, stay in capitals (`ABC`, `TNL Foods`); longer words get normal casing (`Sunfield`).
5. Keep every accent and special character.
6. Set `quality_name_review = true`, with `name_needs_review`, only where a reader would notice a wrong result: you removed or kept a word that may change the name, the name is only a legal form or a generic word, or the registered name holds extra words (a place, a former owner, "firm") and you can't tell which part people use. Casing decided by the rule in 4 isn't a judgment call.

```
Legal name                              Display name          Review
SUNFIELD BAKERY LTD                     Sunfield Bakery       no
"VALLEY CREAMERY" Ltd                   Valley Creamery       no
Ridge Meats Ltd - quality since 2017    Ridge Meats           no
ABC Ltd                                 ABC                   no
"NORTH" Hillside trading firm Ltd       North?                yes
```

If more than about 1 name in 10 is flagged, the flag is too eager. Tighten your rules before step 7, or Names will grade the list on casing instead of on real problems.

## Pitfalls
- **Over-cleaning.** Words like "Group", "Holding" or "International" are often part of how the company is known. Keep them unless the user agrees otherwise; they also help tell group companies apart in step 5.
- **Legal-form words inside names.** Only remove a legal form at the edges of the name, and only forms from that country's notes.
- **Breaking characters.** Never convert `ł` to `l` or `ß` to `ss` in the display name. If a later tool needs plain ASCII, make a separate column for it there.
- **Rewriting what isn't broken.** If a name is already in normal casing, leave the casing alone.

## Done when
IDs, domains and countries are in one format with originals kept, every row has a `quality_display_name`, and you've reported how many rows had each kind of fix and how many names need review.
