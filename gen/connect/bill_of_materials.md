# Bill_of_materials (`bill_of_materials`)
<!-- id: connect:bill_of_materials -->

| Fact | Value |
|---|---|
| Table | `bill_of_materials` |
| Primary key | `bill_of_materials_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `available_for_all_assemblies` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `available_for_all_assemblies` |
| `available_for_all_locations` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `available_for_all_locations` |
| `bill_of_materials_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `bom_external_id` |
| `bill_of_materials_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `bill_of_materials_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `exclude_child_subsidiaries` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `exclude_child_subsidiaries` |
| `form_template_component_id` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `form_template_component_id` |
| `form_template_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `memo` | VARCHAR2 | 1024 |  |  |  |  | `purpose`: `memo` |
| `name` | VARCHAR2 | 600 |  |  |  |  | `purpose`: `name` |
| `original_assembly_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `original_assembly_id` |
| `use_component_yield` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_component_yield` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `bill_of_materials_items_fk` | `original_assembly_id` | `items` | `item_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Assembly_boms_bill_of_materials_fk` | `assembly_boms` | `bill_of_materials_id` | `bill_of_materials_id` |
| `Bom_revisions_bill_of_materials_fk` | `bom_revisions` | `bill_of_materials_id` | `bill_of_materials_id` |
