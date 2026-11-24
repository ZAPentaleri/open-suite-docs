# Commission_authorization_link (`commission_authorization_link`)
<!-- id: connect:commission_authorization_link -->

| Fact | Value |
|---|---|
| Table | `commission_authorization_link` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `authorized_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `commission_amount_authorized` |
| `calc_type` | VARCHAR2 | 11 |  |  |  |  | `purpose`: `commission_calculation_type`; `value_examples`: `per_period`, `per_transaction` |
| `commission_transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `commission_transaction_id` |
| `commission_transaction_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `commission_transaction_line_id` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `schedule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `commission_schedule_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
