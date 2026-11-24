# Analytical Impact (`analyticalimpact`)
<!-- id: record:analyticalimpact -->

| Fact | Value |
|---|---|
| Internal ID | `analyticalimpact` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `SaaS Metrics` [`real_name`]
- `script_functions`: `read`, `update`, `delete`, `search`
- `not_scriptable`: `stored_template`
- `search_only_sublists`: `systemnotes`
- `configured_by`: `enable_features_analytics_subtab` [`saas_metrics`]
- `effect`: `source_records_with_tracked_item_generate_records`
- `can`: `past_start_date_for_historical_reporting`
- `recommendation`: `set_end_date_to_avoid_reporting_errors`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no |  |  | `purpose`: `class_of_source`; `value_source`: `source_record` |
| `datecreated` | datetime | Date Created | no |  |  | `purpose`: `date_created`; `value_source`: `system_recorded` |
| `department` | select | Department | no |  |  | `purpose`: `department_of_source`; `value_source`: `source_record` |
| `enddate` | date | End Date | no |  |  | `purpose`: `end_date_of_source`; `value_source`: `source_record` |
| `entity` | select | Customer | no |  |  | `purpose`: `customer_of_source`; `value_source`: `source_record` |
| `ignore` | checkbox | Ignore | no |  |  | `purpose`: `exclude_from_saas_metrics_report` |
| `item` | select | Item | no |  |  | `purpose`: `item_of_source`; `value_source`: `source_record`; `help_text_shared_with`: `analyticalimpact.sourceline` |
| `lastmodifieddate` | datetime | Last Modified Date | no |  |  | `purpose`: `date_last_modified`; `value_source`: `system_recorded` |
| `location` | select | Location | no |  |  | `purpose`: `location_of_source`; `value_source`: `source_record` |
| `renewedfrom` | select | Renewed From | no |  |  | `purpose`: `original_impact_record_of_renewal`; `constraint`: `original_ends_within_31_days_of_renewal_start`; `shown_only_when`: `stand_alone_source_record_type`; `value_source`: `system_assigned` (when `linked_renewal`); `cannot`: `change` (when `linked_renewal`) |
| `source` | text | Source | no |  |  | `purpose`: `source_record` |
| `sourcecurrency` | select | Source Currency | no |  |  | `purpose`: `currency_of_source`; `value_source`: `source_record` |
| `sourceline` | text | Source Line | no |  |  | `purpose`: `item_of_source`; `value_source`: `source_record`; `help_text_shared_with`: `analyticalimpact.item` |
| `sourcerecordtype` | select | Source Record Type | no |  |  | `purpose`: `record_type_of_source`; `value_source`: `source_record` |
| `sourcestatus` | text | Source Status | no |  |  | `purpose`: `status_of_source_record_or_line`; `value_source`: `source_record` |
| `startdate` | date | Start Date | no |  |  | `purpose`: `start_date_of_source`; `value_source`: `source_record` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `subsidiary_of_source`; `value_source`: `source_record` |
| `terminmonths` | integer | Term In Months | no |  |  | `purpose`: `term_in_months`; `value_source`: `months_between_source_start_and_end` |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `name` | text | ID | no |  |  |  |
| `sourceid` | integer | Source ID | no |  |  |  |

## Sublist `analyticalimpactevent` (Events)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `adjustimpactcategory` | select | Adjust Impact Category | no |  |  |
| `adjustimpactsubcategory` | select | Adjust Impact Subcategory | no |  |  |
| `adjustmonthlyimpactamount` | currency2 | Adjust Monthly Impact Amount | no |  |  |
| `analyticalimpact` | select | Analytical Impact | no |  |  |
| `effectivedate` | date | Effective Date | no |  |  |
| `eventnumber` | integer | Event Number | no |  |  |
| `id` | integer | Internal ID | no |  |  |
| `impactcategory` | select | Impact Category | no |  |  |
| `impactlevel` | text | Impact Level | no | marked_new |  |
| `impactsubcategory` | select | Impact Subcategory | no |  |  |
| `monthlyimpactamount` | currency2 | Monthly Impact Amount | no |  |  |
| `remainingimpactcategory` | select | Remaining Impact Category | no |  |  |
| `remainingimpactsubcategory` | select | Remaining Impact Subcategory | no |  |  |
| `remainingmonthlyimpactamount` | currency2 | Remaining Monthly Impact Amount | no |  |  |

## Sublist `analyticalimpactlink` (Related Records)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `analyticalimpact` | select | Analytical Impact | no |  |  |
| `linkedrecordid` | number | Record ID | no |  |  |
| `linkedrecordname` | text | Source | no |  |  |
| `linkedrecordtype` | text | Type | no |  |  |
| `linkedrecordurl` | text | URL | no |  |  |
| `linkenddate` | date | End Date | no |  |  |
| `linkstartdate` | date | Start Date | no |  |  |
| `linktype` | text | Link Type | no |  |  |
