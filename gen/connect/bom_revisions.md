# Bom_revisions (`bom_revisions`)
<!-- id: connect:bom_revisions -->

| Fact | Value |
|---|---|
| Table | `bom_revisions` |
| Record | `bomrevision` |
| Primary key | `bom_revision_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bill_of_materials_id` | NUMBER |  | 39 | 0 | `bill_of_materials`.`bill_of_materials_id` | foreign_key | `purpose`: `bill_of_materials_id` |
| `bom_revision_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `bom_revision_external_id` |
| `bom_revision_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `bom_revision_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_effective` |
| `date_obsolete` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_obsolete` |
| `form_template_component_id` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `form_template_component_id` |
| `form_template_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `memo` | VARCHAR2 | 1024 |  |  |  |  | `purpose`: `memo` |
| `name` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `name` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `bom_revisions_bill_of_materials_fk` | `bill_of_materials_id` | `bill_of_materials` | `bill_of_materials_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Bom_revision_components_bom_revisions_fk` | `bom_revision_components` | `bom_revision_id` | `bom_revision_id` |
