# Employee Change Request (`employeechangerequest`)
<!-- id: record:employeechangerequest -->

| Fact | Value |
|---|---|
| Internal ID | `employeechangerequest` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Employee Change Requests`, `Effective Dating`, `Advanced Employee Permissions` [`real_name`]
- `availability`: `limited_accounts`
- `purpose`: `manager_initiated_employee_change_with_approval`
- `configured_by`: `enable_features_employees_subtab`
- `requires_feature`: `SuiteFlow` (when `approval_workflow`) [`real_name`]
- `applies_to`: `direct_reports_via_employee_center`
- `effect`: `approved_change_applied_on_proposed_date`
- `script_functions`: `read`, `create`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `editable_only_when`: `creating` [`requester`, `employee`, `requested_fields`]
- `format`: `company_date_format_preference` [`proposeddate`]
- `requires`: `change_request_type_assigned_to_change_reason`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `decliningreason` | textarea | Reason | no |  |  | `purpose`: `declining_reason` |
| `employeechangerequesttype` | select | Change Request Type | yes |  |  | `purpose`: `change_request_type`; `allowed_values_listed_in`: `employee_change_request_types` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_change_request`; `ui_effect`: `hidden_from_list_unless_show_inactives` |
| `justification` | textarea | Justification | yes |  |  | `purpose`: `justification`; `required_when`: `manager_creates_request` |
| `nextapprover` | select | Next Approver | no |  |  | `purpose`: `next_approver`; `requires_permission`: `edit_employee_change_requests` (when `changing_approver`) |
| `proposeddate` | date | Proposed Date | yes |  |  | `purpose`: `proposed_effective_date`; `constraint`: `future_date`; `effect`: `request_expires_if_not_approved_by_date` |
| `requester` | select | Requester | no |  |  | `purpose`: `requester` |
| `requeststatus` | select | Request Status | yes |  |  | `purpose`: `request_status`; `requires`: `decliningreason` (when `declined`) |
| `declinedby` | select | Declined By | no |  |  |  |
| `employee` | select | Employee | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `origbinactive` | checkbox |  | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `declinedby` | select | Declined By |  |
| `decliningreason` | textarea | Reason |  |
| `employee` | select | Employee |  |
| `employeechangerequesttype` | select | Employee Change Request Type |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `justification` | textarea | Justification |  |
| `nextapprover` | select | Next Approver |  |
| `proposeddate` | date | Proposed Date |  |
| `requester` | select | Requester |  |
| `requeststatus` | select | Request Status |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `declinedby` | select | Declined By |  |
| `decliningreason` | textarea | Reason |  |
| `employee` | select | Employee |  |
| `employeechangerequesttype` | select | Employee Change Request Type |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `justification` | textarea | Justification |  |
| `nextapprover` | select | Next Approver |  |
| `proposeddate` | date | Proposed Date |  |
| `requester` | select | Requester |  |
| `requeststatus` | select | Request Status |  |
