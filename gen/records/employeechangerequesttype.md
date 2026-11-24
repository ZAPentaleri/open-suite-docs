# Employee Change Request Type (`employeechangerequesttype`)
<!-- id: record:employeechangerequesttype -->

| Fact | Value |
|---|---|
| Internal ID | `employeechangerequesttype` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `delete`, `search`
- `requires_feature`: `Employee Change Requests`, `Effective Dating`, `Advanced Employee Permissions` [`real_name`]
- `not_supported`: `copy`, `transform`
- `availability`: `limited_accounts`
- `purpose`: `change_types_managers_can_request`
- `configured_by`: `enable_features_employees_subtab`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `changereason` | select | Change Reason | yes |  |  | `purpose`: `change_reason`; `effect`: `used_for_effective_dated_change_after_approval` |
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_request_type`; `ui_effect`: `hidden_from_list_unless_show_inactives` |
| `name` | text | Name | yes |  |  | `purpose`: `request_type_name` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `origbinactive` | checkbox |  | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `changereason` | select | Change Reason |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `changereason` | select | Change Reason |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
