# Subscription_terms (`subscription_terms`)
<!-- id: connect:subscription_terms -->

| Fact | Value |
|---|---|
| Table | `subscription_terms` |
| Record | `subscriptionterm` |
| Primary key | `term_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `duration` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_length_count`; `requires_together`: `duration`, `unit_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subscription_term_inactive` |
| `name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `subscription_term_name` |
| `term_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `subscription_term_external_id` |
| `term_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subscription_term_id` |
| `term_type_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `subscription_term_type_id` |
| `unit_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `term_length_unit`; `requires_together`: `duration`, `unit_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_subscriptions_subscription_terms_fk` | `billing_subscriptions` | `default_renewal_term_id` | `term_id` |
| `Billing_subscriptions_subscription_terms_fk_2` | `billing_subscriptions` | `initial_term_id` | `term_id` |
| `Subscription_change_orders_subscription_terms_fk` | `subscription_change_orders` | `renewal_term_id` | `term_id` |
| `Subscription_plans_subscription_terms_fk` | `subscription_plans` | `initial_term_id` | `term_id` |
| `Subscription_plans_subscription_terms_fk_2` | `subscription_plans` | `default_renewal_term_id` | `term_id` |
