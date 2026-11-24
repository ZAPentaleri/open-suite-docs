# Commissionplan (`commissionplan`)
<!-- id: connect:commissionplan -->

| Fact | Value |
|---|---|
| Table | `commissionplan` |
| Primary key | `commission_plan_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `commission_plan` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `commission_plan` |
| `commission_plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `commission_plan_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_modified` |
| `description` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
