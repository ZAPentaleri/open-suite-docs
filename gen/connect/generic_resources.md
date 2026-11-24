# Generic_resources (`generic_resources`)
<!-- id: connect:generic_resources -->

| Fact | Value |
|---|---|
| Table | `generic_resources` |
| Record | `genericresource` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_class_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `generic_resource_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `generic_resources_extension_id` |
| `generic_resource_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `generic_resource_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `labor_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `labor_cost` |
| `price` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price` |
| `resource_name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `resource_name` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
| `workcalendar_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `work_calendar_id` |
