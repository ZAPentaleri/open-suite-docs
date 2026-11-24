# Gift Certificate (`giftcertificate`)
<!-- id: record:giftcertificate -->

| Fact | Value |
|---|---|
| Internal ID | `giftcertificate` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `rule`: `code_active_after_purchase_order_billed`
- `purpose`: `store_credit_sold_as_gift`
- `options`: `own_codes`, `system_random_hash` [`code_generation_preference`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amountremaining` | currency | Amount Remaining | no | no |  | `purpose`: `amount_remaining`; `value_source`: `original_minus_applied_purchases` |
| `email` | email | Recipient Email | no | no |  | `purpose`: `email_address`; `doc_inconsistency`: `gift_certificate_help_describes_employee_email` |
| `expirationdate` | date | Expiration Date | no | no |  | `purpose`: `expiration_date`; `default_behavior`: `from_item_days_before_expiration`; `constraint`: `no_balance_use_after_expiration` |
| `message` | textarea | Gift Message | no | no |  | `purpose`: `recipient_message` |
| `name` | text | Recipient Name | no | no |  | `purpose`: `certificate_name`; `not_displayed_on`: `web_store` |
| `originalamount` | currency | Original Amount | no | no |  | `purpose`: `original_amount`; `effect`: `purchases_deducted_remainder_in_amountremaining` |
| `sender` | text | From | no | no |  | `purpose`: `sender_name` |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `giftcertcode` | text | Code | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amountavailablebilled` | currency | Amount Remaining (Billed) |  |
| `amountremaining` | currency | Amount Remaining |  |
| `createddate` | datetime | Date Created |  |
| `email` | text | To (Email) |  |
| `expirationdate` | date | Expiration Date |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `giftcertcode` | text | Gift Certificate Code |  |
| `incomeaccount` | select | Income Account |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isactive` | checkbox | Active |  |
| `item` | select | Item |  |
| `liabilityaccount` | select | Liability Account |  |
| `message` | text | Gift Message |  |
| `name` | text | To (Name) |  |
| `originalamount` | currency | Gift Certificate Amount |  |
| `purchasedate` | date | Purchase Date |  |
| `sender` | text | From (Name) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amountremaining` | currency | Amount Available |  |
| `amtavailbilled` | currency | Amount Available (Billed) |  |
| `createddate` | datetime | Date Created |  |
| `email` | text | To (Email) |  |
| `expirationdate` | date | Expiration Date |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `gcactive` | checkbox | Active |  |
| `giftcertcode` | text | Gift Certificate Code |  |
| `incomeacct` | text | Income Account |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `liabilityacct` | text | Liability Account |  |
| `message` | text | Gift Message |  |
| `name` | text | To (Name) |  |
| `originalamount` | currency | Amount |  |
| `purchasedate` | date | Purchase Date |  |
| `sender` | text | From (Name) |  |

## Suspected source errors

> [!SOURCE-ERROR] `gift_certificate_help_describes_employee_email` (on `email`)
<!-- source-error: record:giftcertificate | email | gift_certificate_help_describes_employee_email -->
