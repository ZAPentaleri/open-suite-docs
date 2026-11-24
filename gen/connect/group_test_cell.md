# Group_test_cell (`group_test_cell`)
<!-- id: connect:group_test_cell -->

| Fact | Value |
|---|---|
| Table | `group_test_cell` |
| Primary key | `crm_group_id`, `test_cell_id` |
| Domains | Campaignevents |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `crm_group_id` | NUMBER |  | 39 | 0 | `crmgroup`.`group_id` | primary_key | `purpose`: `crm_group` |
| `cumulativepct` | NUMBER |  | 39 | 0 |  |  | `purpose`: `cumulative_percentage` |
| `name` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `name` |
| `percentage` | NUMBER |  | 39 | 0 |  |  | `purpose`: `percentage` |
| `previouspct` | NUMBER |  | 39 | 0 |  |  | `purpose`: `previous_percentage` |
| `test_cell_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `test_cell_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `crmgroup_group_test_cell_fk` | `crm_group_id` | `crmgroup` | `group_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignevent_group_test_cell_fk` | `campaignevent` | `test_cell_id`, `group_id` | `test_cell_id`, `crm_group_id` |
