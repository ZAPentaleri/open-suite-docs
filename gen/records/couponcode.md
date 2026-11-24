# Coupon Code (`couponcode`)
<!-- id: record:couponcode -->

| Fact | Value |
|---|---|
| Internal ID | `couponcode` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Promotion Codes` [`real_name`]
- `purpose`: `promotion_code_for_revenue_tracking_and_discounts`
- `configured_by`: `enable_features_transactions_subtab` [`promotion_codes`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `code` | text | Coupon Code | yes | no |  | `purpose`: `coupon_code`; `options`: `multiple_use`, `single_use`; `constraint`: `single_use_one_transaction_one_customer` |
| `datesent` | date | Date Sent | no | no |  | `purpose`: `date_sent`; `value_source`: `system_displayed` |
| `promotion` | select | Promotion | yes | no |  | `purpose`: `promotion_of_code`; `value_source`: `system_displayed` |
| `recipient` | select | Recipient | no | no |  | `purpose`: `code_recipient`; `value_source`: `system_displayed`; `caveat`: `recipient_may_differ_from_user` |
| `used` | checkbox | Used | no | no |  | `purpose`: `code_used_flag` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `code` | text | CODE |  |
| `datesent` | date | Date Sent |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `promotion` | select | Promotion |  |
| `recipient` | select | Recipient |  |
| `usecount` | integer | Use Count |  |
| `used` | checkbox | Used |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `code` | text | Code |  |
| `datesent` | date | Date Sent |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `promotion` | text | Promotion |  |
| `recipient` | text | Recipient |  |
| `usecount` | integer | Use Count |  |
| `used` | checkbox | Used |  |
