# Partner_types (`partner_types`)
<!-- id: connect:partner_types -->

| Fact | Value |
|---|---|
| Table | `partner_types` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subtype_of` |
| `partner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `partner_id` |
| `partner_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `partner_type_external_id` |
| `partner_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `partner_type_id` |
