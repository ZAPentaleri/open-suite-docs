# Budget (`budget`)
<!-- id: connect:budget -->

| Fact | Value |
|---|---|
| Table | `budget` |
| Primary key | `budget_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | foreign_key | `purpose`: `accounting_book_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `budget_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date` |
| `budget_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `budget_id` |
| `category_id` | NUMBER |  | 39 | 0 | `budget_category`.`budget_category_id` | foreign_key | `purpose`: `budget_category_id` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `customer_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `customer_id` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `budget_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `budget_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `budget_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `budget_budget_category_fk` | `category_id` | `budget_category` | `budget_category_id` |
| `budget_classes_fk` | `class_id` | `classes` | `class_id` |
| `budget_customers_fk` | `customer_id` | `customers` | `customer_id` |
| `budget_departments_fk` | `department_id` | `departments` | `department_id` |
| `budget_items_fk` | `item_id` | `items` | `item_id` |
| `budget_locations_fk` | `location_id` | `locations` | `location_id` |
| `budget_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
