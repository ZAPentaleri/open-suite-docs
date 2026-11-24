# Currencyrates (`currencyrates`)
<!-- id: connect:currencyrates -->

| Fact | Value |
|---|---|
| Table | `currencyrates` |
| Record | `currencyrate` |
| Primary key | `currencyrate_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `anchor_currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `anchor_currency_id` |
| `base_currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `base_currency_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `currencyrate_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `currency_rate_id` |
| `currencyrate_provider_id` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `currency_rate_provider_id` |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_effective` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `user_who_entered_rate` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `is_anchor_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `anchor_only_flag` |
| `update_method_id` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `update_method_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `currencyrates_entity_fk` | `entity_id` | `entity` | `entity_id` |
