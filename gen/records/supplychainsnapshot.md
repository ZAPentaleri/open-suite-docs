# Supply Chain Snapshot (`supplychainsnapshot`)
<!-- id: record:supplychainsnapshot -->

| Fact | Value |
|---|---|
| Internal ID | `supplychainsnapshot` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`
- `script_functions`: `create`, `read`, `edit`, `delete`, `search`
- `requires_feature`: `Multi-Location Inventory`, `Advanced Inventory Management`, `Demand Planning`, `Supply Chain Control Tower` [`real_name`]
- `script_functions`: `create`, `read`, `edit`, `delete`, `search`
- `not_supported`: `copy`
- `based_on`: `saved_searches`
- `record_actions`: `refresh`
- `purpose`: `simulate_supply_and_demand_against_planned_levels`
- `read_only_fields`: `replenishmentmethod`, `daterun`, `futurehorizon`, `futurehorizondate`, `stockunit`
- `editable_only_when`: `creating` [`item`]
- `allowed_values`: `7`, `14`, `30`, `45`, `60` [`pasthorizon_days`]
- `editable_only_when`: `create_or_refresh_true` [`pasthorizon`]
- `allowed_range`: `0`, `365` days [`futurehorizon`]
- `caveat`: `refresh_not_persisted_and_unsettable_on_create`
- `options`: `supplychainsnapshot:header`, `supplychainsnapshotdetails:lines` [`search_types`]
- `use_cases`: `scheduled_shortage_check_creates_purchase_orders`, `weekly_refresh_of_all_snapshots`
- `doc_inconsistency`: `future_horizon_date_described_from_past_horizon`
- `record_actions`: `refresh:snapshot_past_and_future_transactions`
- `governance_note`: `refresh:5`
- `ui_location`: `refresh_button`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `daterun` | datetime | Date Run | no | no |  | `purpose`: `snapshot_date`; `value_source`: `system_displayed` |
| `futurehorizon` | integer | Future Horizon | no | no |  | `purpose`: `future_horizon`; `configured_by`: `item_record_or_default_preference`; `default_behavior`: `default_future_horizon_used` (when `blank`) |
| `futurehorizondate` | date | Future Horizon Date | no | no |  | `purpose`: `future_horizon_days`; `value_examples`: `30_includes_orders_due_in_30_days` |
| `item` | select | Item Name/Number | yes | no |  | `purpose`: `snapshot_item`; `allowed_values`: `assembly_items`, `inventory_items` |
| `memo` | textarea | Memo | no | no |  | `purpose`: `memo` |
| `pasthorizon` | select | Past Horizon | yes | no |  | `purpose`: `past_horizon`; `default_behavior`: `default_past_horizon_preference`; `max_value`: `60` days |
| `pasthorizondate` | date | Past Horizon Date | no | no |  | `purpose`: `past_horizon_days` |
| `replenishmentmethod` | select | Replenishment Method | no | no |  | `purpose`: `replenishment_method`; `value_source`: `system_displayed` |
| `stockunit` | select | Stock Unit | no | no |  | `purpose`: `stock_unit`; `value_source`: `system_displayed` |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `daterun` | datetime | Date Run |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `futurehorizon` | integer | Future Horizon |  |
| `futurehorizondate` | date | Future Horizon Date |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `pasthorizon` | integer | Past Horizon |  |
| `pasthorizondate` | date | Past Horizon Date |  |
| `replenishmentmethod` | select | Replenishment Method |  |
| `stockunit` | select | Stock Unit |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `daterun` | datetime | Date Run |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `futurehorizon` | integer | Future Horizon |  |
| `futurehorizondate` | date | Future Horizon Date |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `pasthorizon` | integer | Past Horizon |  |
| `pasthorizondate` | date | Past Horizon Date |  |
| `replenishmentmethod` | text | Replenishment Method |  |
| `stockunit` | text | Stock Unit |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `item` | Item | Item |

## Suspected source errors

> [!SOURCE-ERROR] `future_horizon_date_described_from_past_horizon`
<!-- source-error: record:supplychainsnapshot | - | future_horizon_date_described_from_past_horizon -->
