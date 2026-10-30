# Agent Instructions: Clean and Grade a Company List

You are checking a **company list**: one row per company, from any source. Your job is to find what's wrong with it, fix what can be fixed safely, flag what can't, and give the list an honest grade for the purpose the user has in mind. You are not enriching the list or finding people. Those come later.

Read this file first, then each step file in `steps/` in order. Work through them with your user.

## Principles
1. **Never delete a row.** Duplicates, dead companies and out-of-scope rows stay in the full file with the reason. The user must be able to see and question every decision.
2. **Never guess.** If you can't confirm a value, leave it empty or mark it unchecked. An honest gap is better than a confident error.
3. **The official record wins.** Where a market map can confirm an ID, a name or a status, use it over anything a data provider or a website says.
4. **Don't fetch registers.** Checking against official records goes through a market map from the [Market map](https://github.com/bruno-salna/gtm-market-map) guide. If there isn't one, suggest it; if the user declines, mark the rows unverified. Don't build a partial map here.
5. **Grade for a purpose.** A list that's fine for sizing a market can be unfit for outreach. Ask what the list is for before you judge it.
6. **Show your counts.** Every step reports numbers: rows checked, rows changed or flagged, and why.
7. **Keep the original untouched.** Work on a copy. The input file is the evidence of what you were given.
8. **Never ask for passwords or logins.** If a source needs an account, ask the user to download the data themselves and hand you the file.

## What to ask your user at the start
- Where did the list come from, and when was it pulled or last updated?
- What will it be used for? Outreach, CRM import or market sizing: this picks the grading preset in step 7.
- Did the list come from a market map, or do you have a market map for its countries? (A market map is the output of the [Market map](https://github.com/bruno-salna/gtm-market-map) guide; see step 1.)
- Is there a defined scope? (Countries, and a market definition if there is one.)
- What do you have access to: paid data sources, tools, budget?
- Are there columns you already know are wrong or unreliable?

## Steps
| # | File | Output |
|---|---|---|
| 1 | [steps/01-take-in-the-list.md](steps/01-take-in-the-list.md) | A column mapping, a purpose preset and a step 4 path per country, signed off by the user |
| 2 | [steps/02-learn-the-naming-rules.md](steps/02-learn-the-naming-rules.md) | A filled-in [country-notes-template.md](country-notes-template.md) per country |
| 3 | [steps/03-clean-the-fields.md](steps/03-clean-the-fields.md) | Normalised IDs, domains and countries, and a display name per row |
| 4 | [steps/04-confirm-identity-and-status.md](steps/04-confirm-identity-and-status.md) | Each row matched against a market map, with its status, plus coverage; or marked unverified |
| 5 | [steps/05-find-duplicates.md](steps/05-find-duplicates.md) | Duplicates flagged and linked to the row they repeat |
| 6 | [steps/06-measure-the-list.md](steps/06-measure-the-list.md) | Scores for completeness, freshness and scope, plus a size check |
| 7 | [steps/07-grade-and-output.md](steps/07-grade-and-output.md) | `list-clean.csv`, `list-full.csv` and `list-scorecard.md` |

**Checkpoints:** stop and confirm with the user after steps 1, 5 and 7. Step 1 fixes what "good" means. Step 5 is where wrong merges happen. Step 7 is where the user decides whether to use the list.

## Working files
Keep each list in its own folder, named for the list (for example `trade-fair-2026/`):
- `input/`: the file exactly as you received it
- `maps/`: the market-map files the user supplies for step 4
- `scripts/`: any code you write, so the check can be rerun on a fresh copy of the list
- the step outputs: column mapping, country notes, the final files

Company lists often come from paid sources, and market-map files can be large. Keep `input/` and `maps/` out of version control.

## When you're done
Go through [checklist.md](checklist.md). Every item must pass, or the scorecard must say why it doesn't.

## Output format
All rows follow [schema.md](schema.md). Don't rename or remove its columns.
