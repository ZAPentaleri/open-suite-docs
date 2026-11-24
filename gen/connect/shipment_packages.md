# Shipment_packages (`shipment_packages`)
<!-- id: connect:shipment_packages -->

| Fact | Value |
|---|---|
| Table | `shipment_packages` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `admissibility_package_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `admissibility_package_type` |
| `alcohol_recipient_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `alcohol_recipient_type` |
| `authorization_number` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `authorization_number` |
| `carrier_packaging` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `carrier_packaging` |
| `cash_on_delivery_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `cash_on_delivery_amount` |
| `cash_on_delivery_method` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `cash_on_delivery_method` |
| `cod_other_charge` | NUMBER |  | 39 | 0 |  |  | `purpose`: `cod_other_charge` |
| `contents_description` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `contents_description` |
| `declared_value` | NUMBER |  | 39 | 0 |  |  | `purpose`: `declared_value` |
| `delivery_confirmation` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `delivery_confirmation` |
| `dimension_unit` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `dimension_unit` |
| `dry_ice_weight` | NUMBER |  | 39 | 0 |  |  | `purpose`: `dry_ice_weight` |
| `freight_charge_added_to_cod` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `freight_charge_added_to_cod` |
| `has_additional_handling` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_additional_handling` |
| `has_cash_on_delivery` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_cash_on_delivery` |
| `has_declared_value` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_declared_value` |
| `has_insured_value` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_insured_value` |
| `insured_value` | NUMBER |  | 39 | 0 |  |  | `purpose`: `insured_value` |
| `is_alcohol` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_alcohol` |
| `is_non_hazardous_li_batteries` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_non_hazardous_li_batteries` |
| `is_non_standard_container` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_non_standard_container` |
| `item_fulfillment_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_fulfillment_id` |
| `package_height` | NUMBER |  | 39 | 0 |  |  | `purpose`: `package_height` |
| `package_length` | NUMBER |  | 39 | 0 |  |  | `purpose`: `package_length` |
| `package_width` | NUMBER |  | 39 | 0 |  |  | `purpose`: `package_width` |
| `priority_alert_content` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `priority_alert_content` |
| `priority_alert_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `priority_alert_type` |
| `reference_one` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `reference_one` |
| `reference_two` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `reference_two` |
| `signature_option` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `signature_option` |
| `signature_release` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `signature_release` |
| `tracking_number` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `tracking_number` |
| `weight_in_lbs` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weight_in_pounds` |
| `your_packaging` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `your_packaging` |
