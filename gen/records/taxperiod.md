# Tax Period (`taxperiod`)
<!-- id: record:taxperiod -->

| Fact | Value |
|---|---|
| Internal ID | `taxperiod` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `allowed_values`: `month`, `quarter`, `year` [`period_length`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `enddate` | date | End Date | yes | no |  | `purpose`: `tax_period_end_date`; `format`: `dd.mm.yyyy` |
| `periodname` | text | Period Name | yes | no |  | `purpose`: `tax_period_name` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `tax_period_start_date`; `format`: `dd.mm.yyyy` |
| `allclosed` | checkbox | Closed | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isadjust` | checkbox | Period is Adjustment | no | no |  |  |
| `isquarter` | checkbox |  | no | no |  |  |
| `isyear` | checkbox |  | no | no |  |  |

## Sublist `fiscalcalendars` (Parent Periods)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `fiscalcalendar` | select | Fiscal Calendar | no |  |  |
| `parent` | select | Sub-period of | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `parents` | Fiscal Calendars |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `allclosed` | checkbox | All Closed |  |
| `alllocked` | checkbox | All Locked |  |
| `aplocked` | checkbox | AP Locked |  |
| `arlocked` | checkbox | AR Locked |  |
| `closed` | checkbox | Closed |  |
| `closedondate` | date | Date Closed On |  |
| `enddate` | date | End Date |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isadjust` | checkbox | Adjust |  |
| `isinactive` | checkbox | Inactive |  |
| `isquarter` | checkbox | Quarter |  |
| `isyear` | checkbox | Year |  |
| `parent` | select | Sub-Period Of |  |
| `payrolllocked` | checkbox | Payroll Locked |  |
| `periodname` | text | Period Name |  |
| `startdate` | date | Start Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `allclosed` | checkbox | All Closed |  |
| `alllocked` | checkbox | All Locked |  |
| `aplocked` | checkbox | AP Locked |  |
| `arlocked` | checkbox | AR Locked |  |
| `closed` | checkbox | Closed |  |
| `closedondate` | date | Date Closed On |  |
| `enddate` | date | End Date |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isadjust` | checkbox | Adjust |  |
| `isinactive` | checkbox | Inactive |  |
| `isquarter` | checkbox | Quarter |  |
| `isyear` | checkbox | Year |  |
| `parent` | select | Parent |  |
| `payrolllocked` | checkbox | Payroll Locked |  |
| `periodname` | text | Name |  |
| `startdate` | date | Start Date |  |
