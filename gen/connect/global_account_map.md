# Global_account_map (`global_account_map`)
<!-- id: connect:global_account_map -->

| Fact | Value |
|---|---|
| Table | `global_account_map` |
| Primary key | `global_account_map_id` |
| Domains | Multibooks |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | foreign_key | `purpose`: `accounting_book_id` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_effective` |
| `date_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_end` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `destination_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `destination_account_id` |
| `form_template_component_id` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `form_template_component_id` |
| `form_template_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template` |
| `global_account_map_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `global_account_map_external_id` |
| `global_account_map_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `global_account_map_id` |
| `is_class_any` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `any_class` |
| `is_department_any` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `any_department` |
| `is_location_any` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `any_location` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `source_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `primary_book_account` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `global_account_map_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `global_account_map_accounts_fk_1` | `destination_account_id` | `accounts` | `account_id` |
| `global_account_map_accounts_fk_2` | `source_account_id` | `accounts` | `account_id` |
| `global_account_map_classes_fk` | `class_id` | `classes` | `class_id` |
| `global_account_map_departments_fk` | `department_id` | `departments` | `department_id` |
| `global_account_map_locations_fk` | `location_id` | `locations` | `location_id` |
| `global_account_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
