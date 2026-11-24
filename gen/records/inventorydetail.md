# Inventory Detail (`inventorydetail`)
<!-- id: record:inventorydetail -->

| Fact | Value |
|---|---|
| Internal ID | `inventorydetail` |
| Category | subrecord |
| Client scripts | yes |
| Subrecord | yes |
| Tags | `access_via_parent_record` |

## Record facts

- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Advanced Bin Numbered Inventory Management` [`real_name`]
- `is_subrecord`: `true`
- `script_types`: `client`
- `server_access`: `through_parent_record`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `scriptable_from`: `body_field`, `line_item`
- `purpose`: `bin_and_serial_lot_quantities_for_items_and_lines`
- `ui_effect`: `opens_as_popup_from_inventory_detail_field`
- `doc_inconsistency`: `records_browser_omits_receiptinventorynumber` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `item` | select | Item | no | no |  | `purpose`: `target_items_for_attribute_copy`; `value_kind`: `multi_select`; `doc_inconsistency`: `inventory_detail_help_describes_attribute_copy`; `caveat`: `extraneous_fields_copied_but_unused` |
| `itemdescription` | text | Description | no | no |  | `purpose`: `item_description` |
| `location` | select | Location | no | no |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`); `ui_effect`: `new_option_creates_record` |
| `quantity` | float | Quantity | no | no |  | `purpose`: `quantity_to_build`; `constraint`: `not_above_buildable_quantity` |
| `unit` | select | Units | no | no |  | `purpose`: `units`; `value_source`: `assembly_base_units` (when `units_of_measure`) |
| `externalid` | text | ExternalId | no | no |  |  |
| `tolocation` | text | Location | no | no |  |  |

## Sublist `inventoryassignment` (Inventory Detail)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `binnumber` | select | Bin | yes |  |  |
| `expirationdate` | date | Expiration Date | no |  |  |
| `internalid` | integer |  | no |  |  |
| `inventorydetail` | integer |  | no |  |  |
| `inventorystatus` | select | Status | yes |  |  |
| `issueinventorynumber` | select | Serial/Lot Number | yes |  |  |
| `lotquantityavailable` | float | Lot Qty Available Across Bins | no |  |  |
| `packcarton` | text | Pack Carton | no |  |  |
| `pickcarton` | text | Pick Carton | no |  |  |
| `quantity` | float | Quantity | yes |  |  |
| `quantityavailable` | posfloat | Available | no |  |  |
| `quantitystaged` | posfloat | Quantity Staged | no |  |  |
| `tobinnumber` | select | Bin | yes |  |  |
| `toinventorystatus` | select | Status | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `binnumber` | select | Bin Number |  |
| `expirationdate` | date | Expiration Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `inventorynumber` | select | Number |  |
| `packcarton` | text | Pack Carton |  |
| `pickcarton` | text | Pick Carton |  |
| `quantity` | float | Quantity |  |
| `quantitystaged` | float | Staged Quantity |  |
| `status` | select | Status |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `binnumber` | select | Bin Number |  |
| `expirationdate` | date | Expiration Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `inventorynumber` | select | Number |  |
| `packcarton` | text | Pack Carton |  |
| `pickcarton` | text | Pick Carton |  |
| `quantity` | float | Quantity |  |
| `status` | select | Status |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `inventorynumber` | Inventory Number | Inventory Numbers |

## Suspected source errors

> [!SOURCE-ERROR] `records_browser_omits_receiptinventorynumber`
<!-- source-error: record:inventorydetail | - | records_browser_omits_receiptinventorynumber -->

> [!SOURCE-ERROR] `inventory_detail_help_describes_attribute_copy` (on `item`)
<!-- source-error: record:inventorydetail | item | inventory_detail_help_describes_attribute_copy -->
