# Currency_exchange_rate_types (`currency_exchange_rate_types`)
<!-- id: connect:currency_exchange_rate_types -->

| Fact | Value |
|---|---|
| Table | `currency_exchange_rate_types` |
| Primary key | `currency_rate_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_rate_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `exchange_rate_type_external_id` |
| `currency_rate_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `exchange_rate_type_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 2000 |  |  |  |  | `purpose`: `exchange_rate_type_description`; `caveat`: `optional_value` |
| `is_default` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `default_exchange_rate_type` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `exchange_rate_type_inactive` |
| `name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `exchange_rate_type_name`; `constraint`: `unique` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Currency_exchange_rates_currency_exchange_rate_types_fk` | `currency_exchange_rates` | `currency_rate_type_id` | `currency_rate_type_id` |
