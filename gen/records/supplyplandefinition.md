# Supply Plan Definition (`supplyplandefinition`)
<!-- id: record:supplyplandefinition -->

| Fact | Value |
|---|---|
| Internal ID | `supplyplandefinition` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `sublist_ids`: `SCOPE_FS_TAB`
- `record_actions`: `launch`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`, `copy`
- `not_supported`: `transform`
- `purpose`: `criteria_and_results_holder_for_supply_planning`
- `value_examples`: `planning_horizon`, `planning_scope`, `multiple_facilities`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`material_requirements_planning`]
- `doc_inconsistency`: `page_calls_record_supply_change_order`
- `record_actions`: `launch:run_supply_planning`
- `action_effect`: `planned_orders_generated`
- `returns_detail`: `notifications`, `response_key_value_pairs`
- `parameters`: `record_id_only`
- `displayed_on`: `bulk_processing_page` (when `launch_errors`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | no |  |  |  |
| `description` | textarea | Description | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `generatepegging` | checkbox | Generate Pegging | no |  |  |  |
| `horizonindays` | integer | Horizon (In Days) | yes |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `itemgroup` | select | Planning Item Group | no |  |  |  |
| `name` | text | Name | yes |  |  |  |
| `rulegroup` | select | Planning Rule Group | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Sublist `scope`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `consumeforecast` | checkbox | Consume Forecast | no |  |  |
| `demandsourcetype` | select | Demand Source Type | yes |  |  |
| `id` | integer | Internal ID | no |  |  |
| `location` | select | Location | yes |  |  |
| `plandefinition` | select | Plan Definition id | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `generatepegging` | checkbox | Generate Pegging |  |
| `horizonindays` | integer | Horizon In Days |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `itemgroup` | select | Planning Item Group |  |
| `name` | text | Name |  |
| `rulegroup` | select | Planning Rule Group |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `generatepegging` | checkbox | Generate Pegging |  |
| `horizonindays` | integer | Horizon In Days |  |
| `internalid` | select | Internal ID |  |
| `itemgroup` | select | Planning Item Group |  |
| `name` | text | Name |  |
| `rulegroup` | select | Planning Rule Group |  |

## Suspected source errors

> [!SOURCE-ERROR] `page_calls_record_supply_change_order`
<!-- source-error: record:supplyplandefinition | - | page_calls_record_supply_change_order -->
