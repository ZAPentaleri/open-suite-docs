# Usages (`usages`)
<!-- id: connect:usages -->

| Fact | Value |
|---|---|
| Table | `usages` |
| Record | `usage` |
| Primary key | `usage_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_usage` | TIMESTAMP |  |  |  |  |  | `purpose`: `usage_date` |
| `status_id` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `usage_status` |
| `usage_customer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `usage_customer_id` |
| `usage_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `usage_external_id` |
| `usage_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `usage_id` |
| `usage_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `usage_item_id` |
| `usage_memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `usage_memo` |
| `usage_quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `usage_quantity` |
| `usage_subscription_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `usage_subscription_id` |
| `usage_subscription_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `usage_subscription_line_id` |
| `usage_subscription_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `usage_subscription_plan_id` |
