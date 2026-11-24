# Consolidated_exchange_rates (`consolidated_exchange_rates`)
<!-- id: connect:consolidated_exchange_rates -->

| Fact | Value |
|---|---|
| Table | `consolidated_exchange_rates` |
| Record | `consolidatedexchangerate` |
| Primary key | `consolidated_exchange_rate_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `accounting_book_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `average_budget_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `average_budget_exchange_rate` |
| `average_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `average_actual_exchange_rate` |
| `consolidated_exchange_rate_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `consolidated_exchange_rate_id` |
| `current_budget_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `current_budget_exchange_rate` |
| `current_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `current_actual_exchange_rate` |
| `from_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `originating_subsidiary` |
| `historical_budget_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `historical_budget_exchange_rate` |
| `historical_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `historical_actual_exchange_rate` |
| `to_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `receiving_subsidiary` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `consolidated_exchange_rates_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `consolidated_exchange_rates_from_subsidiaries_fk` | `from_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `consolidated_exchange_rates_to_subsidiaries_fk` | `to_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
