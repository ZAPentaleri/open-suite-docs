# Uom (`uom`)
<!-- id: connect:uom -->

| Fact | Value |
|---|---|
| Table | `uom` |
| Primary key | `uom_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `abbreviation` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `abbreviation_of_name` |
| `conversion_rate` | NUMBER |  | 18 | 8 |  |  | `purpose`: `conversion_rate_to_base_unit` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `is_base_unit` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_base_unit_of_units_type` |
| `name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `user_defined_name` |
| `plural_abbreviation` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `abbreviation_of_plural_name` |
| `plural_name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `plural_form_of_name` |
| `units_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_units_type` |
| `uom_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `unit_of_measure_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_prices_uom_fk` | `billing_rate_cards_prices` | `sale_unit_id` | `uom_id` |
| `Bom_revision_components_uom_fk` | `bom_revision_components` | `unit_of_measure_id` | `uom_id` |
| `Inbound_shipment_items_uom_fk` | `inbound_shipment_items` | `unit_id` | `uom_id` |
| `Item_default_secondary_units_uom_fk` | `item_default_secondary_units` | `secondary_unit_id` | `uom_id` |
| `Item_group_uom_fk` | `item_group` | `unit_of_measure_id` | `uom_id` |
