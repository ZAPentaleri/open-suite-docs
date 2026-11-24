# Crmgroup (`crmgroup`)
<!-- id: connect:crmgroup -->

| Fact | Value |
|---|---|
| Table | `crmgroup` |
| Primary key | `group_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `group_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `group_id` |
| `group_type` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `group_type` |
| `is_mfg_work_center` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_manufacturing_work_center` |
| `is_private` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `private` |
| `is_sales_group` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `sales_group` |
| `is_support_group` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `support_group` |
| `labor_resources` | NUMBER |  | 39 | 0 |  |  | `purpose`: `labor_resources` |
| `machine_resources` | NUMBER |  | 39 | 0 |  |  | `purpose`: `machine_resources` |
| `owner` | NUMBER |  | 39 | 0 |  |  | `purpose`: `owner` |
| `title` | VARCHAR2 | 51 |  |  |  |  | `purpose`: `title` |
| `work_calendar_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `work_calendar_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignevent_crmgroup_fk` | `campaignevent` | `group_id` | `group_id` |
| `Crmgroup_group_test_cell_fk` | `group_test_cell` | `crm_group_id` | `group_id` |
