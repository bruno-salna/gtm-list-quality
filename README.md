# List Quality

**An agent-ready guide to cleaning and grading any B2B company list, in any EU country, before you score, enrich or contact anyone on it.**

Every company list has problems you can't see by scrolling: the same company twice under two spellings, firms that closed last year, names in capitals with the legal form still attached, and IDs that lost their leading zeros in a spreadsheet. Each problem looks small. Together they waste enrichment budget, skew your scoring and put dead companies into your outreach.

This guide makes an agent find those problems, fix what can be fixed safely, and give the list an honest grade. You find out whether the list is fit for its purpose before anyone relies on it.

## How to use it
This repo contains no code and no data. It's a set of instructions for an AI agent.

1. Fork or clone this repo.
2. Give it to your agent (Claude, ChatGPT, Cursor or any other) and point it to [AGENT.md](AGENT.md).
3. Hand the agent your company list. The agent asks where the list came from and what it's for, then works through the seven steps with you.

You supply the list and its purpose. The agent works out the naming rules for each country and checks with you at the points that matter.

**Checking companies against official records needs a market map.** This guide doesn't download registers. To confirm that each company exists and is still trading, it matches your list against the output of the [Market map](https://github.com/bruno-salna/gtm-market-map) guide for that country. That also tells you how much of the market your list covers. Without a market map, the list is still cleaned, de-duplicated and graded, and the grade is marked "unverified".

**Any company list works:** the output of the [Market map](https://github.com/bruno-salna/gtm-market-map) guide, a data provider's export, a CRM export, an event attendee list or a spreadsheet someone built by hand. The agent maps its columns onto the [shared schema](schema.md) first. This guide grades companies, not people: one row per company.

## In, middle, out
- **In:** a company list from any source, what it's for (outreach, CRM import or market sizing), and a market map for each country, if you have one.
- **Middle:** the agent cleans names, IDs and domains, and checks every company against the market map: is it alive, is it really one company, is it in your market? Then it flags duplicates and measures what's missing.
- **Out:** one grade for the list, the fixes ranked by how much they lift it, a cleaned list ready for the next guide, and the full list with every check recorded.

## The seven steps
| # | Step | What happens |
|---|---|---|
| 1 | [Take in the list](steps/01-take-in-the-list.md) | Learn where the list came from and what it's for; map its columns |
| 2 | [Learn the naming rules](steps/02-learn-the-naming-rules.md) | Legal forms, ID formats and naming conventions, per country |
| 3 | [Clean the fields](steps/03-clean-the-fields.md) | Fix IDs, domains and countries; turn legal names into names people say |
| 4 | [Confirm identity and status](steps/04-confirm-identity-and-status.md) | Match each row against a market map, check the company is alive, measure coverage |
| 5 | [Find duplicates](steps/05-find-duplicates.md) | Flag the same company appearing twice, without deleting anything |
| 6 | [Measure the list](steps/06-measure-the-list.md) | Completeness, freshness, scope, and whether the list is big enough |
| 7 | [Grade and output](steps/07-grade-and-output.md) | One grade for the list, the fixes ranked by how much they lift it, and the cleaned list |

## Why grade a list at all
- **Bad rows cost money downstream.** Every duplicate or dead company you enrich, score and contact is paid for at each step.
- **A grade forces a decision.** "This list is a C; here are the three fixes that make it a B" is something a team can act on. "The data's a bit messy" isn't.
- **European lists break US tools.** Legal forms in a dozen languages, accented names and national ID formats defeat tools built on English names. This guide is built for them.

## Part of a series
This is the second of five guides that chain together through a [shared row schema](schema.md):

[Market map](https://github.com/bruno-salna/gtm-market-map) → **List quality** → [ICP + tiering](https://github.com/bruno-salna/gtm-icp-tiering) → [Signals](https://github.com/bruno-salna/gtm-signals) → Orchestrator

Each guide works on its own. Used together, each one picks up the previous one's output.

## What's in the repo
```
AGENT.md                     start here (for the agent)
steps/                       one file per step
country-notes-template.md    filled in by the agent for each country in the list
schema.md                    output columns, shared across the series
checklist.md                 definition of done
examples/                    made-up sample input and output
LICENSE                      MIT
```

## Influences
The guide is written from scratch. It draws mainly on my own work cleaning and checking company lists for companies selling into European markets, and it also learned from public work:

- **Eric Nowoslawski, [coldoutboundskills](https://github.com/growthenginenowoslawski/coldoutboundskills)** (MIT).
  - `list-quality-scorecard` grades a list on several dimensions, weights the ones that matter most, and sets a hard "don't use below this grade" gate. The grading in step 7 follows that shape.
  - `playbook-company-name-cleaning` cuts a registered name down to what a person would say out loud. Step 3 takes the same goal across European legal forms and character sets.

Where this guide goes its own way: it checks every company against official records through a market map, so "is this company alive, is it really one company, and what is the list missing?" gets an answer from the source rather than a guess.

## Licence
[MIT](LICENSE). Use it, fork it, adapt it, including commercially; keep the copyright notice.
