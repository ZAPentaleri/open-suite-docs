# Posting_account_activity (`posting_account_activity`)
<!-- id: connect:posting_account_activity -->

| Fact | Value |
|---|---|
| Table | `posting_account_activity` |
| Domains | General_accounting |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | foreign_key | `purpose`: `accounting_book_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `activity_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `activity_date` |
| `amount` | NUMBER |  | 25 | 2 |  |  | `purpose`: `amount` |
| `amount_foreign` | NUMBER |  | 25 | 2 |  |  | `purpose`: `foreign_amount` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `custom_transaction_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `custom_transaction_type_id` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `partner_id` | NUMBER |  | 39 | 0 | `partners`.`partner_id` | foreign_key | `purpose`: `partner_id` |
| `promotion_code_id` | NUMBER |  | 39 | 0 | `promotion_codes`.`promotion_code_id` | foreign_key | `purpose`: `promotion_code_id` |
| `quantity` | NUMBER |  | 25 | 8 |  |  | `purpose`: `quantity` |
| `sales_rep_id` | NUMBER |  | 39 | 0 | `sales_reps`.`sales_rep_id` | foreign_key | `purpose`: `sales_representative_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `transaction_type` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `transaction_type` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `posting_account_activity_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `posting_account_activity_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `posting_account_activity_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `posting_account_activity_classes_fk` | `class_id` | `classes` | `class_id` |
| `posting_account_activity_departments_fk` | `department_id` | `departments` | `department_id` |
| `posting_account_activity_items_fk` | `item_id` | `items` | `item_id` |
| `posting_account_activity_locations_fk` | `location_id` | `locations` | `location_id` |
| `posting_account_activity_partners_fk` | `partner_id` | `partners` | `partner_id` |
| `posting_account_activity_promotion_codes_fk` | `promotion_code_id` | `promotion_codes` | `promotion_code_id` |
| `posting_account_activity_sales_reps_fk` | `sales_rep_id` | `sales_reps` | `sales_rep_id` |
| `posting_account_activity_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
