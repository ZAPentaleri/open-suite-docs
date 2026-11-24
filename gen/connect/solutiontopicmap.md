# Solutiontopicmap (`solutiontopicmap`)
<!-- id: connect:solutiontopicmap -->

| Fact | Value |
|---|---|
| Table | `solutiontopicmap` |
| Primary key | `topic_id`, `solution_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `solution_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `solution_id` |
| `topic_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `topic_id` |
