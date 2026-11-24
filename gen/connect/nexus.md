# Nexus (`nexus`)
<!-- id: connect:nexus -->

| Fact | Value |
|---|---|
| Table | `nexus` |
| Record | `nexus` |
| Primary key | `nexus_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `country` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `country_name` |
| `date_valid_since` | TIMESTAMP |  |  |  |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_no_longer_supported` |
| `date_valid_until` | TIMESTAMP |  |  |  |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_no_longer_supported` |
| `description` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `description` |
| `hierarchy_level` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_no_longer_supported` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `nexus_inactive`; `requires_feature`: `SuiteTax` [`real_name`] |
| `nexus_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `nexus_id` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_nexus`; `requires_feature`: `SuiteTax` [`real_name`] |
| `state` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `state` |
| `tax_agency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `first_tax_agency_vendor`; `requires_feature`: `SuiteTax` [`real_name`] |
| `taxcode_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `nexus_tax_code`; `available_only_when`: `suitetax_disabled` |
| `vendtax1_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `first_tax_agency_vendor`; `available_only_when`: `suitetax_disabled` |
| `vendtax2_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `second_tax_agency_vendor`; `available_only_when`: `suitetax_disabled` |
| `is_tax_date_from_fulfillment` | VARCHAR2 | 3 |  |  |  |  |  |
