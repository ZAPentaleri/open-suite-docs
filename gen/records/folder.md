# Folder (`folder`)
<!-- id: record:folder -->

| Fact | Value |
|---|---|
| Internal ID | `folder` |
| Category | file_cabinet |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full`, `ue_before_load_unsupported` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_event_not_supported`: `beforeLoad`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Restrict by Class | no | no |  | `purpose`: `restrict_folder_by_class`; `allowed_values_listed_in`: `class_records`; `effect`: `folder_contents_visible_only_to_matching_users` |
| `department` | select | Restrict by Department | no | no |  | `purpose`: `restrict_folder_by_department`; `allowed_values_listed_in`: `department_records`; `effect`: `folder_contents_visible_only_to_matching_users` |
| `description` | textarea | Description | no | no |  | `purpose`: `description`; `displayed_on`: `this_record_only` |
| `foldertype` | select | Type | no | no |  | `purpose`: `folder_type`; `value_source`: `set_from_parent_folder`; `options`: `Documents and Files`, `SuiteCommerce Advanced Site Templates`; `constraint`: `suitescript_files_in_suitescripts_folder` |
| `group` | select | Restrict by Group | no | no |  | `purpose`: `restrict_folder_by_group`; `effect`: `only_group_members_view_contents`; `allowed_values_listed_in`: `group_records` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_folder`; `ui_effect`: `hidden_from_lists_unless_show_inactives` |
| `isprivate` | checkbox | Private | no | no |  | `purpose`: `private_folder`; `true_means`: `contents_visible_only_to_owner` |
| `location` | select | Restrict by Location | no | no |  | `purpose`: `restrict_folder_by_location`; `allowed_values_listed_in`: `location_records`; `effect`: `folder_contents_visible_only_to_matching_users` |
| `name` | text | Folder Name | yes | no |  | `purpose`: `folder_name`; `value_examples`: `Human Resources` |
| `owner` | select | Owner | no | no |  | `purpose`: `file_owner`; `shown_only_when`: `folder_private` |
| `parent` | select | Sub-Folder of | no | no |  | `purpose`: `parent_folder`; `value_examples`: `Human Resources` |
| `subsidiary` | select | Restrict by Subsidiary | no | no |  | `purpose`: `restrict_folder_by_subsidiary`; `caveat`: `parent_subsidiary_employees_see_child_folders` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `class` | select | Class |  |
| `department` | select | Department |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `group` | select | Group |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `istoplevel` | checkbox | Is Top Level |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `numfiles` | integer | # of Files |  |
| `owner` | select | Owner |  |
| `parent` | select | Sub of |  |
| `predecessor` | select | Predecessor |  |
| `private` | checkbox | Is Private |  |
| `size` | integer | Size (KB) |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `class` | select | Class |  |
| `department` | select | Department |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `foldersize` | integer | Size (KB) |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `group` | select | Group |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `numfiles` | integer | # of Files |  |
| `owner` | select | Owner |  |
| `parent` | select | Sub of |  |
| `subsidiary` | select | Subsidiary |  |
