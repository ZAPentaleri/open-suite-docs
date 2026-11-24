# Inventory_statuses (`inventory_statuses`)
<!-- id: connect:inventory_statuses -->

| Fact | Value |
|---|---|
| Table | `inventory_statuses` |
| Primary key | `inventorystatus_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `description_translated` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `status_description_translated`; `value_kind`: `account_language_translation` |
| `inventorystatus_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `inventory_status_external_id` |
| `inventorystatus_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `inventory_status_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `is_inventory_avail_allocation` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `status_inventory_available_for_allocation` |
| `is_inventory_avail_commitment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `status_inventory_available_for_commitment` |
| `name` | VARCHAR2 | 180 |  |  |  |  | `purpose`: `inventory_status_name`; `constraint`: `unique` |
| `name_translated` | VARCHAR2 | 180 |  |  |  |  | `purpose`: `status_name_translated`; `value_kind`: `account_language_translation` |
| `state` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `status_state`; `option_meanings`: `A:active`, `D:default`, `I:inactive` |
