# Salesforecast (`salesforecast`)
<!-- id: connect:salesforecast -->

| Fact | Value |
|---|---|
| Table | `salesforecast` |
| Primary key | `forecast_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `calculated_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `most_likely_calculated` |
| `date_entered` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_entered` |
| `forecast_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `most_likely_override` |
| `forecast_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `forecast_date` |
| `forecast_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_forecast_id` |
| `high_calculated_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `upside_calculated` |
| `high_forecasted_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `upside_override` |
| `is_alt_sales` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `alt_sales` |
| `low_calculated_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `worst_case_calculated` |
| `low_forecasted_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `worst_case_override` |
| `manager_override` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `manager_override` |
| `modified_by_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `modified_by_id` |
| `pipeline_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `pipeline_amount` |
| `salesrep_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_rep_id` |
| `subsidiary` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary` |
| `team_forecast` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `team_forecast` |
