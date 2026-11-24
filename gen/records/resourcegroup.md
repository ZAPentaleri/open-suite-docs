# Resource Group (`resourcegroup`)
<!-- id: record:resourcegroup -->

| Fact | Value |
|---|---|
| Internal ID | `resourcegroup` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `ui_only_filter_fields`: `subsidiary`, `billingclass`, `class`, `department`, `location`
- `search_only_sublists`: `systemnotes`
- `editable_columns`: `resourcegroupmembers:resource`
- `purpose`: `resources_assigned_together_to_project_task`
- `options`: `divide_estimated_work_evenly`, `assign_full_estimate_to_each`
- `requires_feature`: `Project Management` [`real_name`]
- `read_only_fields`: `subsidiary`, `billingclass`, `unitcost`, `supervisor`, `class`, `department`, `location` [`resourcegroupmembers_columns`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billingclassfilter` | select | Billing Class | no |  |  |  |
| `classfilter` | select | Class | no |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `departmentfilter` | select | Department | no |  |  |  |
| `description` | textarea | Description | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `locationfilter` | select | Location | no |  |  |  |
| `name` | text | Name | yes |  |  |  |
| `origbinactive` | checkbox | Is Inactive (original) | no |  |  |  |
| `subsidiaryfilter` | select | Subsidiary | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Sublist `resourcegroupmembers` (Resources)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `group` | select |  | no |  |  |
| `resource` | select | Resource | yes |  |  |
| `unitcost` | currency | Unit Cost | no |  |  |
