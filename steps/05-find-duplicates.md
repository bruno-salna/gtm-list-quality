# Step 5: Find Duplicates

## Goal
Find every company that appears more than once, link each repeat to one surviving row, and leave related but separate companies apart.

## Why it matters
Duplicates mean paying twice for enrichment, contacting the same company twice, and counting your market wrongly. But a wrong merge is worse than a missed duplicate: it hides a real company behind another one. That's why this step separates certain matches from possible ones, and why the user checks the uncertain ones.

## How
1. **Compare rows in this order,** from strongest evidence to weakest:

| Evidence | Confidence |
|---|---|
| Same `country` and `register_id`, both given in the list or repaired in step 3 | `certain` |
| Same `country` and `register_id`, where either ID was found by a name match in step 4 (`matched`) | `likely` |
| Same VAT number, or another official ID kept in `src_` | `certain` |
| Same domain and the same display name | `likely` |
| Same display name and the same city, no conflicting ID | `likely` |
| Similar display name (a small spelling difference, a word order change) and the same city or domain | `possible` |

2. **Never call rows with different register IDs duplicates.** Two legal entities that share a name stem or a website are a group, a parent and a subsidiary, or a local branch. Mark both `group_member`, and leave them as separate rows.
   - The same goes for a row step 4 left unmatched because it had several candidates. Don't link it as a duplicate of any of them: that would pick a company step 4 refused to pick. It stays a separate, unmatched row, with the candidates in `quality_notes`.
3. **Pick the surviving row** in each set: a confirmed ID first, then the most complete row, then the newest source. Every other row in the set gets `quality_duplicate_of` set to the survivor's `quality_row_id`, `quality_duplicate_confidence`, and the issue code `duplicate`.
4. **Fill the survivor's gaps only from `certain` matches,** and say so in `quality_notes` (e.g. "domain from r00187").
5. **`certain` and `likely` duplicates are flagged automatically.** `possible` duplicates go to the user as a short list to confirm or reject before anything is flagged.

## Checkpoint
Before step 6, show the user:
- the counts of `certain`, `likely` and `possible` duplicates
- a sample of 10 `likely` pairs, so they can see the rule works
- every `possible` pair (or, if there are many, the rule you'd apply and a sample)
- the dormancy rule from step 4 and how many rows it affects
- coverage of the market, for countries matched against a market map

Record the user's decisions.

## Pitfalls
- **Group companies.** A group can have a holding company, an operating company and a property company with nearly the same name and one website. They aren't duplicates, and which one to target is a decision for the ICP + tiering guide.
- **Shared domains on platforms.** Rows whose "domain" is a marketplace or directory share it with thousands of others. These were flagged `domain_not_company` in step 3; never match on them.
- **Generic names.** Names made of common words ("Central Logistics", "Nordic Foods") match many unrelated companies. Require a second field to agree.
- **Duplicates across countries.** The same brand registered in two countries is two legal entities, not a duplicate.

## Done when
Every duplicate is linked to a survivor with a confidence level, group members are marked, the user has decided the `possible` pairs, and the counts are reported.
