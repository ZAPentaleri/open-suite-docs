# Subsidiary_class_map (`subsidiary_class_map`)
<!-- id: connect:subsidiary_class_map -->

| Fact | Value |
|---|---|
| Table | `subsidiary_class_map` |
| Primary key | `subsidiary_id`, `class_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | primary_key | `purpose`: `class_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subsidiary_class_map_classes_fk` | `class_id` | `classes` | `class_id` |
| `subsidiary_class_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
