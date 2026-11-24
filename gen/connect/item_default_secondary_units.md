# Item_default_secondary_units (`item_default_secondary_units`)
<!-- id: connect:item_default_secondary_units -->

| Fact | Value |
|---|---|
| Table | `item_default_secondary_units` |
| Primary key | `item_id`, `unit_purpose` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | primary_key |  |
| `secondary_unit_id` | NUMBER |  | 39 | 0 | `uom`.`uom_id` | foreign_key |  |
| `unit_purpose` | VARCHAR2 | 30 |  |  |  | primary_key |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_default_secondary_units_items_fk` | `item_id` | `items` | `item_id` |
| `item_default_secondary_units_uom_fk` | `secondary_unit_id` | `uom` | `uom_id` |
