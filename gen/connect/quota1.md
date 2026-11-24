# Quota1 (`quota1`)
<!-- id: connect:quota1 -->

| Fact | Value |
|---|---|
| Table | `quota1` |
| Primary key | `quota_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `class_0` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_id` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `date` |
| `department` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_id` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `is_alt_sales` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `alt_sales` |
| `item` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `location_0` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `period` | NUMBER |  | 39 | 0 |  |  | `purpose`: `period` |
| `quarterly_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `quarterly_amount` |
| `quota_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `quota_id` |
| `subsidiary` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
| `team_quota` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `team_quota` |
