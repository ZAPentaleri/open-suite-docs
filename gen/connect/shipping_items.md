# Shipping_items (`shipping_items`)
<!-- id: connect:shipping_items -->

| Fact | Value |
|---|---|
| Table | `shipping_items` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `by_per_item` | NUMBER |  | 39 | 0 |  |  | `purpose`: `per_item` |
| `by_total_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `percent_of_total` |
| `by_weight_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `by_weight_amount` |
| `by_weight_unit_index` | NUMBER |  | 39 | 0 |  |  | `purpose`: `by_weight_unit_index` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `default_unit_price` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `default_unit_price` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `display_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `display_name_code` |
| `flat_rate_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `flat_rate_amount` |
| `free_if_over_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `free_shipping_order_threshold` |
| `free_if_overactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `free_if_overactive` |
| `full_name` | VARCHAR2 | 550 |  |  |  |  | `purpose`: `full_name` |
| `income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `income_account` |
| `is_fedex_one_rate` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `fedex_one_rate_used` |
| `is_omit_packaging` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_packaging_is_omitted` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `ship_item_is_inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `display_in_web_site` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `item_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_external_id` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `maximum_shipping_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `maximum_shipping_amount` |
| `minimum_shipping_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `minimum_shipping_amount` |
| `name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subitem_of` |
| `per_item_default_price` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `per_item_default_price` |
| `pref_purchase_tax_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_purchase_tax_id` |
| `shipping_cost_function` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `shipping_cost_function` |
| `shipping_rate` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_rate` |
| `tax_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_item_id` |
| `use_maximum_ship_cost` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_maximum_shipping_amount` |
| `use_minimum_ship_cost` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_minimum_shipping_amount` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_id` |
