# Accounting Period (`accountingperiod`)
<!-- id: record:accountingperiod -->

| Fact | Value |
|---|---|
| Internal ID | `accountingperiod` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_read`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Accounting Periods` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `search`, `read`
- `ui_location`: `setup_accounting_manage_accounting_periods`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allownonglchanges` | checkbox | Allow Non-G/L Changes | no | no |  | `purpose`: `allow_non_gl_changes_after_close`; `requires_permission`: `allow_non_gl_changes` |
| `enddate` | date | End Date | yes | no |  | `purpose`: `period_end_date` |
| `fiscalcalendar` | select | Fiscal Calendar | yes | no |  | `purpose`: `fiscal_calendar_of_period` |
| `isadjust` | checkbox | Period is Adjustment | no | no |  | `purpose`: `marks_adjustment_period`; `constraint`: `only_journal_entries_in_adjustment_periods`; `not_displayed_on`: `posting_period_of_other_transactions`; `use_cases`: `special_tax_adjustments`, `year_end_impairment`, `management_reporting` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `doc_inconsistency`: `help_text_describes_numbering_sequence` |
| `isquarter` | checkbox |  | no | no |  | `purpose`: `marks_quarter_period` |
| `isyear` | checkbox |  | no | no |  | `purpose`: `marks_year_period` |
| `parent` | select | Sub-period of | yes | no |  | `purpose`: `parent_period`; `caveat`: `soap_returns_it_only_without_multiple_calendars`; `use_instead`: `accountingperiodfiscalcalendars_parent` (when `multiple_calendars`); `value_examples`: `year_2022_parent_of_first_quarter` |
| `periodname` | text | Period Name | yes | no |  | `purpose`: `period_name`; `constraint`: `unique_within_fiscal_calendar`; `constraint`: `base_period_names_unique_across_calendars`; `can`: `select_open_period_from_gl_audit_numbering_setup` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `period_start_date` |
| `alllocked` | checkbox | All Closed | no | no |  |  |
| `aplocked` | checkbox | A/P Closed | no | no |  |  |
| `arlocked` | checkbox | A/R Closed | no | no |  |  |
| `closed` | checkbox | All Closed | no | no |  |  |
| `closedondate` | date | Date Closed | no | no |  |  |
| `isposting` | checkbox | Is Posting | no | no |  |  |
| `payrolllocked` | checkbox | Payroll Closed | no | no |  |  |

## Sublist `fiscalcalendars` (Parent Periods)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `fiscalcalendar` | select | Fiscal Calendar | no |  |  |
| `parent` | select | Sub-period of | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_text_describes_numbering_sequence` (on `isinactive`)
<!-- source-error: record:accountingperiod | isinactive | help_text_describes_numbering_sequence -->
