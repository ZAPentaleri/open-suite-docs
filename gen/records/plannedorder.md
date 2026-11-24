# Planned Order (`plannedorder`)
<!-- id: record:plannedorder -->

| Fact | Value |
|---|---|
| Internal ID | `plannedorder` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `required_init_params`: `supplyplandefinition`, `supplyplanningrun`, `firmed`
- `rule`: `firmed_must_be_true`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `purpose`: `supply_planning_order`
- `value_source`: `created_by_supply_planning_or_workbench`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`material_requirements_planning`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | no |  |  |  |
| `enddate` | date | End Date | yes |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `firmed` | checkbox | Firmed | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `item` | select | Item | no |  |  |  |
| `itemlocation` | integer | Item Location | no |  |  |  |
| `itemtype` | select | Item Type | no |  |  |  |
| `location` | select | Location | yes |  |  |  |
| `memo` | text | Memo | no |  |  |  |
| `plannedorderstatus` | select | Status | no |  |  |  |
| `quantity` | bigdecimal | Quantity | yes |  |  |  |
| `released` | checkbox | Released | no |  |  |  |
| `sourceitemlocation` | integer | Source Location Item | no |  |  |  |
| `sourcelocation` | select | Source Location | no |  |  |  |
| `startdate` | date | Start Date | yes |  |  |  |
| `supplyplandefinition` | select | Supply Plan Definition | no |  |  |  |
| `supplyplanningrunid` | integer |  | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |
| `transactioncreated` | select | Transaction Created | no |  |  |  |
| `transactionlinecreated` | text | Transaction Line Created | no |  |  |  |
| `transactiontype` | select | Transaction Type | yes |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `datecreated` | datetime | Date Created |  |
| `datemodified` | datetime | Date Modified |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isfirmed` | checkbox | Is Firmed |  |
| `isreleased` | checkbox | Is Released |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `plannedorderstatus` | select | Status |  |
| `quantity` | float | Quantity |  |
| `sourcelocation` | select | Source Location |  |
| `startdate` | date | Start Date |  |
| `supplyplandefinition` | select | Supply Plan Definition |  |
| `supplyplanningrun` | integer | Supply Planning Run |  |
| `transactioncreated` | select | Transaction Created |  |
| `trantype` | select | Transaction Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `datecreated` | datetime | Date Created |  |
| `datemodified` | datetime | Date Modified |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isfirmed` | checkbox | Is Firmed |  |
| `isreleased` | checkbox | Is Released |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `plannedorderstatus` | select | Status |  |
| `quantity` | float | Quantity |  |
| `sourcelocation` | select | Source Location |  |
| `startdate` | date | Start Date |  |
| `supplyplandefinition` | select | Supply Plan Definition |  |
| `supplyplanningrun` | integer | Supply Planning Run |  |
| `transactioncreated` | select | Transaction Created |  |
| `trantype` | select | Transaction Type |  |
