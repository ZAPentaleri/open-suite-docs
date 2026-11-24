# Subsidiary_nexus_map (`subsidiary_nexus_map`)
<!-- id: connect:subsidiary_nexus_map -->

| Fact | Value |
|---|---|
| Table | `subsidiary_nexus_map` |
| Primary key | `nexus_id`, `subsidiary_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `nexus_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `nexus_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subsidiary_id` |
