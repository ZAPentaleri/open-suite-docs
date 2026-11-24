# Item_group (`item_group`)
<!-- id: connect:item_group -->

| Fact | Value |
|---|---|
| Table | `item_group` |
| Record | `itemgroup` |
| Primary key | `item_group_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bom_quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `bom_quantity` |
| `component_yield` | NUMBER |  | 39 | 0 |  |  | `purpose`: `component_yield` |
| `effective_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `effective_date` |
| `effective_revision` | NUMBER |  | 39 | 0 |  |  | `purpose`: `effective_revision` |
| `item_group_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_group_id` |
| `item_source` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `item_source` |
| `line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_id` |
| `member_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `member_id` |
| `obsolete_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `obsolete_day` |
| `obsolete_revision` | NUMBER |  | 39 | 0 |  |  | `purpose`: `obsolete_revision` |
| `parent_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `parent_id` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `rate_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `rate_plan`; `not_supported`: `column_no_longer_supported` |
| `unit_of_measure_id` | NUMBER |  | 39 | 0 | `uom`.`uom_id` | foreign_key | `purpose`: `unit_of_measure_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_group_items_fk` | `parent_id` | `items` | `item_id` |
| `item_group_uom_fk` | `unit_of_measure_id` | `uom` | `uom_id` |
