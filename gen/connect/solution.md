# Solution (`solution`)
<!-- id: connect:solution -->

| Fact | Value |
|---|---|
| Table | `solution` |
| Record | `solution` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `id_0` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `code` |
| `is_available_online` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `available_online` |
| `long_description` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `long_description` |
| `solution_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `solution_external_id` |
| `solution_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `solution_id` |
