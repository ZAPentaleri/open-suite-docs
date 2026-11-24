# Expense_amortization_rules (`expense_amortization_rules`)
<!-- id: connect:expense_amortization_rules -->

| Fact | Value |
|---|---|
| Table | `expense_amortization_rules` |
| Primary key | `expense_rule_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount_source` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `amount_source` |
| `contra_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `contra_account_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `end_date_change_impact` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `end_date_change_impact` |
| `end_date_source` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `end_date_source` |
| `expense_rule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `expense_rule_id` |
| `initial_amount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `initial_amount` |
| `name` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `name` |
| `period_offset` | NUMBER |  | 39 | 0 |  |  | `purpose`: `period_offset` |
| `recognition_period` | NUMBER |  | 39 | 0 |  |  | `purpose`: `recognition_period` |
| `reforecast_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `reforecast_method` |
| `residual` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `residual` |
| `rule_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `rule_method` |
| `start_date_source` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `start_date_source` |
| `start_offset` | NUMBER |  | 39 | 0 |  |  | `purpose`: `start_offset` |
| `term_in_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_in_days` |
| `term_in_months` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_in_months` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `expense_amortization_rules_accounts_fk` | `contra_account_id` | `accounts` | `account_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Expense_plans_expense_amortization_rules_fk` | `expense_plans` | `expense_rule_id` | `expense_rule_id` |
