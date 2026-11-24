# Employee_currency_map (`employee_currency_map`)
<!-- id: connect:employee_currency_map -->

| Fact | Value |
|---|---|
| Table | `employee_currency_map` |
| Primary key | `employee_id`, `currency_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | primary_key | `purpose`: `currency_id` |
| `employee_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | primary_key | `purpose`: `employee_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `employee_currency_map_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `employee_currency_map_employees_fk` | `employee_id` | `employees` | `employee_id` |
