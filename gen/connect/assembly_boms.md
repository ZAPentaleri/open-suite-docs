# Assembly_boms (`assembly_boms`)
<!-- id: connect:assembly_boms -->

| Fact | Value |
|---|---|
| Table | `assembly_boms` |
| Primary key | `assembly_id`, `bill_of_materials_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `assembly_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | primary_key | `purpose`: `assembly_id` |
| `bill_of_materials_id` | NUMBER |  | 39 | 0 | `bill_of_materials`.`bill_of_materials_id` | primary_key | `purpose`: `bill_of_materials_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `assembly_boms_bill_of_materials_fk` | `bill_of_materials_id` | `bill_of_materials` | `bill_of_materials_id` |
| `assembly_boms_items_fk` | `assembly_id` | `items` | `item_id` |
