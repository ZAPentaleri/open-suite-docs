# Account_subsidiary_map (`account_subsidiary_map`)
<!-- id: connect:account_subsidiary_map -->

| Fact | Value |
|---|---|
| Table | `account_subsidiary_map` |
| Primary key | `subsidiary_id`, `account_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `account_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subsidiary_id` |
