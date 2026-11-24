# Billing_classes (`billing_classes`)
<!-- id: connect:billing_classes -->

| Fact | Value |
|---|---|
| Table | `billing_classes` |
| Primary key | `billing_class_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_class_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `billing_class_external_id` |
| `billing_class_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_class_id` |
| `description` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `name` |
| `sale_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sale_unit_id` |
| `units_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `units_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_prices_billing_classes_fk` | `billing_rate_cards_prices` | `billing_class_id` | `billing_class_id` |
| `Employee_time_billing_classes_fk` | `employee_time` | `billing_class_id` | `billing_class_id` |
| `Employees_billing_classes_fk` | `employees` | `billing_class_id` | `billing_class_id` |
| `Item_billing_rates_billing_classes_fk` | `item_billing_rates` | `billing_class_id` | `billing_class_id` |
