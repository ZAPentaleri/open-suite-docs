# Term (`term`)
<!-- id: record:term -->

| Fact | Value |
|---|---|
| Internal ID | `term` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `no_search` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `search`
- `script_functions`: `create`, `update`, `copy`, `delete`
- `not_supported`: `search`
- `purpose`: `invoice_payment_due_terms`
- `ui_location`: `setup_accounting_accounting_lists_new_term`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `datedriven` | radio | STANDARD | no | no |  | `purpose`: `date_driven_terms`; `options`: `Standard`, `Date Driven`; `option_meanings`: `standard:days_after_invoice`, `date_driven:calendar_day_of_month` |
| `daydiscountexpires` | integer | Day Discount Expires | no | no |  | `purpose`: `discount_expiry_day_of_month`; `applies_when`: `date_driven_terms`; `value_examples`: `20` |
| `dayofmonthnetdue` | integer | Day Of Month Net Due | no | no |  | `purpose`: `net_due_day_of_month`; `applies_when`: `date_driven_terms`; `default_behavior`: `last_day_of_month` (when `day_not_in_month`) |
| `daysuntilexpiry` | integer | Days Till Discount Expires | no | no |  | `purpose`: `discount_days`; `applies_when`: `standard_terms`; `value_examples`: `15` |
| `daysuntilnetdue` | integer | Days Till Net Due | no | no |  | `purpose`: `days_until_net_due`; `value_examples`: `30_for_net_30` |
| `discountpercent` | posfloat | % Discount | no | no |  | `purpose`: `early_payment_discount_percent`; `applies_when`: `standard_terms` |
| `discountpercentdatedriven` | posfloat | % Discount | no | no |  | `purpose`: `early_payment_discount_percent`; `applies_when`: `date_driven_terms` |
| `duenextmonthifwithindays` | integer | Due Next Month If Within Days | no | no |  | `purpose`: `due_next_month_if_within_days`; `applies_when`: `date_driven_terms`; `value_examples`: `due_30th_within_5_moves_25th_to_30th_into_next_month`; `doc_inconsistency`: `example_table_last_row_says_1_5` |
| `installment` | checkbox | Installment | no | no |  | `purpose`: `installment_terms`; `effect`: `shipping_and_tax_in_first_installment`; `effect`: `remaining_balance_split_equally` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Terms | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `preferred` | checkbox | Preferred | no | no |  | `purpose`: `preferred_term_or_message`; `true_means`: `default_on_sales_transactions`; `caveat`: `customer_terms_override_preferred` |
| `recurrencecount` | integer | Recurrence Count | no | no |  | `purpose`: `number_of_payments`; `value_examples`: `3` |
| `recurrencefrequency` | select | Recurrence Frequency | no | no |  | `purpose`: `payment_frequency`; `allowed_values`: `Monthly`, `Annually`, `Weekly`, `Daily` |
| `repeatevery` | posinteger | Repeat Every | no | no |  | `purpose`: `repeat_every`; `value_examples`: `1_monthly`, `2_with_annual_for_biannual` |
| `splitevenly` | checkbox | Split Evenly | no | no |  | `purpose`: `split_evenly`; `caveat`: `first_payment_adds_tax_and_shipping` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `percentages` (Installments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `percentage` | percent | Percentage | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `datedriven` | checkbox | Date Driven |  |
| `daydiscountexpires` | integer | Day Discount Expires |  |
| `dayofmonthnetdue` | integer | Day of Month Net Due |  |
| `daysuntilexpiry` | integer | Days Till Discount Expires |  |
| `daysuntilnetdue` | integer | Days Till Net Due |  |
| `discountpercent` | float | % Discount |  |
| `discountpercentdatedriven` | float | Date Driven % Discount |  |
| `duenextmonthifwithindays` | integer | Due Next Month if Within Days |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `installment` | checkbox | Installment |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `preferred` | checkbox | Preferred |  |
| `recurrencecount` | integer | Recurrence Count |  |
| `recurrencefrequency` | select | Recurrence Frequency |  |
| `repeatevery` | integer | Repeat Every |  |
| `splitevenly` | checkbox | Split Evenly |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `datedriven` | checkbox | Date Driven |  |
| `daydiscountexpires` | integer | Day Discount Expires |  |
| `dayofmonthnetdue` | integer | Day of Month Net Due |  |
| `daysuntilexpiry` | integer | Days Till Discount Expires |  |
| `daysuntilnetdue` | integer | Days Till Net Due |  |
| `discountpercent` | float | % Discount |  |
| `discountpercentdatedriven` | float | Date Driven % Discount |  |
| `duenextmonthifwithindays` | integer | Due Next Month if Within Days |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `installment` | checkbox | Installment |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `preferred` | checkbox | Preferred |  |
| `recurrencecount` | integer | Recurrence Count |  |
| `recurrencefrequency` | select | Recurrence Frequency |  |
| `repeatevery` | integer | Repeat Every |  |
| `splitevenly` | checkbox | Split Evenly |  |

## Suspected source errors

> [!SOURCE-ERROR] `example_table_last_row_says_1_5` (on `duenextmonthifwithindays`)
<!-- source-error: record:term | duenextmonthifwithindays | example_table_last_row_says_1_5 -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:term | isinactive | says_removes_all_references -->
