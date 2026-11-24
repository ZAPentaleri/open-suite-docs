# Solutioncasemap (`solutioncasemap`)
<!-- id: connect:solutioncasemap -->

| Fact | Value |
|---|---|
| Table | `solutioncasemap` |
| Primary key | `supportcase_id`, `solution_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_applied` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_applied` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `solution_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `solution_id` |
| `supportcase_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `support_case_id` |
