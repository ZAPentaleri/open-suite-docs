# Nexus (`nexus`)
<!-- id: record:nexus -->

| Fact | Value |
|---|---|
| Internal ID | `nexus` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy` |

## Record facts

- `script_types`: `server`
- `not_supported`: `copy`
- `script_functions`: `create`, `update`, `delete`, `search`
- `requires_feature`: `Advanced Taxes` [`real_name`]
- `not_supported`: `copy`
- `purpose`: `taxable_presence_jurisdiction_usually_country`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `country` | select | Country | yes | no |  | `purpose`: `country` |
| `description` | text | Description | yes | no |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `default_value`: `false`; `ui_effect`: `hidden_from_selection_lists`; `shown_only_when`: `suitetax_feature` [`nexus_and_tax_type`] |
| `parentnexus` | select | Parent Nexus | no | no |  | `purpose`: `parent_nexus`; `applies_when`: `subnexus`; `shown_only_when`: `suitetax_feature` |
| `state` | select | State/Province/County | no | no |  | `purpose`: `state_province_or_county`; `allowed_values_listed_in`: `states_provinces_counties`; `shown_only_when`: `suitetax_feature_disabled` [`tax_code_records`] |
| `taxagency` | select | Tax Agency | no | no |  | `purpose`: `tax_agency`; `allowed_values_listed_in`: `vendors_with_tax_agency_category`; `ui_effect`: `new_option_creates_record`; `shown_only_when`: `suitetax_feature_disabled` [`tax_code_records`] |
| `taxdatefromfulfillment` | checkbox | Use Fulfillment to Modify Tax Point Date | no | no |  | `purpose`: `tax_date_from_fulfillment`; `true_means`: `fulfillment_date_is_tax_point`; `effect`: `overrides_transaction_date_default`; `use_when`: `jurisdiction_requires_fulfillment_date` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `country` | select | Country |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `parentnexus` | select | Parent Nexus |  |
| `state` | select | State |  |
| `taxagency` | select | Tax Agency |  |
| `taxdatefromfulfillment` | checkbox | Use Fulfillment to Modify Tax Point Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `country` | select | Country |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `parentnexus` | select | Parent Nexus |  |
| `state` | select | State |  |
| `taxagency` | select | Tax Agency |  |
| `taxdatefromfulfillment` | checkbox | Use Fulfillment to Modify Tax Point Date |  |
