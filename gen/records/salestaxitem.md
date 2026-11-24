# Sales Tax Item (`salestaxitem`)
<!-- id: record:salestaxitem -->

| Fact | Value |
|---|---|
| Internal ID | `salestaxitem` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `city` | text | City | no | no |  | `purpose`: `tax_city`; `effect`: `used_for_automatic_tax_determination`; `shown_only_when`: `suitetax_feature_disabled` |
| `county` | text | County | no | no |  | `purpose`: `tax_county`; `effect`: `used_for_automatic_tax_determination`; `shown_only_when`: `suitetax_feature_disabled` |
| `description` | textarea | Description | no | no |  | `purpose`: `description` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `tax_code_display_name`; `shown_only_when`: `suitetax_feature_disabled` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `apply_to_child_subsidiaries`; `shown_only_when`: `suitetax_feature_disabled`; `caveat`: `colon_separates_parent_from_child` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `default_value`: `false`; `ui_effect`: `hidden_from_selection_lists`; `shown_only_when`: `suitetax_feature` [`nexus_and_tax_type`] |
| `itemid` | text | Tax Name | yes | no |  | `purpose`: `record_name`; `shown_only_when`: `suitetax_feature_disabled` |
| `name` | text | Name | yes | no |  | `purpose`: `record_name`; `shown_only_when`: `suitetax_feature` [`tax_code_records`] |
| `rate` | rate | Rate | yes | no |  | `purpose`: `tax_rate_percent`; `value_examples`: `8%`; `shown_only_when`: `suitetax_feature_disabled` |
| `state` | select | State | no | no |  | `purpose`: `state_province_or_county`; `allowed_values_listed_in`: `states_provinces_counties`; `shown_only_when`: `suitetax_feature_disabled` [`tax_code_records`] |
| `subsidiary` | select | Subsidiaries | no | no |  | `purpose`: `subsidiaries`; `requires_feature`: `OneWorld` [`real_name`]; `shown_only_when`: `suitetax_feature_disabled` |
| `taxaccount` | select | Tax Account | yes | no |  | `purpose`: `tax_control_account`; `shown_only_when`: `suitetax_feature_disabled` |
| `taxagency` | select | Tax Agency | yes | no |  | `purpose`: `tax_agency`; `allowed_values_listed_in`: `vendors_with_tax_agency_category`; `ui_effect`: `new_option_creates_record`; `shown_only_when`: `suitetax_feature_disabled` [`tax_code_records`] |
| `taxtype` | select | Tax Type | no | no |  | `purpose`: `tax_type`; `cannot`: `change_after_save` (when `suitetax_feature`) |
| `zip` | textarea | Zip Codes | no | no |  | `purpose`: `tax_zip_codes`; `format`: `comma_separated`; `effect`: `used_for_automatic_tax_determination`; `shown_only_when`: `suitetax_feature_disabled` |
| `externalid` | text | ExternalId | no | no |  |  |
| `itemtype` | text |  | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `appliestoservice` | checkbox | Applies To Service |  |
| `availableon` | select | Available On |  |
| `country` | select | Country |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `iseccode` | checkbox | EC Code |  |
| `isexcludetaxreports` | checkbox | Excluded From Tax Reports |  |
| `isexport` | checkbox | Export |  |
| `isinactive` | checkbox | Inactive |  |
| `isreversecharge` | checkbox | Reverse Charge |  |
| `itemid` | text | Name |  |
| `name` | text | Name |  |
| `parent` | select | Parent |  |
| `taxtype` | select | Tax Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `appliestoservice` | checkbox | Applies To Service |  |
| `availableon` | select | Available On |  |
| `country` | select | Country |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `iseccode` | checkbox | EC Code |  |
| `isexcludetaxreports` | checkbox | Is Excluded From Tax Reports |  |
| `isexport` | checkbox | Export |  |
| `isinactive` | checkbox | Inactive |  |
| `isreversecharge` | checkbox | Reverse Charge |  |
| `itemid` | text | Name |  |
| `name` | text | Name |  |
| `parent` | select | Parent |  |
| `rate` | currency | Rate |  |
| `taxtype` | select | Tax Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `taxtype` | Tax Type | Tax Type |
