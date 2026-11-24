# Bom_revision_components (`bom_revision_components`)
<!-- id: connect:bom_revision_components -->

| Fact | Value |
|---|---|
| Table | `bom_revision_components` |
| Primary key | `bom_revision_component_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bom_revision_component_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `revision_component_id` |
| `bom_revision_id` | NUMBER |  | 39 | 0 | `bom_revisions`.`bom_revision_id` | foreign_key | `purpose`: `revision_id` |
| `component_yield` | NUMBER |  | 39 | 0 |  |  | `purpose`: `component_yield` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `item_source` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `item_source` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `seq_num` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sequence_number` |
| `unit_of_measure_id` | NUMBER |  | 39 | 0 | `uom`.`uom_id` | foreign_key | `purpose`: `unit_of_measure_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `bom_revision_components_bom_revisions_fk` | `bom_revision_id` | `bom_revisions` | `bom_revision_id` |
| `bom_revision_components_items_fk` | `item_id` | `items` | `item_id` |
| `bom_revision_components_uom_fk` | `unit_of_measure_id` | `uom` | `uom_id` |
