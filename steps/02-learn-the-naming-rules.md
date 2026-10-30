# Step 2: Learn the Naming Rules

## Goal
For each country in the list, find out how its companies are identified and named, so step 3 can clean IDs and names correctly.

## Why it matters
A rule that's right in one country is wrong in the next. A legal form written after the name in one language is written before it in another. An ID with ten digits in one country has eight and a prefix in another. A tool that only knows "Ltd" and "Inc" will mangle half of Europe's company names.

This step needs no data access. Legal forms, ID formats and naming habits are public knowledge, so you can learn them for any country, including countries where the register itself can't be downloaded.

## How
1. **Start from what exists.** If the user has a market-map country profile for the country, take the legal forms and ID format from it, and fill in only what's missing.
2. **Decide which countries get full notes.** Countries in scope get the full [country-notes-template.md](../country-notes-template.md). Countries with a handful of out-of-scope rows can be noted briefly. Agree this with the user.
3. **Fill in the template** for each country, from official sources first:
   - the register ID: format, length, check digit, and how it's commonly damaged
   - legal forms with their variants, and where they sit in the name
   - naming conventions: capitals, quotation marks, characters to keep
   - words that mark a branch, a foreign entity or a status
4. **Test the rules on the list.** Take a sample of 20–30 rows per country and check that the ID format and legal forms you wrote down match what's in the file. Add any variants you find.

## Illustrative examples
*Examples only, to show the kind of thing to look for. Always confirm for the country in front of you.*
- Legal forms come in many shapes: `GmbH`, `S.A.`, `sp. z o.o.`, `B.V.`, `S.r.l.`, `Oy`, `AB`. Some sit before the name, some after, and some are written out in full in one source and abbreviated in another.
- Some abbreviations are also ordinary words, in the local language or in English, so a careless rule strips them out of the middle of a name.
- Some registers write names in capitals inside quotation marks, so the raw name looks nothing like the brand.
- Register IDs often begin with zeros, and some carry a check digit that lets you spot a typo without any lookup.

## Pitfalls
- **Copying another country's rules.** Neighbouring countries can share a language and still use different legal forms and ID formats.
- **Branches and foreign entities.** Many countries record local branches of foreign companies separately, with their own ID and form. Note how to recognise them.
- **Changed rules.** ID formats and legal forms change after reforms. Older lists may hold formats that are no longer issued but are still valid.

## Done when
Every country in scope has country notes, checked against a sample of the list.
