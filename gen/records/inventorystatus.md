# Inventory Status (`inventorystatus`)
<!-- id: record:inventorystatus -->

| Fact | Value |
|---|---|
| Internal ID | `inventorystatus` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `description` | textarea | Description | no | no |  | `purpose`: `item_description`; `displayed_on`: `sales_forms_description_column` |
| `inventoryavailable` | checkbox | Make Inventory Available | no | no |  | `purpose`: `status_counts_as_available`; `true_means`: `inventory_available_for_orders`; `false_means`: `excluded_from_available_count` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `name` | text | Name | yes | no |  | `purpose`: `name`; `doc_inconsistency`: `help_says_item_name` |
| `externalid` | text | ExternalId | no | no |  |  |
| `invavailforallocandplanning` | checkbox | Make Inventory Available For Allocation And Planning | no | no |  |  |
| `state` | text | State | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal Id |  |
| `internalidnumber` | integer | Internal Id (Number) |  |
| `invavailforallocandplanning` | checkbox | Make Inventory Available For Allocation and Planning |  |
| `inventoryavailable` | checkbox | Make Inventory Available |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `state` | select | State |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `invavailforallocandplanning` | checkbox | Make Inventory Available For Allocation and Planning |  |
| `inventoryavailable` | checkbox | Make Inventory Available |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `state` | text | State |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_says_item_name` (on `name`)
<!-- source-error: record:inventorystatus | name | help_says_item_name -->
