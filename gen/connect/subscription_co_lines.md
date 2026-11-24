# Subscription_co_lines (`subscription_co_lines`)
<!-- id: connect:subscription_co_lines -->

| Fact | Value |
|---|---|
| Table | `subscription_co_lines` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `change_order_action_id` | VARCHAR2 | 23 |  |  |  |  | `purpose`: `change_order_action_id` |
| `change_order_id` | NUMBER |  | 39 | 0 | `subscription_change_orders`.`change_order_id` | foreign_key | `purpose`: `change_order_id` |
| `discount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `discount` |
| `is_discount_percentage` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `discount_is_percentage` |
| `plan_item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `plan_item_id` |
| `price_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price_plan_id` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `recurring_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `recurring_amount` |
| `status_id` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `status_id` |
| `subscription_id` | NUMBER |  | 39 | 0 | `billing_subscriptions`.`subscription_id` | foreign_key | `purpose`: `subscription_id` |
| `subscription_line_id` | NUMBER |  | 39 | 0 | `billing_subscription_lines`.`subline_id` | foreign_key | `purpose`: `subscription_line_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subscription_co_lines_billing_subscription_lines_fk` | `subscription_line_id` | `billing_subscription_lines` | `subline_id` |
| `subscription_co_lines_billing_subscriptions_fk` | `subscription_id` | `billing_subscriptions` | `subscription_id` |
| `subscription_co_lines_items_fk` | `plan_item_id` | `items` | `item_id` |
| `subscription_co_lines_subscription_change_orders_fk` | `change_order_id` | `subscription_change_orders` | `change_order_id` |
