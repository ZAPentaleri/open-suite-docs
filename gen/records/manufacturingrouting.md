# Manufacturing Routing (`manufacturingrouting`)
<!-- id: record:manufacturingrouting -->

| Fact | Value |
|---|---|
| Internal ID | `manufacturingrouting` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `requires_feature`: `Manufacturing Routing and Work Center` [`real_name`]
- `configured_by`: `enable_features_items_and_inventory_subtab` [`manufacturing_routing_and_work_center`]
- `ui_location`: `lists_supply_chain_manufacturing_routing`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `autocalculatelag` | checkbox | Auto-calculate Lag | no | no |  | `purpose`: `auto_calculate_lag`; `effect`: `optimal_minimal_lag_computed`; `effect`: `lag_values_default_on_work_orders`; `constraint`: `lag_within_optimal_and_maximal_range`; `editable_only_when`: `work_order_planned_or_released`; `cannot`: `edit_operation_lag` (when `auto_calculate_lag`); `allowed_values`: `Quantity` (when `auto_calculate_lag`) [`lag_type`]; `effect`: `lag_change_reschedules_subsequent_tasks` |
| `billofmaterials` | select | Bill of Materials | yes | no |  | `purpose`: `bill_of_materials`; `shown_only_when`: `advanced_bills_of_materials` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `custom_form` |
| `isdefault` | checkbox | Default | no | no |  | `purpose`: `default_routing`; `effect`: `default_for_special_and_mass_created_work_orders` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_routing`; `ui_effect`: `hidden_from_routing_lists` |
| `item` | select | Item | yes | no |  | `purpose`: `routing_item` |
| `location` | select | Location | yes | no |  | `purpose`: `routing_locations`; `value_kind`: `multi_select` |
| `memo` | textarea | Memo | no | no |  | `purpose`: `memo`; `supported_in`: `search`; `value_examples`: `use_beta_alternate_when_primary_widget_supply_is_unavailable` |
| `name` | text | Name | yes | no |  | `purpose`: `routing_name`; `displayed_on`: `manufacturing_routing_selector`; `value_examples`: `beta_alternate_supply_routing` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `routingcomponent` (Component Per Operation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `bomquantity` | text | BOM Quantity | no |  |  |
| `component` | text |  | no |  |  |
| `description` | text | Description | no |  |  |
| `item` | text |  | no |  |  |
| `itemname` | text | Component | no |  |  |
| `operationdisplaytext` | select | Operation | no |  |  |
| `operationsequencenumber` | text | Operation Sequence Number | no |  |  |
| `quantity` | text | Quantity | no |  |  |
| `revision` | text | Revision | no |  |  |
| `units` | text | Units | no |  |  |
| `yield` | percent | Yield | no |  |  |

## Sublist `routingstep` (Routing Steps)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `laborresources` | double | Labor Resources | no |  |  |
| `lagamount` | integer | Lag Amount | no |  |  |
| `lagtype` | select | Lag Type | no |  |  |
| `lagunits` | text | Lag Units | no |  |  |
| `machineresources` | double | Machine Resources | no |  |  |
| `manufacturingcosttemplate` | select | Manufacturing Cost Template | yes |  |  |
| `manufacturingworkcenter` | select | Manufacturing Work Center | yes |  |  |
| `operationname` | text | Operation Name | yes |  |  |
| `operationsequence` | integer | Operation Sequence | yes |  |  |
| `operationyield` | percent | Operation Yield | no |  |  |
| `runrate` | float | Run Rate (Min/Unit) | yes |  |  |
| `setuptime` | float | Setup Time (Min) | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `routingcomponents` | Component Per Operation |
| `routingsteps` | Routing Steps |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `autocalculatelag` | checkbox | Auto-calculate Lag |  |
| `billofmaterials` | select | Bill of Materials |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdefault` | checkbox | Default |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Item |  |
| `lagamount` | float | Lag Amount |  |
| `lagtype` | select | Lag Type |  |
| `lagunits` | text | Lag Units |  |
| `location` | select | Location |  |
| `manufacturingcosttemplate` | select | Manufacturing Cost Template |  |
| `manufacturingworkcenter` | select | Manufacturing Work Center |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `operationname` | text | Operation Name |  |
| `operationyield` | percent | Operation Yield |  |
| `runrate` | float | Run Rate |  |
| `sequence` | integer | Operation Sequence |  |
| `setuptime` | float | Setup Time |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `autocalculatelag` | checkbox | Auto-calculate Lag |  |
| `billofmaterials` | select | Bill of Materials |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isdefault` | checkbox | Default |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Item |  |
| `lagamount` | float | Lag Amount |  |
| `lagtype` | text | Lag Type |  |
| `lagunits` | text | Lag Units |  |
| `location` | select | Location |  |
| `manufacturingcosttemplate` | select | Manufacturing Cost Template |  |
| `manufacturingworkcenter` | select | Manufacturing Work Center |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `operationname` | text | Operation Name |  |
| `operationyield` | percent | Operation Yield |  |
| `runrate` | float | Run Rate |  |
| `sequence` | integer | Operation Sequence |  |
| `setuptime` | float | Setup Time |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `location` | Location | Location |
| `user` | User | Employee |
