# Role_subsidiary_map (`role_subsidiary_map`)
<!-- id: connect:role_subsidiary_map -->

| Fact | Value |
|---|---|
| Table | `role_subsidiary_map` |
| Primary key | `role_id`, `subsidiary_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `role_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `role_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `role_subsidiary_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
