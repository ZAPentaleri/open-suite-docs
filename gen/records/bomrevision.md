# Bill of Materials Revision (`bomrevision`)
<!-- id: record:bomrevision -->

| Fact | Value |
|---|---|
| Internal ID | `bomrevision` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `server`
- `requires_feature`: `Advanced BOM` [`real_name`]
- `script_functions`: `read`, `create`, `edit`, `copy`, `delete`, `search`
- `purpose`: `bom_changes_over_product_lifecycle_with_history`
- `use_cases`: `compare_and_track_cost_savings_across_revisions`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`advanced_bom`]
- `ui_location`: `lists_supply_chain_bill_of_materials`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `createddate` | date | Date Created | no | no |  | `purpose`: `date_created`; `value_source`: `system_recorded` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_bom_or_revision`; `ui_effect`: `hidden_from_lists_on_records_and_forms` |
| `memo` | text | Memo | no | no |  | `purpose`: `bom_memo` |
| `name` | text | Name | yes | no |  | `purpose`: `bom_or_revision_name`; `constraint`: `unique`; `value_examples`: `Grill BOM`, `BOM Revision 1` |
| `billofmaterial` | select | Bill of Materials | no | no |  |  |
| `effectivedate` | date | Effective Start Date | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `obsoletedate` | date | Effective End Date | no | no |  |  |

## Sublist `bomrevisioncomponent` (Components)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `bomquantity` | posfloat | BOM Quantity | no |  |  |
| `componentyield` | percent | Component Yield | no |  |  |
| `description` | textarea | Description | no |  |  |
| `internalid` | integer | ID | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsource` | select | Item Source | no |  |  |
| `itemsourcelist` | text | Item Source options list | no |  |  |
| `linenumber` | integer | Sequence | no |  |  |
| `quantity` | posfloat | Quantity | yes |  |  |
| `unit` | select | Units | no |  |  |

## Sublist `systemnotes` (System Notes)

## Tabs

| Tab ID | Label |
|---|---|
| `components` | Components |
| `history` | System Notes |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billofmaterial` | select | Bill of Materials |  |
| `createddate` | date | Created Date |  |
| `effectivedate` | date | Effective Start Date |  |
| `externalid` | text | External ID |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `obsoletedate` | date | Effective End Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billofmaterial` | select | Bill of Materials |  |
| `createddate` | date | Created Date |  |
| `effectivedate` | date | Effective Start Date |  |
| `externalid` | text | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | integer | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `obsoletedate` | date | Effective End Date |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billofmaterial` | Bill of Material | Bill of Materials |
| `component` | Component | Bill of Materials Revision Component |
