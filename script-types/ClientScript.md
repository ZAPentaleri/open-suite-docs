# ClientScript
<!-- id: script:ClientScript -->

A client script runs in the user's browser while a form is open. NetSuite calls it on predefined
form events: the form loading, a field changing, a sublist line being selected or committed, the
user saving. Its two jobs are checking what the user enters before it reaches the server, and
filling in fields or sublist lines automatically.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `ClientScript` |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Client (browser) |
| SuiteScript 1.0 counterpart | Client Scripts ([1.0 concepts](../topics/ss1_api.client_scripts.md)) |
| Entry points | [`pageInit`](#pageinitcontext), [`fieldChanged`](#fieldchangedcontext), [`postSourcing`](#postsourcingcontext), [`sublistChanged`](#sublistchangedcontext), [`lineInit`](#lineinitcontext), [`validateField`](#validatefieldcontext), [`validateLine`](#validatelinecontext), [`validateInsert`](#validateinsertcontext), [`validateDelete`](#validatedeletecontext), [`saveRecord`](#saverecordcontext), [`localizationContextEnter`](#localizationcontextentercontext), [`localizationContextExit`](#localizationcontextexitcontext) |

Related pages: [N/currentRecord](../modules/N_currentRecord.md), [N/ui/dialog](../modules/N_ui_dialog.md),
[N/ui/message](../modules/N_ui_message.md), [N/log](../modules/N_log.md),
[script deployment](../topics/script_deployment.md), [form-level deployment](../topics/script_deployment.form_level.md),
[record-level deployment](../topics/script_deployment.record_level.md), [SDF object](../topics/sdf.clientscript.md),
[UserEventScript](UserEventScript.md), [Suitelet](Suitelet.md).

## How it runs

**Where it runs.** Client scripts work on entity and transaction record types, and more broadly on
most standard records, custom record types, and custom pages such as Suitelets. A script can be
deployed globally to a record type, attached to a specific form, and limited to certain roles.

**Edit mode only.** Client scripts run while a record is being edited, not while it is merely viewed.
Clicking Edit on a record you are viewing is what makes them run; in particular `pageInit` does not
fire on a form opened in view mode.

**What fires them.** Loading a form for editing, entering or changing a field value, changing a field
that sources other fields, selecting a sublist line, adding a line, and saving the form.

**Order of execution.** Form-level client scripts run first, then record-level client scripts, then
user event scripts. The order among scripts can be adjusted on the Scripted Records page.

**The current record.** The record that is active in the browser is reached through
[N/currentRecord](../modules/N_currentRecord.md); every entry point receives it as
`context.currentRecord`.

**Permissions follow the user.** A client script has no elevated rights; it follows the permissions
of the current user's role. Creating, loading, copying or changing a record from a client script is a
remote call to the NetSuite database made with that role, and searches run with that role too. If the
role cannot see a record the script uses, the call fails with a permission error, and a search over
records the role cannot view fails. A typical trap: on a sales rep change, the script reads the
employee record, which works for an administrator and fails for a restricted role. Plan for every role
that uses the form, including records those roles cannot open. Options when a role lacks access:

- grant the permissions the script needs;
- use only records every user of the form can access;
- move the logic to a user event script that executes as administrator;
- for searches, send a GET request with URL parameters to a [Suitelet](Suitelet.md) that runs as
  administrator; the Suitelet reads the parameters from
  [`ServerRequest.parameters`](../modules/N_http.md), runs the search and writes the result to the
  response with [`ServerResponse.write`](../modules/N_http.md).

**Logging.** When the script is attached to a form, calls to [N/log](../modules/N_log.md) are silently
ignored; use the browser's `console.log` instead.

**Alerts.** Browser alerts only work in client scripts.

**Managing scripts.** Client scripts can be managed with SDF, copied between accounts with Copy to
Account (the control is at the upper right of the client script page), and reviewed with SuiteScript
Analysis.

## Entry points

### pageInit(context)
<!-- id: script:ClientScript.pageInit -->

Runs when the page finishes loading and when the form is reset. It only runs in edit mode: it fires
when the user clicks Edit, not when the record is opened for viewing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.mode` | string | - | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |

Both context properties exist since 2015.2.

- `context.mode` is `copy`, `create` or `edit`, telling you how the form was opened.
- `context.currentRecord` is the record open in the form.

**Returns:** `void`

Good uses: setting default field values; enabling or disabling fields, including based on the data
the record was loaded with; setting initial values together with flags your other functions check;
alerting the user when the loaded data is inconsistent; adapting fields to the logged-in user; and
checking that fields your custom code depends on are actually on the form.

### fieldChanged(context)
<!-- id: script:ClientScript.fieldChanged -->

Runs when a field value changes, whether the user typed it or client code set it. Use it to show
information based on what was entered, or to enable and disable other fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.line` | string | - | |
| `context.column` | string | - | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |
| `context.fieldId` | string | - | |

All context properties exist since 2015.2.

- `context.line` is the zero-based line index (typed as a string); it is undefined when the field is
  not on a sublist or matrix.
- `context.column` is the zero-based column index (typed as a string); it is undefined when the field
  is not on a matrix.
- `context.sublistId` and `context.fieldId` identify the field that changed.

**Returns:** `void`

- It does not fire for a field whose value arrives through the page URL. Handle those values in
  [`pageInit`](#pageinitcontext) instead.
- The source also lists a user event script's [`beforeLoad`](UserEventScript.md#beforeloadcontext) as
  something that can trigger `fieldChanged`. It does not state how or under what conditions that
  happens, so test it before depending on it.

### postSourcing(context)
<!-- id: script:ClientScript.postSourcing -->

Runs after a field that sources other fields has changed (by a click or a new value) and all the
dependent values have been filled in. Think of it as `fieldChanged` that waits until sourcing has
completely finished.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |
| `context.fieldId` | string | - | |

All context properties exist since 2015.2.

**Returns:** `void`

- Available on transaction forms only.
- Not triggered for a field that has no dependent fields.

### sublistChanged(context)
<!-- id: script:ClientScript.sublistChanged -->

Runs after a sublist line has been added, removed or edited. It takes over the role of the SuiteScript
1.0 `recalc` function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |

Both context properties exist since 2015.2.

**Returns:** `void`

### lineInit(context)
<!-- id: script:ClientScript.lineInit -->

Runs when the user selects an existing line or adds a new one. It plays the part of `pageInit` for
individual lines of inline editor and editor sublists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |

Both context properties exist since 2015.2.

**Returns:** `void`

### validateField(context)
<!-- id: script:ClientScript.validateField -->

Runs when a field changes, by the user or by a client call, and decides whether the new value is
accepted. It also applies to fields that a user event script added in
[`beforeLoad`](UserEventScript.md#beforeloadcontext).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.line` | string | - | |
| `context.column` | string | - | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |
| `context.fieldId` | string | - | |

All context properties exist since 2015.2.

- `context.line`: zero-based line index; undefined when the field is not on a sublist or matrix.
- `context.column`: zero-based column index; undefined when the field is not on a matrix.

**Returns:** `boolean`. `true` accepts the change; `false` rejects it.

- Typical checks: length of the entry, its format, an allowed range, or consistency with a related
  field.
- Not supported on list fields or box fields.

### validateLine(context)
<!-- id: script:ClientScript.validateLine -->

Runs before a line is added to an inline editor or editor sublist. It is the line-level counterpart
of [`saveRecord`](#saverecordcontext): it decides whether the line goes in.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |

Both context properties exist since 2015.2.

**Returns:** `boolean`. `true` adds the line; `false` rejects it.

- Applies to inline editor sublists.

### validateInsert(context)
<!-- id: script:ClientScript.validateInsert -->

Runs when a line is inserted into an edit sublist, and decides whether the insert goes ahead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |

Both context properties exist since 2015.2.

**Returns:** `boolean`. `true` lets the insert go ahead; `false` blocks it.

- Applies to edit sublists.

### validateDelete(context)
<!-- id: script:ClientScript.validateDelete -->

Runs when a line is removed from an edit sublist, and decides whether the removal goes ahead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.lineCount` | number | - | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |
| `context.sublistId` | string | - | |

`context.lineCount` exists since 2020.2; the other two since 2015.2.

- `context.lineCount` is the number of lines about to be deleted. It is only defined when the delete
  came from the Clear All Lines button.

**Returns:** `boolean`. `true` allows the deletion; `false` blocks it.

- Applies to edit sublists.

### saveRecord(context)
<!-- id: script:ClientScript.saveRecord -->

Runs after the user clicks the submit button and before the form is sent to the server. It is the
last chance to stop a save.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |

`context.currentRecord` exists since 2015.2.

**Returns:** `boolean`. `true` means the record is valid and is saved; `false` blocks the save.

- Typical uses: alerting the user before the save, re-enabling fields that other functions disabled,
  and redirecting to a URL.

### localizationContextEnter(context)
<!-- id: script:ClientScript.localizationContextEnter -->

Runs when the record enters the localization context the deployment is filtered on.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.locale` | string | - | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |

`context.locale` exists since 2020.1; `context.currentRecord` since 2015.2.

- `context.locale` holds the countries of the localization context the record has just entered.

**Returns:** `void`

When the deployment uses localization context filtering:

- `pageInit` is ignored;
- no entry point is called before the record enters the context or after it leaves it.

If the record never enters the context, none of the script's callbacks run.

### localizationContextExit(context)
<!-- id: script:ClientScript.localizationContextExit -->

Runs when the record leaves the localization context.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.locale` | string | - | |
| `context.currentRecord` | currentRecord.CurrentRecord | - | |

`context.locale` exists since 2020.1; `context.currentRecord` since 2015.2.

- `context.locale` is described as the countries of the *new* localization context.

> [!SOURCE-ERROR] The source describes the exit event's `locale` with the same wording as the enter
> event (the countries of the new localization context), which does not fit a record that is leaving
> a context. Check in your account whether `locale` here names the context being left or another one.
<!-- source-error: script:ClientScript.localizationContextExit | locale | exit_locale_described_as_new_context -->

**Returns:** `void`

With localization context filtering set, `pageInit` is ignored and no entry point runs before entry or
after exit. A record that never enters the context gets no callbacks at all.

## Examples

Purchase order entry checks. A copied order starts with an empty memo so the buyer writes a fresh
justification; a line's expected receipt date may not fall after the order's Receive By date
(`duedate`); very large quantities raise a banner that names the line (remember `context.line` is
zero-based); and an order without item lines cannot be saved.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/dialog', 'N/ui/message'], (dialog, message) => {
  const BULK_QTY = 500;

  const pageInit = (context) => {
    if (context.mode === 'copy') {
      context.currentRecord.setValue({ fieldId: 'memo', value: '', ignoreFieldChange: true });
    }
  };

  const validateField = (context) => {
    if (context.sublistId !== 'item' || context.fieldId !== 'expectedreceiptdate') {
      return true;
    }
    const rec = context.currentRecord;
    const lineDate = rec.getCurrentSublistValue({ sublistId: 'item', fieldId: 'expectedreceiptdate' });
    const receiveBy = rec.getValue({ fieldId: 'duedate' });
    if (lineDate instanceof Date && receiveBy instanceof Date && lineDate > receiveBy) {
      dialog.alert({
        title: 'Receipt date too late',
        message: 'A line cannot be expected after the order Receive By date.'
      });
      return false;
    }
    return true;
  };

  const fieldChanged = (context) => {
    if (context.sublistId !== 'item' || context.fieldId !== 'quantity') {
      return;
    }
    const qty = Number(context.currentRecord.getCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity' }));
    if (qty >= BULK_QTY) {
      const lineNo = context.line === undefined ? 'new' : Number(context.line) + 1;
      message.create({
        type: message.Type.INFORMATION,
        title: 'Bulk quantity',
        message: `Line ${lineNo} orders ${qty} units. Check the vendor's pallet pricing.`
      }).show({ duration: 8000 });
    }
  };

  const saveRecord = (context) => {
    if (context.currentRecord.getLineCount({ sublistId: 'item' }) === 0) {
      dialog.alert({ title: 'No items', message: 'Add at least one item line before saving.' });
      return false;
    }
    return true;
  };

  return { pageInit, validateField, fieldChanged, saveRecord };
});
```

Line housekeeping on the same form. New lines start with a quantity of 1; after the item on a line
has finished sourcing, a missing rate is reported in the browser console (N/log calls are ignored in
form-attached client scripts); closed lines cannot be deleted; and a bulk clear through Clear All
Lines, the only case where `lineCount` is set, shows a warning first.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/message'], (message) => {
  const lineInit = (context) => {
    if (context.sublistId !== 'item') {
      return;
    }
    const rec = context.currentRecord;
    const item = rec.getCurrentSublistValue({ sublistId: 'item', fieldId: 'item' });
    if (!item) {
      rec.setCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity', value: 1, ignoreFieldChange: true });
    }
  };

  const postSourcing = (context) => {
    if (context.sublistId !== 'item' || context.fieldId !== 'item') {
      return;
    }
    const rate = Number(context.currentRecord.getCurrentSublistValue({ sublistId: 'item', fieldId: 'rate' }));
    if (!(rate > 0)) {
      console.log('Sourced item has no purchase rate; the buyer must enter one.');
    }
  };

  const validateDelete = (context) => {
    if (context.sublistId !== 'item') {
      return true;
    }
    if (typeof context.lineCount === 'number') {
      message.create({
        type: message.Type.WARNING,
        title: 'Clearing item lines',
        message: `${context.lineCount} lines are about to be removed.`
      }).show({ duration: 6000 });
    }
    const closed = context.currentRecord.getCurrentSublistValue({ sublistId: 'item', fieldId: 'isclosed' });
    return closed !== true;
  };

  return { lineInit, postSourcing, validateDelete };
});
```
