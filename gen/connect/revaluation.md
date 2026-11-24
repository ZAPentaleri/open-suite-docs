# Revaluation (`revaluation`)
<!-- id: connect:revaluation -->

| Fact | Value |
|---|---|
| Table | `revaluation` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `account_id` |
| `balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `base_currency_balance` |
| `curr_fx_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `current_exchange_rate` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `fx_balance` | NUMBER |  | 21 | 3 |  |  | `purpose`: `foreign_currency_balance` |
| `is_tax_reg_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_tax_registration_override` |
| `legal_name` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `legal_name` |
| `net_variance` | NUMBER |  | 20 | 2 |  |  | `purpose`: `net_variance` |
| `prior_variance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `prior_variance` |
| `promotion_code_instance_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `promotion_code_id` |
| `reval_class` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revaluation_class` |
| `reval_department` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revaluation_department` |
| `reval_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revaluation_journal_line` |
| `reval_location` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revaluation_location` |
| `reval_memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `revaluation_memo` |
| `reval_tran_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revaluation_journal_transaction` |
| `reval_tran_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revaluation_journal_posting_period` |
| `reval_type` | VARCHAR2 | 24 |  |  |  |  | `purpose`: `revaluation_transaction_type` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
| `tax_reg_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_no_longer_supported` |
| `total_variance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `total_variance` |
| `tran_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `source_transaction_posting_date` |
| `tran_fx_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `transaction_exchange_rate` |
| `tran_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_transaction_id` |
| `variance` | NUMBER |  | 20 | 2 |  |  | `purpose`: `variance` |
| `variance_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `variance_account_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `revaluation_accounts_fk` | `variance_account_id` | `accounts` | `account_id` |
