# Subscription_line_revisions (`subscription_line_revisions`)
<!-- id: connect:subscription_line_revisions -->

| Fact | Value |
|---|---|
| Table | `subscription_line_revisions` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `change_order_id` | NUMBER |  | 39 | 0 | `subscription_change_orders`.`change_order_id` | foreign_key | `purpose`: `change_order_triggering_revision` |
| `created_by` | VARCHAR2 | 167 |  |  |  |  | `purpose`: `who_created_the_revision` |
| `date_change_order_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `triggering_change_order_effective_date` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_revision` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `revision_last_modified_date` |
| `delta_amount` | NUMBER |  | 26 | 8 |  |  | `purpose`: `amount_change_since_last_revision` |
| `delta_quantity` | NUMBER |  | 28 | 8 |  |  | `purpose`: `quantity_change_since_last_revision` |
| `discount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_discount`; `value_kind`: `initial_value_or_value_after_revision` |
| `is_applied_to_change_order` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `revision_from_line_unchecked_in_change_order` |
| `is_created_from_void` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `revision_from_voided_change_order` |
| `is_discount_percentage` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `discount_is_percentage` |
| `is_overage_discount_percentage` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `overage_discount_is_percentage` |
| `overage_discount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `overage_discount`; `value_kind`: `initial_value_or_value_after_revision` |
| `overage_price_plan_id` | NUMBER |  | 39 | 0 | `price_plans`.`price_plan_id` | foreign_key | `purpose`: `overage_price_plan`; `value_kind`: `initial_value_or_value_after_revision` |
| `price_plan_id` | NUMBER |  | 39 | 0 | `price_plans`.`price_plan_id` | foreign_key | `purpose`: `line_price_plan`; `value_kind`: `initial_value_or_value_after_revision` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_quantity`; `value_kind`: `initial_value_or_value_after_revision` |
| `recurring_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_recurring_amount`; `value_kind`: `initial_value_or_value_after_revision` |
| `revenue_element_id` | NUMBER |  | 39 | 0 | `revenue_elements`.`revenue_element_id` | foreign_key | `purpose`: `revision_revenue_element`; `value_source`: `usage_version_element_else_subscription_line_element` |
| `subscription_line_id` | NUMBER |  | 39 | 0 | `billing_subscription_lines`.`subline_id` | foreign_key | `purpose`: `id_of_line_revision` |
| `subscription_revision` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_revision_number` |
| `total_contract_value` | NUMBER |  | 26 | 8 |  |  | `purpose`: `contract_value_change_since_last_revision` |
| `date_end` | TIMESTAMP |  |  |  |  |  |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subscription_line_revisions_billing_subscription_lines_fk` | `subscription_line_id` | `billing_subscription_lines` | `subline_id` |
| `subscription_line_revisions_price_plans_fk` | `price_plan_id` | `price_plans` | `price_plan_id` |
| `subscription_line_revisions_price_plans_fk_2` | `overage_price_plan_id` | `price_plans` | `price_plan_id` |
| `subscription_line_revisions_revenue_elements_fk` | `revenue_element_id` | `revenue_elements` | `revenue_element_id` |
| `subscription_line_revisions_subscription_change_orders_fk` | `change_order_id` | `subscription_change_orders` | `change_order_id` |
