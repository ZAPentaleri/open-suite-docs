# Tax Group (`taxgroup`)
<!-- id: record:taxgroup -->

| Fact | Value |
|---|---|
| Internal ID | `taxgroup` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `nexuscountry`, `countrycode` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_types`: `client`, `server`
- `purpose`: `combine_tax_codes_across_jurisdictions`
- `value_examples`: `us_state_city_and_transit_tax`, `canada_tax_group`
- `benefit`: `apply_one_group_instead_of_several_codes`
- `ui_location`: `setup_accounting_tax_groups`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `city` | text | City | no | no |  | `purpose`: `tax_city`; `effect`: `used_for_automatic_tax_determination`; `shown_only_when`: `suitetax_feature_disabled` |
| `county` | text | County | no | no |  | `purpose`: `tax_county`; `effect`: `used_for_automatic_tax_determination`; `shown_only_when`: `suitetax_feature_disabled` |
| `description` | textarea | Description | no | no |  | `purpose`: `description` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `apply_to_child_subsidiaries`; `shown_only_when`: `suitetax_feature_disabled`; `caveat`: `colon_separates_parent_from_child` |
| `isdefault` | checkbox | Default Code | no | no |  | `purpose`: `default_tax_group`; `effect`: `used_when_ship_address_matches_no_group`; `shown_only_when`: `suitetax_feature_disabled` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `default_value`: `false`; `ui_effect`: `hidden_from_selection_lists`; `shown_only_when`: `suitetax_feature` [`nexus_and_tax_type`] |
| `itemid` | text | Tax Name | yes | no |  | `purpose`: `record_name`; `shown_only_when`: `suitetax_feature_disabled` |
| `piggyback` | checkbox | Piggy Back | no | no |  | `purpose`: `piggyback_gst`; `true_means`: `pst_calculated_on_amount_plus_gst` |
| `rate` | percent | Rate | no | no |  | `purpose`: `tax_rate_percent`; `value_examples`: `8%`; `shown_only_when`: `suitetax_feature_disabled` |
| `state` | select | State | no | no |  | `purpose`: `state_province_or_county`; `allowed_values_listed_in`: `states_provinces_counties`; `shown_only_when`: `suitetax_feature_disabled` [`tax_code_records`] |
| `subsidiary` | select | Subsidiaries | no | no |  | `purpose`: `subsidiaries`; `requires_feature`: `OneWorld` [`real_name`]; `shown_only_when`: `suitetax_feature_disabled` |
| `taxitem1` | select | GST/HST | yes | no |  | `purpose`: `gst_item` |
| `taxitem2` | select | PST | yes | no |  | `purpose`: `pst_item` |
| `taxtype` | select | Tax Type | yes | no |  | `purpose`: `tax_type`; `cannot`: `change_after_save` (when `suitetax_feature`) |
| `unitprice1` | rate | Rate | no | no |  | `purpose`: `gst_rate`; `value_source`: `system_displayed` |
| `unitprice2` | rate | Rate | no | no |  | `purpose`: `pst_rate`; `value_source`: `system_displayed` |
| `zip` | textarea | Zip Codes | no | no |  | `purpose`: `tax_zip_codes`; `format`: `comma_separated`; `effect`: `used_for_automatic_tax_determination`; `shown_only_when`: `suitetax_feature_disabled` |
| `externalid` | text | ExternalId | no | no |  |  |
| `itemtype` | text |  | no | no |  |  |
| `nexuscountry` | text |  | no | no |  |  |

## Sublist `taxitem` (Tax Codes)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `rate` | percent | Rate | no |  | `purpose`: `tax_rate_percent`; `value_examples`: `8%` |
| `taxtype` | text | Type | no |  | `purpose`: `tax_type`; `default_behavior`: `type_tax_control_account` |
| `basis` | posfloat | Basis | no |  |  |
| `city` | text |  | no |  |  |
| `county` | text |  | no |  |  |
| `state` | text |  | no |  |  |
| `taxitemnkey` | text |  | no |  |  |
| `taxname` | select | Name | yes |  |  |
| `taxname2` | text |  | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `city` | text | City |  |
| `country` | select | Country |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `itemid` | text | Name |  |
| `piggyback` | checkbox | Piggy Back |  |
| `rate` | percent | Rate |  |
| `state` | select | State |  |
| `subsidiary` | select | Subsidiary |  |
| `taxitem1` | select | GST/HST |  |
| `taxitem2` | select | PST |  |
| `taxtype` | select | Tax Type |  |
| `unitprice1` | percent | GST/HST Rate |  |
| `unitprice2` | percent | PST Rate |  |
| `zip` | textarea | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `city` | text | City |  |
| `country` | select | Country |  |
| `county` | text | County |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `itemid` | text | Name |  |
| `piggyback` | checkbox | Piggy Back |  |
| `rate` | currency | Rate |  |
| `state` | text | State |  |
| `statedisplayname` | select | State Display Name |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `taxitem1` | select | GST/HST |  |
| `taxitem2` | select | PST |  |
| `taxtype` | select | Tax Type |  |
| `unitprice1` | percent | GST/HST Rate |  |
| `unitprice2` | percent | PST Rate |  |
| `zip` | textarea | Zip |  |
