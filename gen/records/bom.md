# Bill of Materials (`bom`)
<!-- id: record:bom -->

| Fact | Value |
|---|---|
| Internal ID | `bom` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Advanced BOM` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `edit`, `copy`, `delete`, `search`
- `replaces`: `assembly_bill_of_materials`
- `purpose`: `materials_and_quantities_to_manufacture_product`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`advanced_bom`]
- `ui_location`: `lists_supply_chain_bill_of_materials`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `availableforallassemblies` | checkbox | Available For All Assemblies | no | no |  |  | `purpose`: `available_for_all_assemblies`; `false_means`: `only_restricttoassemblies` |
| `availableforalllocations` | checkbox | Available For All Locations | no | no |  |  | `purpose`: `available_for_all_locations`; `false_means`: `only_restricttolocations` |
| `createddate` | date | Date Created | no | no |  |  | `purpose`: `date_created`; `value_source`: `system_recorded` |
| `includechildren` | checkbox | Include Children | no | no |  |  | `purpose`: `share_bom_with_child_subsidiaries`; `doc_inconsistency`: `help_also_describes_usedonassembly` |
| `isinactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_bom_or_revision`; `ui_effect`: `hidden_from_lists_on_records_and_forms` |
| `memo` | text | Memo | no | no |  |  | `purpose`: `bom_memo` |
| `name` | text | Name | yes | no |  |  | `purpose`: `bom_or_revision_name`; `constraint`: `unique`; `value_examples`: `Grill BOM`, `BOM Revision 1` |
| `restricttoassemblies` | select | Assembly | no | no |  |  | `purpose`: `assemblies_allowed_to_use_bom`; `value_kind`: `multi_select`; `applies_when`: `availableforallassemblies_cleared` |
| `restricttolocations` | select | Locations | no | no |  |  | `purpose`: `locations_allowed_to_use_bom`; `value_kind`: `multi_select`; `applies_when`: `availableforalllocations_cleared` |
| `subsidiary` | select | Subsidiaries | no | no |  |  | `purpose`: `bom_subsidiaries`; `requires_feature`: `OneWorld` [`real_name`]; `value_kind`: `multi_select`; `effect`: `inherited_by_bom_revisions` |
| `usecomponentyield` | checkbox | Use Component Yield | no | no |  |  | `purpose`: `use_component_yield`; `effect`: `applies_to_all_bom_revisions`; `value_kind`: `percent_of_component_surviving_manufacture` |
| `usedonassembly` | checkbox | Used On Assembly | no | no | yes |  | `purpose`: `bom_used_on_assembly` |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `bomrevisions` | Revisions |
| `history` | System Notes |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `availableforallassemblies` | checkbox | Available For All Assemblies |  |
| `availableforalllocations` | checkbox | Available For All Locations |  |
| `createddate` | date | Created Date |  |
| `externalid` | text | ExternalId |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `includechildren` | checkbox | Include Children |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `restricttoassemblies` | multiselect | Restrict To Assemblies |  |
| `restricttolocations` | multiselect | Restrict To Locations |  |
| `subsidiary` | multiselect | Subsidiary |  |
| `usecomponentyield` | checkbox | Use Component Yield |  |
| `usedonassembly` | checkbox | Used on Assembly |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `availableforallassemblies` | checkbox | Available For All Assemblies |  |
| `availableforalllocations` | checkbox | Available For All Locations |  |
| `createddate` | date | Created Date |  |
| `externalid` | text | ExternalId |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `includechildren` | checkbox | Include Children |  |
| `internalid` | integer | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `restricttoassemblies` | multiselect | Restrict To Assemblies |  |
| `restricttolocations` | multiselect | Restrict To Locations |  |
| `revisionname` | text | Revision : Name |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `usecomponentyield` | checkbox | Use Component Yield |  |
| `usedonassembly` | checkbox | Used on Assembly |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `assemblyitem` | Assembly Item | Assembly Item Bill of Materials |
| `revision` | Revision | Bill of Materials Revision |

## Suspected source errors

> [!SOURCE-ERROR] `help_also_describes_usedonassembly` (on `includechildren`)
<!-- source-error: record:bom | includechildren | help_also_describes_usedonassembly -->
