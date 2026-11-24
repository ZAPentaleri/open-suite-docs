# Billing_schedule (`billing_schedule`)
<!-- id: connect:billing_schedule -->

| Fact | Value |
|---|---|
| Table | `billing_schedule` |
| Record | `billingschedule` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bill_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `bill_amount` |
| `bill_amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_currency_bill_amount` |
| `bill_count` | NUMBER |  | 15 | 5 |  |  | `purpose`: `bill_count` |
| `bill_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `bill_date` |
| `bill_net_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_net_amount` |
| `bill_net_amount_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_net_amount`; `value_unit`: `foreign_currency` |
| `milestone_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `milestone_id` |
| `payment_terms_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_terms_id` |
| `rev_rec_end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `rev_rec_end_date` |
| `rev_rec_start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `rev_rec_start_date` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_line_id` |
