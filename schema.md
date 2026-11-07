# Shared Row Schema

This is the output format for every repo in the series: Market map → List quality → ICP + tiering → Signals → Orchestrator. Each repo reads and writes rows in this format, which is what lets them chain. Each repo also works on its own.

## Rules
- **File:** CSV, UTF-8, comma-separated, one header row. Keep the original characters in names (ä, ł, ō …); don't strip accents.
- **Column names:** `snake_case`, exactly as listed below.
- **Empty means unknown.** Never fill a field with a guess. A blank field is honest; a wrong value spreads into every later step.
- **One row per legal entity,** identified by `country` + `register_id`. Lists from other sources may not have a `register_id` yet; the List quality repo adds `quality_row_id` so every row can still be referred to.
- **Don't drop columns you don't use.** Later repos may need them.
- **Add only within your prefix.** Each repo owns a prefix for any columns it adds (see the end of this file).

## Columns written by the Market map

### Identity
| Column | Type | Meaning |
|---|---|---|
| `country` | text | ISO 3166-1 alpha-2 code, e.g. `DE`, `PL` |
| `register_id` | text | The company's identifier in the national register. Store it as text so leading zeros survive. |
| `legal_name` | text | Name exactly as registered. Cleaning happens in the List quality repo. |
| `legal_form` | text | Legal form as the register writes it, e.g. `GmbH`, `sp. z o.o.` |
| `status` | text | Status as the register writes it |
| `status_normalised` | text | One of `active`, `inactive`, `in_liquidation`, `unknown` |
| `registered_date` | date | `YYYY-MM-DD` |

### Classification and place
| Column | Type | Meaning |
|---|---|---|
| `industry_code` | text | Primary activity code as registered |
| `industry_code_system` | text | Which classification and version, e.g. `NACE Rev. 2.1` or the national variant |
| `region` | text | First-level region as the register writes it |
| `city` | text | City or municipality |

### Size and web
| Column | Type | Meaning |
|---|---|---|
| `employees` | integer | Exact headcount from filed accounts. Leave empty if only a range is known. |
| `employees_range` | text | Range, if that's all the source gives, e.g. `10-49` |
| `revenue` | number | Annual revenue, in the currency below |
| `revenue_currency` | text | ISO 4217 code, e.g. `EUR` |
| `financials_year` | integer | The year the employee and revenue figures refer to |
| `domain` | text | Bare domain, e.g. `example.com`. No `https://`, no `www.` |
| `domain_confidence` | text | `high`, `medium`, `low`, or empty if no match |

### Market map fields
| Column | Type | Meaning |
|---|---|---|
| `in_market` | boolean | `true` if the row passed every filter |
| `excluded_by` | text | Name of the first filter that removed the row. Empty if `in_market` is `true`. |
| `segment` | text | The segment label from the market definition |
| `source_name` | text | Which source the row came from (the user's own source, named in the country profile) |
| `source_date` | date | When the source data was pulled, `YYYY-MM-DD` |

Lists that didn't come from the Market map can leave `in_market`, `excluded_by` and `segment` empty.

## Columns written by List quality
| Column | Type | Meaning |
|---|---|---|
| `quality_row_id` | text | A stable ID given to every input row at intake, e.g. `r00042`. Never reused. |
| `quality_display_name` | text | The name a person would say: no legal form, no tagline, sensible casing, accents kept |
| `quality_name_review` | boolean | `true` if the display name was a judgment call a person should check |
| `quality_id_check` | text | `confirmed` (the ID was given and found in the market map), `matched` (the ID was found in the market map by name and place), `unmatched`, `invalid` (the ID's format is wrong for the country), `not_checked` (no market map) |
| `quality_status_check` | text | `active_confirmed`, `inactive`, `in_liquidation`, `dormant_suspect`, `not_found`, `not_checked` (no market map) |
| `quality_checked_date` | date | When the row was matched against the market map, `YYYY-MM-DD` |
| `quality_duplicate_of` | text | The `quality_row_id` of the row this one repeats. Empty if it's not a duplicate. |
| `quality_duplicate_confidence` | text | `certain`, `likely` or `possible` (see step 5). Empty if not a duplicate. |
| `quality_in_scope` | boolean | `true` if the row is inside the scope agreed in step 1 (country, market) |
| `quality_issues` | text | Issue codes, separated by `;` (list below) |
| `quality_usable` | boolean | `true` if the row is fit for the stated purpose (see step 7) |
| `quality_notes` | text | Free text for anything a later reader needs to know |

### Issue codes
| Code | Meaning |
|---|---|
| `missing_register_id` | No register ID in the input |
| `invalid_register_id` | The ID's format doesn't fit the country's rules |
| `id_repaired` | The ID was fixed in a safe way (e.g. leading zeros restored); the original is in `src_` |
| `name_needs_review` | The display name is a judgment call |
| `missing_domain` | No website domain |
| `domain_not_company` | The domain is a free email provider, a directory page or a social profile |
| `missing_industry_code` | No activity code |
| `missing_size` | No headcount or revenue |
| `stale_financials` | Size figures are older than agreed in step 1 |
| `country_inferred` | The country wasn't given and was worked out from the ID, domain or address |
| `out_of_scope` | Outside the agreed countries or market |
| `duplicate` | Repeats another row |
| `group_member` | Shares a domain or name stem with another legal entity; not a duplicate |
| `branch_or_foreign_entity` | A branch, or a foreign company's local registration |
| `inactive`, `in_liquidation`, `dormant_suspect`, `not_found` | Mirror the status check |
| `unverified` | No market map for this country; identity and status weren't checked |

## Columns kept from the input
Input columns that don't map to the schema are kept at the end of the file, with the prefix `src_` and their original name in `snake_case` (e.g. `Reg. No.` → `src_reg_no`). The original value of any field that was changed (an ID, a domain) is also kept here. Nothing from the input is lost.

## Columns other repos add
| Repo | Prefix | Examples |
|---|---|---|
| List quality | `quality_` | see above |
| ICP + tiering | `icp_`, `tier` | `icp_score`, `icp_confidence`, `tier`; full list in its [schema](https://github.com/bruno-salna/gtm-icp-tiering/blob/main/schema.md) |
| Signals | `signal_` | `signal_priority`, `signal_top_type`, `signal_why_now`; full list in its [schema](https://github.com/bruno-salna/gtm-signals/blob/main/schema.md) |
| Orchestrator | `run_` | `run_id`, `run_step` |
| (input columns) | `src_` | kept as they came, see above |
