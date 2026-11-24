# Global_inventory_relationships (`global_inventory_relationships`)
<!-- id: connect:global_inventory_relationships -->

| Fact | Value |
|---|---|
| Table | `global_inventory_relationships` |
| Record | `globalinventoryrelationship` |
| Primary key | `global_invt_relationship_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `global_invt_relationship_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `global_inventory_relationship_external_id` |
| `global_invt_relationship_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `global_inventory_relationship_id` |
| `inventory_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `inventory_subsidiary_id` |
| `is_all_locations_cust_return` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `all_locations_customer_return` |
| `is_all_locations_fulfillment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_all_locations_fulfillment` |
| `is_all_locations_receipt` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `is_all_locations_vend_return` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `is_allow_cross_sub_cust_return` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `cross_subsidiary_customer_return_allowed` |
| `is_allow_cross_sub_fulfillment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_allow_cross_subsidiary_fulfillment` |
| `is_allow_cross_sub_receipt` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `is_allow_cross_sub_vend_return` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `originating_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `originating_subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `global_inventory_relationships_invt_subsidiaries_fk` | `inventory_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `global_inventory_relationships_orig_subsidiaries_fk` | `originating_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
