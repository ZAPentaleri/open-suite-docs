# Billing_schedule_descriptions (`billing_schedule_descriptions`)
<!-- id: connect:billing_schedule_descriptions -->

| Fact | Value |
|---|---|
| Table | `billing_schedule_descriptions` |
| Primary key | `billing_schedule_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_schedule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_schedule_id` |
| `billing_schedule_type` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `billing_schedule_type` |
| `frequency` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `frequency` |
| `name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `name` |
| `recurrence_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `recurrence_count` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_accounts_billing_schedule_descriptions_fk` | `billing_accounts` | `billing_schedule_id` | `billing_schedule_id` |
| `Charges_billing_schedule_descriptions_fk` | `charges` | `billing_schedule_id` | `billing_schedule_id` |
| `Customers_billing_schedule_descriptions_fk` | `customers` | `billing_schedule_id` | `billing_schedule_id` |
| `Transaction_lines_billing_schedule_descriptions_fk` | `transaction_lines` | `billing_schedule_id` | `billing_schedule_id` |
