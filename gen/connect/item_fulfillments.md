# Item_fulfillments (`item_fulfillments`)
<!-- id: connect:item_fulfillments -->

| Fact | Value |
|---|---|
| Table | `item_fulfillments` |
| Record | `itemfulfillment` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `accounting_period_id` |
| `ancillary_endorsement` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `ancillary_endorsement` |
| `b13a_filling_option` | VARCHAR2 | 24 |  |  |  |  | `purpose`: `b13a_filling_option` |
| `b13a_statement_data` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `b13a_statement_data` |
| `backup_ship_notification_email` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `backup_ship_notification_email` |
| `billaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `bill_address` |
| `booking_confirmation_number` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `booking_confirmation_number` |
| `carrier_identification_code` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `carrier_identification_code` |
| `created_by_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `created_by_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `date_blanket_period_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_blanket_period_end` |
| `date_blanket_period_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_blanket_period_start` |
| `date_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_closed` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_future_shipping` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_future_shipping` |
| `date_home_delivery` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_home_delivery` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_license` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_license` |
| `date_sales_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `sales_effective_date` |
| `date_transaction` | TIMESTAMP |  |  |  |  |  | `purpose`: `transaction_date` |
| `delivery_instructions` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `delivery_instrcutions`; `doc_inconsistency`: `misspelled_instructions` |
| `eccn` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `eccn` |
| `email` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `email` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fulfillment_customer_or_job`; `allowed_values_listed_in`: `customers`, `projects` |
| `entry_number` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `entry_number` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `export_type` | VARCHAR2 | 22 |  |  |  |  | `purpose`: `export_type` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `fedex_ship_alert` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fedex_ship_alert` |
| `fob` | VARCHAR2 | 13 |  |  |  |  | `purpose`: `free_on_board` |
| `handling_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `handling_cost` |
| `has_cert_of_origin` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_certification_of_origin` |
| `has_commercial_invoice` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_commercial_invoice` |
| `has_nafta_cert_of_origin` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_nafta_certificate_of_origin`; `doc_inconsistency`: `certificate_of_written_as_certification_or` |
| `has_pro_forma_invoice` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_promo_forma_invoice` |
| `hold_at_location_address_one` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `hold_at_location_address_line_1` |
| `hold_at_location_address_three` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `hold_at_location_address_line_3` |
| `hold_at_location_address_two` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `hold_at_location_address_line_2` |
| `hold_at_location_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `hold_at_location_city` |
| `hold_at_location_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `hold_at_location_country` |
| `hold_at_location_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `hold_at_location_phone` |
| `hold_at_location_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `hold_at_location_state` |
| `hold_at_location_zip` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `hold_at_location_postal_code` |
| `home_delivery_type` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `home_delivery_type` |
| `in_bond_code` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `in_bond_code` |
| `international_exemption_number` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `international_exemption_number` |
| `is_certified_mail` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_certified_mail` |
| `is_fdx_signature_home_delivery` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `fedex_signature_home_delivery` |
| `is_held_at_location` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_held_at_location` |
| `is_holiday_delivery` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_holiday_delivery` |
| `is_inside_delivery` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inside_delivery` |
| `is_inside_pickup` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inside_pickup` |
| `is_intercompany` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_intercompany` |
| `is_memorized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_memorized` |
| `is_online_bill_pay` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_online_bill_pay` |
| `is_related_parties_transaction` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_related_parties_transaction` |
| `is_routed_export_transaction` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_routed_export_transaction` |
| `is_saturday_delivery` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_saturday_delivery` |
| `is_saturday_pickup` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_saturday_pickup` |
| `is_saturday_service` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_saturday_service` |
| `is_tax_reg_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_tax_region_override` |
| `is_visible_in_customer_center` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `visible_in_customer_center` |
| `is_weekend_delivery` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_weekend_delivery` |
| `license_exception` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `license_exception` |
| `license_number` | VARCHAR2 | 35 |  |  |  |  | `purpose`: `license_number` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `fulfillment_memo`; `caveat`: `optional_value`; `supported_in`: `search`; `displayed_on`: `reports` |
| `message` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `message` |
| `method_of_transport` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `method_of_transport` |
| `needs_bill` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `needs_bill` |
| `notification_email_message` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `notification_email_message` |
| `promotion_code_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `promotion_code_id` |
| `reason_for_export` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `reason_for_export` |
| `recipient_customs_id` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `recipient_customs_id` |
| `recipient_customs_id_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `recipient_customs_id_type` |
| `recipient_tax_id` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `recipient_tax_id` |
| `related_tranid` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `related_transaction_id` |
| `return_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `return_type` |
| `return_type_description` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `return_type_description` |
| `ship_notification_email` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `ship_notification_email` |
| `ship_notification_email_two` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `ship_notification_email_2` |
| `shipaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `ship_address` |
| `shipping_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_cost` |
| `shipping_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_item_id` |
| `status` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `status` |
| `tax_reg_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_no_longer_supported` |
| `terms_freight_charge` | NUMBER |  | 39 | 0 |  |  | `purpose`: `terms_freight_charge` |
| `terms_insurance_charge` | NUMBER |  | 39 | 0 |  |  | `purpose`: `terms_of_insurance_charge` |
| `terms_of_sale` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `terms_of_sale` |
| `title` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `title` |
| `total_package_weight_in_lbs` | NUMBER |  | 39 | 0 |  |  | `purpose`: `total_package_weight`; `value_unit`: `pounds` |
| `tranid` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `tran_id` |
| `transaction_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `transaction_extension_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transfer_location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transfer_location_id` |
| `use_backup_email` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_backup_email` |
| `use_fedex_ship_alert` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_fedex_ship_alert` |
| `use_ship_notification_email` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_ship_notification_email` |
| `hold_at_location_loc_phone` | VARCHAR2 | 100 |  |  |  |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `misspelled_instructions` (on `delivery_instructions`)
<!-- source-error: connect:item_fulfillments | delivery_instructions | misspelled_instructions -->

> [!SOURCE-ERROR] `certificate_of_written_as_certification_or` (on `has_nafta_cert_of_origin`)
<!-- source-error: connect:item_fulfillments | has_nafta_cert_of_origin | certificate_of_written_as_certification_or -->
