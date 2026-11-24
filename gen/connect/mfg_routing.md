# Mfg_routing (`mfg_routing`)
<!-- id: connect:mfg_routing -->

| Fact | Value |
|---|---|
| Table | `mfg_routing` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bom_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_of_materials_id` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `routing_created_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `routing_last_modified_date` |
| `externalid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `is_autocalculate_lag` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `autocalculate_lag` |
| `isdefault` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `mfg_routing_is_default` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `mfg_routing_is_inactive` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `mfg_routing_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `mfg_routing_id` |
| `mfg_routing_memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `mfg_routing_memo` |
| `mfg_routing_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mfg_routing_name` |
