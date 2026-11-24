# Item Demand Plan (`itemdemandplan`)
<!-- id: record:itemdemandplan -->

| Fact | Value |
|---|---|
| Internal ID | `itemdemandplan` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_transform` |

## Record facts

- `matrix_sublists`: `demandplandetail`
- `recommendation`: `dynamic_mode`
- `requires`: `item_replenishment_method_time_phased`
- `plan_shapes`: `monthly:row_per_month_1_column`, `weekly:row_per_week_1_column`, `daily:row_per_week_7_columns`
- `requires`: `required_body_fields_set_before_matrix_values`
- `configured_by`: `first_day_of_week_company_preference` (when `weekly_row_boundaries_and_daily_column_order`)
- `default_value`: `sunday` (when `first_day_of_week`)
- `caveat`: `first_row_start_may_precede_body_start_date` (when `weekly_or_daily_plan`)
- `caveat`: `last_row_end_may_follow_body_end_date` (when `weekly_or_daily_plan`)
- `allowed_values`: `1`, `7` (when `daily_plan_quantity_column`)
- `effect`: `daily_column_1_is_company_first_day_of_week`
- `value_examples`: `monthly_and_weekly_quantity_column_always_1`
- `requires`: `select_line_before_current_matrix_value_apis` (refs `N/record.Record#selectLine`, `N/currentRecord.CurrentRecord#selectLine`)
- `script_types`: `client`, `server`
- `not_supported`: `copy`, `transform`
- `requires_feature`: `Demand Planning` [`real_name`]
- `doc_inconsistency`: `functions_sentence_names_invoice`
- `plan_types`: `monthly`, `weekly`, `daily`
- `rule`: `body_fields_before_matrix_values`
- `script_functions`: `create`, `update`, `delete`, `search`
- `matrix_sublists`: `demandplandetail`
- `recommendation`: `dynamic_mode`
- `requires_feature`: `Demand Planning` [`inventory_items`, `real_name`]
- `requires_feature`: `Work Orders` [`assembly_bom_items`, `real_name`]
- `requires`: `item_replenishment_method_time_phased`
- `plan_shapes`: `monthly:row_per_month_1_column`, `weekly:row_per_week_1_column`, `daily:row_per_week_7_columns`
- `read_only_fields`: `startdate`, `enddate` [`demandplandetail`]
- `default_value`: `current_year_first_to_last_day` [`start_and_end_date`]
- `ui_location`: `item_record_replenishment_method_field` (when `setting_time_phased`)
- `doc_inconsistency`: `garbled_sentence_on_time_phased_requirement`
- `doc_inconsistency`: `samples_use_flat_quantity_field_not_matrix_apis` [`needs_human_review`]
- `doc_inconsistency`: `samples_leave_line_0_uncommitted_and_pass_line_as_string` [`needs_human_review`]
- `doc_inconsistency`: `weekly_and_daily_samples_repeat_monthly_sample` [`needs_human_review`]
- `matrix_apis` (refs `N/record.Record#getCurrentMatrixSublistValue`, `N/record.Record#setCurrentMatrixSublistValue`, `N/record.Record#getMatrixSublistField`, `N/record.Record#getMatrixSublistValue`, `N/record.Record#findMatrixSublistLineWithValue`, `N/currentRecord.CurrentRecord#getCurrentMatrixSublistValue`, `N/currentRecord.CurrentRecord#setCurrentMatrixSublistValue`, `N/currentRecord.CurrentRecord#getMatrixSublistField`, `N/currentRecord.CurrentRecord#getMatrixSublistValue`, `N/currentRecord.CurrentRecord#findMatrixSublistLineWithValue`) [`demandplandetail`]
- `matrix_field`: `quantity` [`demandplandetail`]
- `doc_inconsistency`: `sample_captions_say_2011_code_uses_2023` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `alternatesourceitem` | text | Alternate Source Item | no | no |  | `purpose`: `alternate_item_for_demand_history` |
| `analysisduration` | integer | Historical Analysis Duration | no | no |  | `purpose`: `historical_analysis_periods`; `option_meanings`: `moving_average:periods_for_next_average`, `linear_regression:periods_for_regression`, `seasonal_average:periods_for_future_buckets` |
| `demandplancalendartype` | select | View | no | no |  | `purpose`: `demand_plan_view_range`; `options`: `Daily`, `Weekly`, `Monthly` [`view`]; `help_text_shared_with`: `itemdemandplan.enddate`, `itemdemandplan.month`, `itemdemandplan.startdate`, `itemdemandplan.year`; `allowed_values`: `MONTHLY`, `WEEKLY`, `DAILY`; `caveat`: `values_must_be_upper_case` |
| `enddate` | date | End Date | no | no |  | `purpose`: `demand_plan_view_range`; `options`: `Daily`, `Weekly`, `Monthly` [`view`]; `help_text_shared_with`: `itemdemandplan.demandplancalendartype`, `itemdemandplan.month`, `itemdemandplan.startdate`, `itemdemandplan.year`; `default_value`: `last_day_of_current_year` |
| `item` | select | Item | yes | no |  | `purpose`: `planned_item`; `constraint`: `item_replenishment_method_time_phased` |
| `location` | select | Location | yes | no |  | `purpose`: `location`; `requires_feature`: `Multi-Location Inventory` [`real_name`]; `effect`: `filters_items_to_time_phased_at_location`; `required_when`: `multi_location_inventory` |
| `memo` | text | Memo | no | no |  | `purpose`: `memo`; `supported_in`: `search` |
| `month` | select | Month | no | no |  | `purpose`: `demand_plan_view_range`; `options`: `Daily`, `Weekly`, `Monthly` [`view`]; `help_text_shared_with`: `itemdemandplan.demandplancalendartype`, `itemdemandplan.enddate`, `itemdemandplan.startdate`, `itemdemandplan.year` |
| `projectionduration` | integer | Projection Duration | no | no |  | `purpose`: `projection_periods`; `value_examples`: `6_for_six_months` |
| `projectioninterval` | text | Projection Interval | no | no |  | `purpose`: `projection_interval`; `constraint`: `monthly_only_for_seasonal_average`; `value_examples`: `Weekly` |
| `projectionmethod` | text | Projection Method | no | no |  | `purpose`: `projection_method`; `options`: `Linear Regression`, `Moving Average`, `Seasonal Average`, `Sales Forecast`; `option_meanings`: `linear_regression:steady_linear_growth`, `moving_average:overall_average`, `seasonal_average:repeat_seasonal_trend`, `sales_forecast:forward_sales_forecast`; `constraint`: `seasonal_average_needs_monthly_interval`; `applies_to`: `cash_sales`, `invoices`, `estimates`, `opportunities`, `sales_orders`, `item_fulfillments` (when `sales_forecast`) |
| `projectionstartdate` | date | Projection Start Date | no | no |  | `purpose`: `projection_start_date`; `value_source`: `first_date_of_selected_interval` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `demand_plan_view_range`; `options`: `Daily`, `Weekly`, `Monthly` [`view`]; `help_text_shared_with`: `itemdemandplan.demandplancalendartype`, `itemdemandplan.enddate`, `itemdemandplan.month`, `itemdemandplan.year`; `default_value`: `first_day_of_current_year` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `required_when`: `oneworld` |
| `units` | select | Unit of Measure | no | no |  | `purpose`: `plan_unit_of_measure`; `available_only_when`: `multiple_units_of_measure` |
| `year` | year | Year | no | no |  | `purpose`: `demand_plan_view_range`; `options`: `Daily`, `Weekly`, `Monthly` [`view`]; `help_text_shared_with`: `itemdemandplan.demandplancalendartype`, `itemdemandplan.enddate`, `itemdemandplan.month`, `itemdemandplan.startdate` |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `quantity` |  | Quantity |  |  |  |  |

## Sublist `demandplandetail` (Demand)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `quantity_1_` | posfloat | Quantity | no |  | `matrix_access`: `field_id_quantity_with_column_param` (refs `N/record.Record#setCurrentMatrixSublistValue`, `N/currentRecord.CurrentRecord#setCurrentMatrixSublistValue`) |
| `enddate` | date | End Date | no |  |  |
| `startdate` | date | Start Date | no |  |  |
| `systemcalculated` | float | Monthly Calculated | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `demand` | Demand |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `alternatesourceitem` | select | Last Alternate Source Item |  |
| `analysisduration` | integer | Last Historical Analysis Duration |  |
| `demanddate` | date | Demand Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `lastmodifieddate` | datetime | Last Plan Modified |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `projectionduration` | integer | Last Projection Duration |  |
| `projectioninterval` | select | Last Projection Interval |  |
| `projectionmethod` | select | Last Projection Method |  |
| `projectionstartdate` | date | Last Projection Start Date |  |
| `quantity` | float | Quantity |  |
| `subsidiary` | select | Subsidiary |  |
| `units` | select | Units |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `alternatesourceitem` | select | Last Alternate Source Item |  |
| `analysisduration` | integer | Last Historical Analysis Duration |  |
| `demanddate` | date | Demand Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `lastmodifieddate` | datetime | Last Plan Modified |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `projectionduration` | integer | Last Projection Duration |  |
| `projectioninterval` | text | Last Projection Interval |  |
| `projectionmethod` | text | Last Projection Method |  |
| `projectionstartdate` | date | Last Projection Start Date |  |
| `quantity` | float | Quantity |  |
| `quantityuom` | float | Quantity in Demand Plan Units |  |
| `subsidiary` | select | Subsidiary |  |
| `units` | select | Units |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `location` | Location | Location |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_invoice`
<!-- source-error: record:itemdemandplan | - | functions_sentence_names_invoice -->

> [!SOURCE-ERROR] `garbled_sentence_on_time_phased_requirement`
<!-- source-error: record:itemdemandplan | - | garbled_sentence_on_time_phased_requirement -->

> [!SOURCE-ERROR] `samples_use_flat_quantity_field_not_matrix_apis`
<!-- source-error: record:itemdemandplan | - | samples_use_flat_quantity_field_not_matrix_apis -->

> [!SOURCE-ERROR] `samples_leave_line_0_uncommitted_and_pass_line_as_string`
<!-- source-error: record:itemdemandplan | - | samples_leave_line_0_uncommitted_and_pass_line_as_string -->

> [!SOURCE-ERROR] `weekly_and_daily_samples_repeat_monthly_sample`
<!-- source-error: record:itemdemandplan | - | weekly_and_daily_samples_repeat_monthly_sample -->

> [!SOURCE-ERROR] `sample_captions_say_2011_code_uses_2023`
<!-- source-error: record:itemdemandplan | - | sample_captions_say_2011_code_uses_2023 -->
