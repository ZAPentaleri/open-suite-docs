# Currency_exchange_rates (`currency_exchange_rates`)
<!-- id: connect:currency_exchange_rates -->

| Fact | Value |
|---|---|
| Table | `currency_exchange_rates` |
| Primary key | `currency_rate_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `anchor_currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `anchor_currency`; `requires_feature`: `Currency Exchange Rate Integration` [`real_name`] |
| `base_currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `base_currency_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_currency`; `same_as`: `transaction_currency`, `foreign_currency` |
| `currency_rate_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `currency_rate_id` |
| `currency_rate_provider_id` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `exchange_rate_provider`; `requires_feature`: `Currency Exchange Rate Integration` [`real_name`] |
| `currency_rate_type_id` | NUMBER |  | 39 | 0 | `currency_exchange_rate_types`.`currency_rate_type_id` | foreign_key | `purpose`: `exchange_rate_type`; `requires_feature`: `Currency exchange rate types` [`real_name`] |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `exchange_rate_effective_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `associated_entity` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `currency_exchange_rate` |
| `is_anchor_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `anchor_only_flag`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `update_method_id` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `exchange_rate_update_method` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `currency_exchange_rates_currency_exchange_rate_types_fk` | `currency_rate_type_id` | `currency_exchange_rate_types` | `currency_rate_type_id` |
| `currency_exchange_rates_entity_fk` | `entity_id` | `entity` | `entity_id` |
