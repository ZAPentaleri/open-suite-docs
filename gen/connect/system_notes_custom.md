# System_notes_custom (`system_notes_custom`)
<!-- id: connect:system_notes_custom -->

| Fact | Value |
|---|---|
| Table | `system_notes_custom` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `author_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `author_id` |
| `company_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `company_id` |
| `context_type_name` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `context_type_name` |
| `custom_field` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `changed_line_field`; `value_examples`: `description`, `account` |
| `customfield_created_by_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `custom_field_created_by` |
| `customfield_modified_by_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `custom_field_modified_by` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_customfield_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `custom_field_created_date` |
| `date_customfield_modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `custom_field_modified_date` |
| `event_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `event_id` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | foreign_key | `purpose`: `line_id` |
| `line_transaction_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_id` | foreign_key | `purpose`: `line_transaction_id` |
| `name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `name` |
| `note_type_id` | NUMBER |  | 39 | 0 | `notetype`.`note_type_id` | foreign_key | `purpose`: `note_type_id` |
| `operation` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `system_note_operation`; `value_examples`: `changed`, `set` |
| `record_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_id` |
| `record_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_type_id` |
| `role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `role_id` |
| `standard_field` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `standard_field` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `transaction_id` |
| `value_new` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `custom_field_note_new_value`; `value_kind`: `value_after_change_or_value_set`; `caveat`: `null_for_unset_operations`; `doc_inconsistency`: `example_value_missing_closing_quote` |
| `value_old` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `custom_field_note_old_value`; `value_kind`: `value_before_change_or_unset`; `caveat`: `null_for_set_operations` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `system_notes_custom_entity_fk` | `company_id` | `entity` | `entity_id` |
| `system_notes_custom_entity_fk_2` | `author_id` | `entity` | `entity_id` |
| `system_notes_custom_entity_fk_3` | `customfield_created_by_id` | `entity` | `entity_id` |
| `system_notes_custom_entity_fk_4` | `customfield_modified_by_id` | `entity` | `entity_id` |
| `system_notes_custom_items_fk` | `item_id` | `items` | `item_id` |
| `system_notes_custom_notetype_fk` | `note_type_id` | `notetype` | `note_type_id` |
| `system_notes_custom_transaction_lines_fk` | `line_transaction_id`, `line_id` | `transaction_lines` | `transaction_id`, `transaction_line_id` |
| `system_notes_custom_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |

## Suspected source errors

> [!SOURCE-ERROR] `example_value_missing_closing_quote` (on `value_new`)
<!-- source-error: connect:system_notes_custom | value_new | example_value_missing_closing_quote -->
