# Competitor (`competitor`)
<!-- id: record:competitor -->

| Fact | Value |
|---|---|
| Internal ID | `competitor` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `purpose`: `track_competitors_affecting_sales`
- `configured_by`: `opportunities_feature` [`competitor_on_opportunity`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `competitor_description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_competitor`; `ui_effect`: `hidden_on_sales_transactions` |
| `name` | text | Name | yes | no |  | `purpose`: `competitor_name`; `displayed_on`: `opportunity_competitors_sublist` |
| `productservice` | textarea | Products & Services | no | no |  | `purpose`: `competitor_products_services` |
| `strategy` | textarea | Strategy | no | no |  | `purpose`: `strategy_against_competitor`; `displayed_on`: `this_record_only` |
| `strengths` | textarea | Strengths | no | no |  | `purpose`: `competitor_strengths`; `displayed_on`: `this_record_only` |
| `url` | url | URL | no | no |  | `purpose`: `competitor_website_url` |
| `weaknesses` | textarea | Weaknesses | no | no |  | `purpose`: `competitor_weaknesses`; `displayed_on`: `this_record_only` |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `oppnotes` | textarea | Opportunity Notes |  |
| `productservices` | textarea | Products/Services |  |
| `strategy` | textarea | Strategy |  |
| `strengths` | textarea | Strengths |  |
| `url` | url | Web Address |  |
| `weaknesses` | textarea | Weaknesses |  |
| `wonopportunity` | checkbox | Won Opportunity |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `oppnotes` | textarea | Opportunity Notes |  |
| `opportunity` | select | Opportunity |  |
| `oppwinloss` | checkbox | Winner |  |
| `productservices` | richtext | Products/Services |  |
| `strategy` | richtext | Strategy |  |
| `strengths` | richtext | Strengths |  |
| `url` | url | Web Address |  |
| `weaknesses` | richtext | Weaknesses |  |
