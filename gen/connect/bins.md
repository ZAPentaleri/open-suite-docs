# Bins (`bins`)
<!-- id: connect:bins -->

| Fact | Value |
|---|---|
| Table | `bins` |
| Record | `bin` |
| Primary key | `bin_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bin_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `bin_id` |
| `bin_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `bin_number` |
| `external_id` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `is_inactive` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_inactive` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
