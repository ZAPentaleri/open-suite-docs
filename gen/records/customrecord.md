# Custom Record (`customrecord`)
<!-- id: record:customrecord -->

| Fact | Value |
|---|---|
| Internal ID | `customrecord` |
| Tags | `type_template` |

## Record facts

- `purpose`: `business_specific_data_in_user_defined_record_types`
- `cannot`: `create_or_alter_custom_record_types_via_script` [`types_defined_in_ui`]
- `can`: `create_instances`, `update_instances`, `copy_instances`, `delete_instances`, `search_instances`
- `type_id_prefix`: `customrecord` [`system_ids_end_with_number`]
- `field_id_prefix`: `custrecord`
- `type_id_found_at`: `customization_lists_records_fields_record_types`
- `set_type_with`: `custom_record_type_string_id` (refs `N/record.Record#type`, `N/currentRecord.CurrentRecord#type`)
- `difference_from`: `standard_types_use_record_type_enum` (refs `N/record.Type`)
- `scriptable_in`: `client`, `server`
- `user_event_entry_points` (refs `script:UserEventScript.beforeLoad`, `script:UserEventScript.beforeSubmit`, `script:UserEventScript.afterSubmit`)
- `modules` (refs `N/record`, `N/currentRecord`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `created` | datetime | Date Created | no | no |  | `purpose`: `date_created`; `doc_inconsistency`: `custom_record_help_describes_term_record` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Name | yes | no |  | `purpose`: `record_name`; `doc_inconsistency`: `custom_record_help_describes_term_record` |
| `owner` | select | Owner | yes | no |  | `purpose`: `record_creator` |
| `altname` | text | Name | yes | no |  |  |
| `autoname` | checkbox | Auto | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodified` | datetime | Last Modified | no | no |  |  |
| `lastmodifiedby` | select | By | no | no |  |  |
| `linenumber` | integer |  | no | no |  |  |
| `recordid` | integer | ID | no | no |  |  |
| `rectype` | select |  | no | no |  |  |

## Sublist `translations` (Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `label` | text | Name | no |  |  |
| `language` | text | Language | no |  |  |
| `locale` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `media` | File Cabinet |
| `merge` | Mail Merge |
| `notes` | Notes |
| `translation` | Translation |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `availableoffline` | checkbox | Always Available Offline |  |
| `created` | datetime | Date Created |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `lastmodified` | datetime | Last Modified |  |
| `lastmodifiedby` | select | Last Modified By |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `parent` | select | Parent |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `altname` | text | Name |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `created` | datetime | Date Created |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `lastmodified` | datetime | Last Modified |  |
| `lastmodifiedby` | select | Last Modified By |  |
| `lastviewed` | datetime | Last Viewed |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `parent` | select | Parent |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `messages` | Messages | Message |
| `owner` | Owner | Employee |
| `parent` | Parent | Custom |

## Suspected source errors

> [!SOURCE-ERROR] `custom_record_help_describes_term_record` (on `created`)
<!-- source-error: record:customrecord | created | custom_record_help_describes_term_record -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:customrecord | isinactive | says_removes_all_references -->

> [!SOURCE-ERROR] `custom_record_help_describes_term_record` (on `name`)
<!-- source-error: record:customrecord | name | custom_record_help_describes_term_record -->
