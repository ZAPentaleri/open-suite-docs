# Opportunities (`opportunities`)
<!-- id: connect:opportunities -->

| Fact | Value |
|---|---|
| Table | `opportunities` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `action_item` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `action_item` |
| `billaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `billing_address` |
| `closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `closed` |
| `company_status_id` | NUMBER |  | 39 | 0 | `company_status`.`company_status_id` | foreign_key | `purpose`: `company_status_id` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `email` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `email` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `expected_close` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_close` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `fob` | VARCHAR2 | 13 |  |  |  |  | `purpose`: `fob` |
| `forecast_type` | VARCHAR2 | 384 |  |  |  |  | `purpose`: `forecast_type` |
| `is_tax_reg_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_tax_registration_override` |
| `lead_source_id` | NUMBER |  | 39 | 0 | `campaigns`.`campaign_id` | foreign_key | `purpose`: `lead_source_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `memorized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `memorized` |
| `message` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `message` |
| `needs_bill` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `needs_bill` |
| `partner_id` | NUMBER |  | 39 | 0 | `partners`.`partner_id` | foreign_key | `purpose`: `partner_id` |
| `payment_terms_id` | NUMBER |  | 39 | 0 | `payment_terms`.`payment_terms_id` | foreign_key | `purpose`: `payment_terms_id` |
| `probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `probability` |
| `projected_total` | NUMBER |  | 20 | 2 |  |  | `purpose`: `projected_total` |
| `promotion_code_instance_id` | NUMBER |  | 39 | 0 | `coupon_codes`.`coupon_code_id` | foreign_key | `purpose`: `promotion_code_instance_id` |
| `related_tranid` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `related_transaction_id` |
| `renewal` | TIMESTAMP |  |  |  |  |  | `purpose`: `renewal` |
| `reversing_transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `reversing_transaction_id` |
| `sales_rep_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `sales_rep_id` |
| `shipaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `shipment_received` | TIMESTAMP |  |  |  |  |  | `purpose`: `shipment_received` |
| `shipping_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_item_id` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `status` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `status` |
| `tax_reg_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_no_longer_supported` |
| `title` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `title` |
| `trandate` | TIMESTAMP |  |  |  |  |  | `purpose`: `transaction_date` |
| `tranid` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `check_or_document` |
| `transaction_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `transaction_external_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_partner` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `transaction_partner` |
| `transaction_type` | VARCHAR2 | 192 |  |  |  |  | `purpose`: `transaction_type` |
| `weighted_total` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weighted_total` |
| `win_loss_reason_id` | NUMBER |  | 39 | 0 | `winlossreason`.`win_loss_reason_id` | foreign_key | `purpose`: `win_loss_reason_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `opportunities_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `opportunities_campaigns_fk` | `lead_source_id` | `campaigns` | `campaign_id` |
| `opportunities_company_status_fk` | `company_status_id` | `company_status` | `company_status_id` |
| `opportunities_coupon_codes_fk` | `promotion_code_instance_id` | `coupon_codes` | `coupon_code_id` |
| `opportunities_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `opportunities_entity_fk` | `sales_rep_id` | `entity` | `entity_id` |
| `opportunities_partners_fk` | `partner_id` | `partners` | `partner_id` |
| `opportunities_payment_terms_fk` | `payment_terms_id` | `payment_terms` | `payment_terms_id` |
| `opportunities_transactions_fk` | `reversing_transaction_id` | `transactions` | `transaction_id` |
| `opportunities_winlossreason_fk` | `win_loss_reason_id` | `winlossreason` | `win_loss_reason_id` |
