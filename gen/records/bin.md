# Bin (`bin`)
<!-- id: record:bin -->

| Fact | Value |
|---|---|
| Internal ID | `bin` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Bin Management` [`real_name`]
- `client_supported`: `false`
- `purpose`: `track_on_hand_quantities_within_warehouse`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`bin_management`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `binnumber` | text | Bin Number | yes | no |  | `purpose`: `bin_number`; `constraint`: `no_ampersand`; `value_examples`: `AA01` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `location` | select | Location | yes | no |  | `purpose`: `bin_location`; `cannot`: `change_after_save`; `allowed_values_listed_in`: `location_records`; `recommendation`: `numbering_scheme_per_location` |
| `memo` | textarea | Memo | no | no |  | `purpose`: `bin_memo`; `not_displayed_on`: `other_pages`, `lists` |
| `sequencenumber` | float | Sequence Number | no |  |  | `purpose`: `bin_sequence_number`; `effect`: `recommended_bin_on_pick_tasks` |
| `type` | select | Type | no |  |  | `purpose`: `bin_type` |
| `zone` | select | Zone | no |  |  | `purpose`: `warehouse_zone`; `use_when`: `pick_strategies` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `binnumber` | text | Bin Number |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `inactive` | checkbox | Inactive |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `binnumber` | text | Bin Number |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `inactive` | checkbox | Inactive |  |
| `internalid` | select | Internal ID |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:bin | isinactive | says_removes_all_references -->
