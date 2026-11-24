# SuiteScript 1.0 user event scripts
<!-- id: topic:ss1_api.user_events -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is
> [UserEventScript](../script-types/UserEventScript.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## Event functions

A 1.0 user event script binds up to three functions on its script record: `beforeLoad`,
`beforeSubmit` and `afterSubmit`. They fire for records handled through the UI, web services,
other server scripts, CSV import and XML requests.

| Function | When | Arguments |
|---|---|---|
| `beforeLoad` | Before the record is shown or returned | `type`, `form`, `request` |
| `beforeSubmit` | Before the record is written; the data is not yet committed | `type` |
| `afterSubmit` | After the record is written; the data is already committed | `type` |

- `request` is only passed to `beforeLoad` for requests coming from a browser.
- `afterSubmit` runs asynchronously only for web store checkout.

## The `type` argument

NetSuite supplies `type`; it is not a deployment parameter. Always branch on it, because a single
function serves many operations. If the deployment restricts the event type, that setting wins
over whatever checks the script makes, because the script is not called for other types.

| `type` value | `beforeLoad` | `beforeSubmit` | `afterSubmit` |
|---|---|---|---|
| `create` | Yes | Yes | Yes |
| `edit` | Yes | Yes | Yes |
| `view` | Yes | | |
| `copy` | Yes | | |
| `print` | Yes | | |
| `email` | Yes | | |
| `quickview` | Yes | | |
| `delete` | | Yes | Yes |
| `xedit` | | Yes | Yes |
| `approve` | | Yes | Yes |
| `reject` | | Yes | Yes |
| `cancel` | | Yes | Yes |
| `pack` | | Yes | Yes |
| `ship` | | Yes | Yes |
| `markcomplete` | | Yes | |
| `reassign` | | Yes | |
| `editforecast` | | Yes | |
| `dropship` | | | Yes |
| `specialorder` | | | Yes |
| `orderitems` | | | Yes |
| `paybills` | | | Yes |

- `approve`, `reject` and `cancel` only occur on record types that support those actions.
- `xedit` covers inline edits and mass updates; see [inline editing](ss1_api.inline_editing.md).
- Attaching or detaching a child record fires the parent's events with type `edit`.
- For `paybills`, the record carries no sublist data; search for the bills instead.

## What triggers beforeLoad

- Navigating to the record in the UI.
- A web services read.
- Attaching or detaching a child record.
- [`nlapiLoadRecord`](../ss1/records.md) in another script.

`beforeLoad` does not fire for online forms. Standard records cannot be sourced from `beforeLoad`;
do that in a client `pageInit` instead.

## Changing records safely

- Change the current record in `beforeSubmit`, not in `afterSubmit`; by `afterSubmit` the data is
  already saved.
- In `beforeSubmit`, never load or submit the record being saved; doing so can lose data.
- To change the record after it is saved, load it with `nlapiLoadRecord` in `afterSubmit`, change
  it, and submit it.
- Messages that arrive as inbound email skip `beforeSubmit`.
- Scripts cannot override the permissions set on custom fields, even when running as
  administrator.

Read the record being processed with [`nlapiGetNewRecord`](../ss1/records.md) and its previous
state with [`nlapiGetOldRecord`](../ss1/records.md).

> [!SOURCE-ERROR] The source refers to a function `nlapiGetRecord` [sic], which is not part of the
> documented 1.0 API. The spec does not say which function was meant; the documented ways to reach
> the record in a user event are `nlapiGetNewRecord` and `nlapiGetOldRecord`.
<!-- source-error: topic:ss1_api.user_events | - | names_nonexistent_api_nlapigetrecord -->

## Converting

In 2.1 the three functions become entry points of
[UserEventScript](../script-types/UserEventScript.md); `type`, `form` and `request` arrive on the
context object, and `beforeLoad` has no old record. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

A `beforeSubmit` function that stamps the date on which a support case was first set to priority
1. It branches on `type`, changes only the record being saved, and never loads or submits it.
Inline edits are left out on purpose: during `xedit` unchanged fields read as `null`, so the
"already stamped" test would be unreliable. The custom field ID is illustrative.

```js
function stampEscalation(type) {
  if (type !== 'create' && type !== 'edit') {
    return;
  }
  var rec = nlapiGetNewRecord();
  var priority = rec.getFieldValue('priority');
  if (priority === '1' && !rec.getFieldValue('custevent_escalated_on')) {
    rec.setFieldValue('custevent_escalated_on', nlapiDateToString(new Date()));
  }
}
```
