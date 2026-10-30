# Examples

**Everything in this folder is made up.** "Examplia" (`XX`) and "Otherland" (`YY`) are fictional countries, and every company, number and domain is invented. The files show the shape of the input and output, not real data.

- [list-input.csv](list-input.csv): a few rows of a messy company list, as a data provider might export it
- [list-full.csv](list-full.csv): the same rows after the check, with every `quality_` column
- [list-scorecard.md](list-scorecard.md): a filled-in scorecard for the whole list the rows came from

The Examplia rows were checked against an Examplia market map (path B in step 1). Otherland has no market map, so its row is unverified (path C).

The rows show the common problems: a lost leading zero, a duplicate under a different spelling, a tagline in the name, a company in liquidation, a dormant company, two group companies that aren't duplicates, a free email domain, a row with no ID, and a row from outside the agreed country.
