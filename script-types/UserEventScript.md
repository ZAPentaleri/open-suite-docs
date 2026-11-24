# UserEventScript
<!-- id: script:UserEventScript -->

A user event script is server-side code attached to a record type. It reacts to things that happen
to individual records: a record being opened (read), and a record being saved (before and after
the database write). Use it for checks and rules that must hold however a record gets changed,
for adjusting the form a user sees, and for follow-up work once data is saved.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `UserEventScript` (the annotation value is not case-sensitive, but write it in PascalCase) |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server |
| Features that must be enabled | "Server SuiteScript" and "Client SuiteScript" (both) |
| SuiteScript 1.0 counterpart | User Event Scripts ([1.0 concepts](../topics/ss1_api.user_events.md)) |
| Entry points | [`beforeLoad`](#beforeloadcontext), [`beforeSubmit`](#beforesubmitcontext), [`afterSubmit`](#aftersubmitcontext) |
| Enums | [`UserEventType`](#usereventtype) |

Related pages: [record modes](../topics/record_modes.md), [script deployment](../topics/script_deployment.md),
[UI setup for user events](../topics/user_event_script.ui_setup.md), [SDF object](../topics/sdf.usereventscript.md),
[N/record](../modules/N_record.md), [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
[ClientScript](ClientScript.md).

## How it runs

**What starts it.** The script fires on record actions: create, load, update, copy, delete and
submit. In terms of timing there are three moments: when a record is being opened (before it is
handed to the caller), right after the user clicks Save but before the database write, and right
after the write has been committed.

**Who can cause it.** Writes that reach a record from the UI, SOAP or REST web services, server-side
SuiteScript, CSV import or XML all run the save-time entry points. Record work done by
[scheduled scripts](ScheduledScript.md), [portlets](Portlet.md) and [Suitelets](Suitelet.md) does
trigger user event scripts, and so do web-service calls and CSV imports. Two sources never trigger
them: another user event script, and a workflow running in a user event context. So a record saved
from inside your `afterSubmit` will not fire user event scripts on that second record.

**The save sequence.** For a submit, the order is: permission check, then `beforeSubmit` (where you
validate or adjust), then the database save, then `afterSubmit`, which sees the saved data.
`beforeSubmit` always runs before anything is stored.

**Synchronous by default.** `afterSubmit` normally runs in line with the save. The one exception is
web store checkout, where after-submit processing runs asynchronously.

**Ordering.** When several user event scripts are deployed to one record type, their order is set on
the Scripted Records page.

**Where it can be deployed.** Most standard record types and all custom record types support user
event scripts. Personal identification records (for example driver's license and passport records),
some revenue recognition records and some timecard records do not.

**Required features.** The account needs both the "Server SuiteScript" feature and the
"Client SuiteScript" feature enabled.

**Managing scripts.** User event scripts can be managed with the SuiteCloud Development Framework,
copied between accounts with Copy to Account (its control sits at the top right of the script
page), and reviewed with SuiteScript Analysis.

**Typical jobs.** Customizing an entry form or workflow, processing data before it is stored,
validating against other data, enforcing data integrity and business rules, permission checks and
record restrictions, keeping an external system in sync in near real time, redirecting the user or
creating follow-up work (for instance scheduling a follow-up phone call whenever a new customer is
saved), and tailoring forms.

**A setup gotcha.** If a script depends on a customer's sales rep, note that customers using team
selling usually have no value in the sales rep field. An employee is marked as a sales rep on the
employee record, in the Sales Rep box of the Human Resources subtab.

## Entry points

### beforeLoad(context)
<!-- id: script:UserEventScript.beforeLoad -->

Runs whenever a record is read and before it is returned to whoever asked for it: after the
permission check and the load, but before the data reaches the client. Reads that trigger it
include navigating to the record in the UI, a SOAP or REST web-service read, a CSV import, and a
record load done by server-side SuiteScript (other than a user event script).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.form` | serverWidget.Form | - | |
| `context.newRecord` | record.Record | - | |
| `context.request` | http.ServerRequest | - | |
| `context.type` | string | - | |

All four context properties exist since 2015.2.

- `context.form` is the form about to be shown. Its main use is shaping the page, for example
  disabling a field before the page loads so it appears dimmed and cannot be edited (see
  [N/ui/serverWidget](../modules/N_ui_serverWidget.md)).
- `context.newRecord` is the record being loaded.
- `context.request` is not present when the load was triggered on the server rather than by a
  browser request, so check for it before using it.
- `context.type` tells you why the record is being loaded. The event types that reach `beforeLoad`
  are create, edit, view, copy, print, email and quick view. Branch on this value instead of
  assuming the user is editing.

**Trigger types:** `CREATE`, `EDIT`, `VIEW`, `COPY`, `PRINT`, `EMAIL`, `QUICKVIEW`

**Returns:** `void`

Behaviour to keep in mind:

- Changes you make to the record being loaded are ignored; `beforeLoad` is for the presentation,
  not for changing stored data. It can, however, create other records and update the records it
  created.
- It cannot source standard records. If you need sourcing to happen, do it in a client script's
  [`pageInit`](ClientScript.md#pageinitcontext) instead.
- It is not triggered by online customer forms, online custom record forms or online case forms.
- Attaching or detaching a child custom record counts as an edit event.

### beforeSubmit(context)
<!-- id: script:UserEventScript.beforeSubmit -->

Runs after the user saves and before the record is written to the database. Changes made here
persist: anything you set on `context.newRecord` is saved along with the record. That makes this
the place for validation, restrictions and permission checks, and last-minute adjustments.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.newRecord` | record.Record | - | |
| `context.oldRecord` | record.Record | - | |
| `context.type` | string | - | |

All three context properties exist since 2015.2.

- `context.newRecord`: set values on it with [`Record.setValue`](../modules/N_record.md); they are
  stored with the record.
- `context.oldRecord`: the record as it was before this save.
- There is no `context.form`; the form is not available during a save.
- The records in the context are in standard (not dynamic) mode. See [record modes](../topics/record_modes.md).

**Trigger types:** `CREATE`, `EDIT`, `XEDIT`, `DELETE`, `APPROVE`, `CANCEL`, `REJECT`, `PACK`, `SHIP`,
`MARKCOMPLETE`, `REASSIGN`, `EDITFORECAST`

**Returns:** `void`

Which records some trigger types apply to:

| Trigger type | Applies to |
|---|---|
| `APPROVE`, `CANCEL`, `REJECT` | Sales orders, expense reports, time bills, purchase orders and return authorizations (see the note below) |
| `PACK`, `SHIP` | Certain record types, for example item fulfillments |
| `MARKCOMPLETE` | Calls and tasks |
| `REASSIGN` | The case "grab" action |
| `EDITFORECAST` | Opportunities or estimates edited in the forecast editor |

The source says `APPROVE`, `CANCEL` and `REJECT` are limited to the record types listed, but it
also calls that list examples only. Treat the list as indicative rather than complete, and test the
record type you deploy to.

Behaviour to keep in mind:

- For `XEDIT` (inline edit), the new record carries only the fields that were edited, not the full
  record.
- It does not run for message records created by inbound email case capture.
- A user event script cannot get around custom field permissions, even when it runs as
  administrator.
- Attaching or detaching a child custom record counts as an edit event.

### afterSubmit(context)
<!-- id: script:UserEventScript.afterSubmit -->

Runs immediately after the record has been written. The saved values are final at this point, so
use it for consequences of the save: creating related or child records from the submitted values,
sending email about new data, assigning tasks, redirecting the browser, or pushing changes to an
external system.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.newRecord` | record.Record | - | |
| `context.oldRecord` | record.Record \| null | - | |
| `context.type` | string | - | |

All three context properties exist since 2015.2.

- `context.newRecord` is read-only here. To change the saved record, load it, change it and save
  it again (and remember that this second save will not fire user event scripts).
- `context.oldRecord` is read-only, and empty on create because there was no previous version.

**Trigger types:** `CREATE`, `EDIT`, `XEDIT`, `DELETE`, `APPROVE`, `CANCEL`, `REJECT`, `PACK`, `SHIP`,
`DROPSHIP`, `SPECIALORDER`, `ORDERITEMS`, `PAYBILLS`

**Returns:** `void`

Which records some trigger types apply to:

| Trigger type | Applies to |
|---|---|
| `APPROVE`, `CANCEL`, `REJECT` | Only some record types: sales orders, expense reports, time bills, purchase orders and return authorizations |
| `PACK`, `SHIP` | Item fulfillments |
| `DROPSHIP` | Purchase orders for drop-ship items |
| `SPECIALORDER` | Purchase orders for special-order items |
| `ORDERITEMS` | Purchase orders created through Order Items |
| `PAYBILLS` | Vendor payments made through the Pay Bills page |

Behaviour to keep in mind:

- For `PAYBILLS`, the record has no sublist line data. Get line details with a search or a field
  lookup instead.
- For `XEDIT`, only the edited fields are present.
- During web store checkout this entry point runs asynchronously.
- Attaching or detaching a child custom record counts as an edit event.

## Enums

### UserEventType
<!-- id: script:UserEventScript.UserEventType -->

The set of event types that `context.type` can hold in all three entry points. It is a plain object
whose properties are read-only strings, and a script reaches it through the context object as
`context.UserEventType` (so a check reads `context.type === context.UserEventType.EDIT`).

| Value | Notes |
|---|---|
| `APPROVE` | |
| `CANCEL` | |
| `CHANGEPASSWORD` | Fires when a user changes their password in the UI; only visible in the account where the password was changed |
| `COPY` | |
| `CREATE` | |
| `DELETE` | |
| `DROPSHIP` | |
| `EDIT` | Also raised when a child custom record is attached or detached |
| `EDITFORECAST` | |
| `EMAIL` | |
| `MARKCOMPLETE` | |
| `ORDERITEMS` | |
| `PACK` | |
| `PAYBILLS` | |
| `PRINT` | |
| `QUICKVIEW` | |
| `REASSIGN` | |
| `REJECT` | |
| `SHIP` | |
| `SPECIALORDER` | |
| `TRANSFORM` | |
| `VIEW` | |
| `XEDIT` | |

The source names the members only; it does not state the string value each member holds. Compare
`context.type` with the enum members rather than with hard-coded strings. `UserEventType` is not
among the context properties listed for the individual entry points above, even though the source
says it is reached through the context.

**Voiding is not an event type.** There is no `VOID` value because voiding a transaction has no
execution context of its own. A void can, however, cause scripts that listen for edits to run. If an
edit-time script is deployed on a transaction type that can be voided, filter out those runs: check
whether the memo reads "VOID" or "Voided" and check the record's voided field.

## Examples

Lock the approval status on purchase orders when they are opened for editing, and refuse to create
one without a memo:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/ui/serverWidget', 'N/error'], (serverWidget, error) => {
  const beforeLoad = (context) => {
    if (context.type !== context.UserEventType.EDIT) {
      return;
    }
    const statusField = context.form.getField({ id: 'approvalstatus' });
    if (statusField) {
      statusField.updateDisplayType({ displayType: serverWidget.FieldDisplayType.INLINE });
    }
  };

  const beforeSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const memo = context.newRecord.getValue({ fieldId: 'memo' });
    if (!memo || !String(memo).trim()) {
      throw error.create({
        name: 'PO_MEMO_REQUIRED',
        message: 'Enter a memo describing the purpose of this purchase order.',
        notifyOff: true
      });
    }
  };

  return { beforeLoad, beforeSubmit };
});
```

Record what changed after a save, comparing the old and new due date. `oldRecord` is empty on
create, so the script checks for it first:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log'], (log) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.EDIT || !context.oldRecord) {
      return;
    }
    const before = context.oldRecord.getValue({ fieldId: 'duedate' });
    const after = context.newRecord.getValue({ fieldId: 'duedate' });
    if (String(before) !== String(after)) {
      log.audit({
        title: `Due date moved on PO ${context.newRecord.id}`,
        details: `${before} -> ${after}`
      });
    }
  };

  return { afterSubmit };
});
```
