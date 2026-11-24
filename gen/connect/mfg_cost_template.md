# Mfg_cost_template (`mfg_cost_template`)
<!-- id: connect:mfg_cost_template -->

| Fact | Value |
|---|---|
| Table | `mfg_cost_template` |
| Primary key | `mfg_cost_template_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `cost_template_created_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `cost_template_last_modified_date` |
| `externalid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `cost_template_external_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `cost_template_inactive` |
| `mfg_cost_template_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `cost_template_id` |
| `mfg_cost_template_memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `cost_template_memo` |
| `mfg_cost_template_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `cost_template_name` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `cost_template_subsidiary` |
