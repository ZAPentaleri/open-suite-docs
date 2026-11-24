# Supply Change Order (`supplychangeorder`)
<!-- id: record:supplychangeorder -->

| Fact | Value |
|---|---|
| Internal ID | `supplychangeorder` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_create`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `script_functions`: `read`, `update`, `delete`, `search`
- `not_supported`: `create`, `copy`, `transform`
- `record_actions`: `approve`, `reject`
- `doc_inconsistency`: `sample_says_create`
- `value_source`: `created_by_reacting_to_workbench_action_message`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`material_requirements_planning`]
- `record_actions`: `approve:approve_all_pending`, `reject:reject_all_pending_optional_note`
- `raises_error`: `action_exception` (when `transaction_line_update_fails`)
- `parameters`: `record_id_only`
- `ui_location`: `approve_all_pending_and_reject_all_pending_buttons`
- `effect`: `approve_updates_transaction_line`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approvalstatus` | select | Approval Status | yes |  |  |  |
| `changeaction` | select | Change Action | yes |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `memo` | text | Memo | no |  |  |  |
| `newdate` | date | New Date | no |  |  |  |
| `nextapprover` | select | Next Approver | no |  |  |  |
| `originaldate` | date | Original Date | yes |  |  |  |
| `supplyplandefinition` | select | Supply Plan Definition | no |  |  |  |
| `transactionlineid` | text | Transaction Line | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `approvalstatus` | select | Approval Status |  |
| `changeaction` | select | Change Action |  |
| `currenttransactiondate` | date | Current Transaction Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `newdate` | date | New Date |  |
| `nextapprover` | select | Next Approver |  |
| `originaldate` | date | Original Date |  |
| `sourcelocation` | select | Source Location |  |
| `status` | select | Status |  |
| `supplyplandefinition` | select | Supply Plan Definition |  |
| `transaction` | select | Transaction |  |
| `transactiontype` | select | Transaction Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `approvalstatus` | select | Approval Status |  |
| `changeaction` | select | Change Action |  |
| `currenttransactiondate` | date | Current Transaction Date |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `newdate` | date | New Date |  |
| `nextapprover` | select | Next Approver |  |
| `originaldate` | date | Original Date |  |
| `sourcelocation` | select | Source Location |  |
| `status` | select | Status |  |
| `supplyplandefinition` | select | Supply Plan Definition |  |
| `transaction` | select | Transaction |  |
| `transactiontype` | select | Transaction Type |  |

## Suspected source errors

> [!SOURCE-ERROR] `sample_says_create`
<!-- source-error: record:supplychangeorder | - | sample_says_create -->
