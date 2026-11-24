# Accounting_books (`accounting_books`)
<!-- id: connect:accounting_books -->

| Fact | Value |
|---|---|
| Table | `accounting_books` |
| Record | `accountingbook` |
| Primary key | `accounting_book_id` |
| Domains | Multibooks |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_book_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `accounting_book_extension_id` |
| `accounting_book_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `accounting_book_id` |
| `accounting_book_name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name_of_the_book` |
| `base_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | foreign_key | `purpose`: `base_book_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `effective_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `effective_period_id` |
| `form_template_component_id` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `form_template_component_id` |
| `form_template_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template_id` |
| `is_adjustment_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_adjustment_only` |
| `is_arrangement_level_reclass` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `arrangement_level_unbilled_receivable_reclass_used` |
| `is_consolidated` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_consolidated` |
| `is_contingent_revenue_handling` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_handles_contingent_revenue` |
| `is_include_child_subsidiaries` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_includes_child_subsidiaries` |
| `is_primary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `book_is_primary`; `false_means`: `secondary_book` |
| `is_two_step_revenue_allocation` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `two_step_revenue_allocation_used` |
| `status` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `status` |
| `unbilled_receivable_grouping` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `unbilled_receivable_grouping` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `accounting_books_accounting_books_fk` | `base_book_id` | `accounting_books` | `accounting_book_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Account_activity_accounting_books_fk` | `account_activity` | `accounting_book_id` | `accounting_book_id` |
| `Account_period_activity_accounting_books_fk` | `account_period_activity` | `accounting_book_id` | `accounting_book_id` |
| `Account_period_activity_pe_accounting_books_fk` | `account_period_activity_pe` | `accounting_book_id` | `accounting_book_id` |
| `Accounting_books_accounting_books_fk` | `accounting_books` | `base_book_id` | `accounting_book_id` |
| `Budget_accounting_books_fk` | `budget` | `accounting_book_id` | `accounting_book_id` |
| `Expense_plans_accounting_books_fk` | `expense_plans` | `transaction_book_id` | `accounting_book_id` |
| `Global_account_map_accounting_books_fk` | `global_account_map` | `accounting_book_id` | `accounting_book_id` |
| `Item_account_map_accounting_books_fk` | `item_account_map` | `accounting_book_id` | `accounting_book_id` |
| `Posting_account_activity_accounting_books_fk` | `posting_account_activity` | `accounting_book_id` | `accounting_book_id` |
| `Posting_account_activity_pe_accounting_books_fk` | `posting_account_activity_pe` | `accounting_book_id` | `accounting_book_id` |
| `Transaction_book_map_accounting_books_fk` | `transaction_book_map` | `accounting_book_id` | `accounting_book_id` |
| `Transaction_line_book_map_accounting_books_fk` | `transaction_line_book_map` | `accounting_book_id` | `accounting_book_id` |
