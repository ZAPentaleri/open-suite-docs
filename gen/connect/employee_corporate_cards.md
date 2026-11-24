# Employee_corporate_cards (`employee_corporate_cards`)
<!-- id: connect:employee_corporate_cards -->

| Fact | Value |
|---|---|
| Table | `employee_corporate_cards` |
| Primary key | `employee_id`, `format_profile_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `employee_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | primary_key | `purpose`: `employee_id` |
| `format_profile_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `format_profile_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `employee_corporate_cards_employees_fk` | `employee_id` | `employees` | `employee_id` |
