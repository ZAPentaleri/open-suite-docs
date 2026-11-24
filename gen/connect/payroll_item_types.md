# Payroll_item_types (`payroll_item_types`)
<!-- id: connect:payroll_item_types -->

| Fact | Value |
|---|---|
| Table | `payroll_item_types` |
| Primary key | `payroll_item_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `child_of` |
| `payroll_item_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `payroll_item_type_id` |
