# Tax Type (`taxtype`)
<!-- id: record:taxtype -->

| Fact | Value |
|---|---|
| Internal ID | `taxtype` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `country`, `countrycode` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `server`
- `purpose`: `where_tax_is_tracked_on_balance_sheet`
- `effect`: `country_tax_types_added_with_new_subsidiary`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `country` | text |  | no | no |  | `purpose`: `country` |
| `description` | text | Description | no | no |  | `purpose`: `description` |
| `doesnotaddtototal` | checkbox | Does Not Add to Transaction Total | no | no |  | `purpose`: `does_not_add_to_total`; `value_examples`: `withholding_tax`; `applies_to`: `sales_transactions`, `purchase_transactions`; `shown_only_when`: `suitetax_feature` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `default_value`: `false`; `ui_effect`: `hidden_from_selection_lists`; `shown_only_when`: `suitetax_feature` [`nexus_and_tax_type`] |
| `name` | text | Name | yes | no |  | `purpose`: `record_name`; `shown_only_when`: `suitetax_feature` [`tax_code_records`] |
| `posttoitemcost` | checkbox | Post to Item Cost | no | no |  | `purpose`: `post_to_item_cost`; `applies_to`: `purchase_transactions`; `shown_only_when`: `suitetax_feature` |
| `reversecharge` | checkbox | Reverse Charge | no |  |  | `purpose`: `reverse_charge`; `applies_to`: `purchase_transactions`; `shown_only_when`: `suitetax_feature_disabled` [`tax_code_records`]; `shown_only_when`: `suitetax_feature` [`tax_type_records`] |
| `taxinnetamount` | checkbox | Tax Included in Net Amount | no |  |  | `purpose`: `tax_included_in_price`; `applies_to`: `sales_transactions`, `purchase_transactions`; `shown_only_when`: `suitetax_feature` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `nexusaccounts` (Tax)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `nexus` | select | Nexus | yes |  | `purpose`: `tax_type_nexus`; `shown_only_when`: `suitetax_feature` |
| `payablesaccount` | select | Payables Account | no |  | `purpose`: `tax_payable_account`; `applies_to`: `sales_transactions`; `shown_only_when`: `suitetax_feature` |
| `receivablesaccount` | select | Receivables Account | no |  | `purpose`: `tax_receivable_account`; `applies_to`: `purchase_transactions`; `shown_only_when`: `suitetax_feature` |

## Tabs

| Tab ID | Label |
|---|---|
| `accounts` | Accounts |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `country` | select | Country |  |
| `description` | textarea | Description |  |
| `doesnotaddtototal` | checkbox | Does Not Add to Transaction Total |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `nexus` | select | Nexus |  |
| `payablesaccount` | select | Payables Account |  |
| `posttoitemcost` | checkbox | Post to Item Cost |  |
| `receivablesaccount` | select | Receivables Account |  |
| `reversecharge` | checkbox | Reverse Charge |  |
| `taxinnetamount` | checkbox | Tax Included in Net Amount |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `country` | select | Country |  |
| `description` | textarea | Description |  |
| `doesnotaddtototal` | checkbox | Does Not Add to Transaction Total |  |
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
| `nexus` | select | Nexus |  |
| `payablesaccount` | select | Payables Account |  |
| `posttoitemcost` | checkbox | Post to Item Cost |  |
| `receivablesaccount` | select | Receivables Account |  |
| `reversecharge` | checkbox | Reverse Charge |  |
| `taxinnetamount` | checkbox | Tax Included in Net Amount |  |
