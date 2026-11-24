# Assembly Build (`assemblybuild`)
<!-- id: record:assemblybuild -->

| Fact | Value |
|---|---|
| Internal ID | `assemblybuild` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `assemblyitem`, `assemblyitem_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billofmaterials` | select | Bill of Materials | no | no |  | `purpose`: `bill_of_materials` |
| `billofmaterialsrevision` | select | Bill of Materials Revision | no | no |  | `purpose`: `bom_revision`; `default_behavior`: `revision_effective_on_transaction_date`; `effect`: `revision_effective_date_sets_membership` |
| `buildable` | float | Buildable Quantity | no | no |  | `purpose`: `buildable_quantity`; `value_source`: `max_buildable_for_assembly`; `value_scope`: `selected_location` (when `locations_used`) |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `inventorydetail` | summary | Inventory Detail | no | no |  | `purpose`: `inventory_detail_subrecord`; `purpose`: `assign_bins` |
| `item` | select | Assembly | yes | no |  | `purpose`: `assembly_item_to_build`; `constraint`: `assembly_items_only` |
| `location` | select | Location | yes | no |  | `purpose`: `build_location`; `effect`: `components_taken_and_assemblies_added_here`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `quantity` | posfloat | Quantity to Build | yes | no |  | `purpose`: `quantity_to_build`; `constraint`: `not_above_buildable_quantity` |
| `revision` | select | Revision | no | no |  | `purpose`: `assembly_revision`; `effect`: `revision_effective_date_sets_membership` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary` |
| `total` | currency | Projected Value | no | no |  | `purpose`: `projected_value`; `value_source`: `parts_value_times_quantity` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `reference_number`; `default_behavior`: `system_generated`; `can`: `override` |
| `units` | select | Units | no | no |  | `purpose`: `units`; `value_source`: `assembly_base_units` (when `units_of_measure`) |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `revisionmemo` | textarea | Revision Memo | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `component` (Components)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `componentinventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `quantity` | float | Quantity | yes |  |  |
| `quantityonhand` | float | Quantity On Hand | no |  |  |
| `unitcost` | float |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `components` | Components |
| `custom` | Custom |
| `history` | History |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `assemblyunbuild` | `customform` |
