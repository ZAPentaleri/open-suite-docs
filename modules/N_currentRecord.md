# N/currentRecord
<!-- id: N/currentRecord -->

`N/currentRecord` gives client-side code a handle on the record that is active on the page in the
user's browser right now: the form they are looking at or editing. You read and change body fields,
walk and edit sublist lines, and inspect field and sublist metadata, all against the live page.

How it fits with the rest of SuiteScript:

- **Client scripts only.** The module is available only in client scripts. For server scripts, or
  for any record other than the one on the page, use [N/record](N_record.md) instead. The two
  modules are comparable in shape, so code that knows one reads easily against the other.
- **Entry-point client scripts don't need to load this module.** Each entry point receives the
  page's record as `scriptContext.currentRecord` (see [ClientScript](../script-types/ClientScript.md)).
  Custom modules and other non-entry-point client code must call
  [`currentRecord.get()`](#currentrecordget) to obtain it.
- **Always dynamic.** The current record always behaves as a dynamic-mode record: sublist edits go
  through "select a line, set values on it, commit it" (see [Record modes](../topics/record_modes.md)).
- **View mode is read-only.** When the page shows the record in view mode, the record is read-only:
  the set methods do not work there and fail if called.
- **Subrecords:** you cannot edit them through this module; you can read them when the record is in
  view mode.
- Adding or removing select options is only possible on fields a script added to the form, and
  `isReadOnly` can only be set on text area fields (details under [Field](#field-object)).

- **Availability:** client scripts.
- **Since:** the source does not state a release for the module as a whole; its members date from
  2016.2 unless noted otherwise.
- **Load it as:** `define(['N/currentRecord'], (currentRecord) => { ... })`
- Related: [N/record](N_record.md), [ClientScript](../script-types/ClientScript.md),
  [N/format](N_format.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`currentRecord.get()`](#currentrecordget) | function | The record active on the current page |
| [`currentRecord.get.promise()`](#currentrecordgetpromise) | function | Promise form of `get` |
| [`CurrentRecord`](#currentrecord-object) | object | Read and edit the page's record and its sublists |
| [`Field`](#field-object) | object | Metadata and select options of a body or sublist field |
| [`Sublist`](#sublist-object) | object | Metadata of a sublist on the page |
| [`Column`](#column-object) | object | Metadata of one sublist column |

## Functions

### currentRecord.get()
<!-- id: N/currentRecord.get -->

Returns the record that is active on the current page. Use it from custom modules and other code
that is not a client script entry point; entry points already have the same object in
`scriptContext.currentRecord`.

**Returns:** `currentRecord.CurrentRecord`, the page's active record.

**Errors:**

- `CANNOT_CREATE_RECORD_INSTANCE`: the operation failed, for example because the page is not
  scriptable or the record object could not be created.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

### currentRecord.get.promise()
<!-- id: N/currentRecord.get.promise -->

The asynchronous form of [`currentRecord.get()`](#currentrecordget). It takes the same (no)
arguments and can fail with the same error, but returns a `Promise` that resolves to the
`CurrentRecord` instead of returning it directly.

**Returns:** `Promise`

**Errors:**

- `CANNOT_CREATE_RECORD_INSTANCE`: the operation failed (the promise rejects).

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

## Objects

### CurrentRecord (object)
<!-- id: N/currentRecord.CurrentRecord -->

The record active on the current page. Entry-point client scripts receive it as
`scriptContext.currentRecord`; custom modules and non-entry-point scripts get it from
[`currentRecord.get()`](#currentrecordget). It is always in dynamic mode. Client scripts, since
2016.2.

Most set and line methods return the same `CurrentRecord`, so calls can be chained.

#### CurrentRecord.getValue(options)
<!-- id: N/currentRecord.CurrentRecord#getValue -->

Reads the value of a body field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `number | Date | string | array | boolean`, the field's value.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.fieldId` is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

Do not use this to check custom password fields: what comes back is an obfuscated value, not the
hash. To test a custom password field, use [`crypto.checkPasswordField`](N_crypto.md) instead.

#### CurrentRecord.getText(options)
<!-- id: N/currentRecord.CurrentRecord#getText -->

Reads a body field's value as display text. For multi-select fields you get an array of texts, and
for rate fields a string that includes the percent sign.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `string` (an array for multi-select fields, as described above).

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.fieldId` is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setValue(options)
<!-- id: N/currentRecord.CurrentRecord#setValue -->

Sets a body field's value. Pass a value whose JavaScript type matches the field type; rate fields
take a number.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.fieldId` | string | Yes | |

- `options.ignoreFieldChange`: when `true`, the field change event and the secondary events it
  would set off are skipped.
- `options.forceSyncSourcing` (since 2019.1): when `true`, fields that depend on this one and are
  still empty are sourced synchronously. Use it to avoid timing problems with sourcing in the
  browser.
- `options.value`: text, radio, select and multi-select fields take a string; checkboxes a boolean;
  date and date/time fields a `Date`; integer, float, currency and percent fields a number.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid, for example its type does not match the
  field's type.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setText(options)
<!-- id: N/currentRecord.CurrentRecord#setText -->

Sets a body field by its display text. For rate fields, pass a string with the percent sign.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |
| `options.fieldId` | string | Yes | |
| `options.text` | string | Yes | |

- `options.ignoreFieldChange`: `true` skips the field change event and its secondary events.
- `options.forceSyncSourcing` (since 2019.1): `true` sources empty dependent fields synchronously,
  avoiding timing problems with sourcing in the browser.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getField(options)
<!-- id: N/currentRecord.CurrentRecord#getField -->

Returns a [`Field`](#field-object) object describing a body field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `currentRecord.Field`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.fieldId` is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#getSubrecord -->

Returns the subrecord held in a body field. If the field has no subrecord yet, one is created.
Setting inventory detail values from a client script is not supported: in client scripts,
subrecords are available in view mode only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `currentRecord.CurrentRecord`, the subrecord.

**Errors:**

- `FIELD_1_IS_DISABLED_YOU_CANNOT_APPLY_SUBRECORD_OPERATION_ON_THIS_FIELD`: the feature behind the
  subrecord field is not enabled.
- `FIELD_1_IS_NOT_A_SUBRECORD_FIELD`: the field does not hold a subrecord.
- `SSS_INVALID_FIELD_ON_SUBRECORD_OPERATION`: the subrecord operation is not valid in the current
  state.
- `SSS_MISSING_REQD_ARGUMENT`: `options.fieldId` is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.hasSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#hasSubrecord -->

Tells you whether a body field holds a subrecord.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `boolean`, `true` if the field has a subrecord.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.removeSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#removeSubrecord -->

Removes the subrecord held in a body field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `currentRecord.CurrentRecord`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getSublist(options)
<!-- id: N/currentRecord.CurrentRecord#getSublist -->

Returns a [`Sublist`](#sublist-object) object for a sublist on the record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `currentRecord.Sublist`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getLineCount(options)
<!-- id: N/currentRecord.CurrentRecord#getLineCount -->

Counts the lines in a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `number`, the number of lines.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#getSublistValue -->

Reads a sublist field's value on a given line. Rate and high-precision rate fields come back as
numbers.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `number | Date | string | array | boolean`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getSublistText(options)
<!-- id: N/currentRecord.CurrentRecord#getSublistText -->

Reads a sublist field on a given line as display text: an array of texts for multi-select fields,
a string with the percent sign for rate fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `string` (an array for multi-select fields).

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getSublistField(options)
<!-- id: N/currentRecord.CurrentRecord#getSublistField -->

Returns a [`Field`](#field-object) object for a sublist field on a given line. It is not supported
in the `fieldChanged`, `pageInit` or `postSourcing` client script entry points. For the line that
is currently selected and not yet committed, the source tells you to use `getCurrentSublistField`
instead.

> [!SOURCE-ERROR] For the current, uncommitted line the source sends you to
> `getCurrentSublistField`, but the method it points to belongs to the `N/record` Record object;
> `CurrentRecord` as documented has no `getCurrentSublistField`. Do not assume that method exists
> on `CurrentRecord`; verify in your account before relying on it.
<!-- source-error: N/currentRecord.CurrentRecord#getSublistField | - | points_to_record_getcurrentsublistfield_for_current_line -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `currentRecord.Field`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts, except the [`fieldChanged`](../script-types/ClientScript.md),
`pageInit` and `postSourcing` entry points.

**Since:** 2016.2

#### CurrentRecord.hasSublistSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#hasSublistSubrecord -->

Tells you whether a sublist field on a given line holds a subrecord.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `boolean`, `true` if there is a subrecord.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.findSublistLineWithValue(options)
<!-- id: N/currentRecord.CurrentRecord#findSublistLineWithValue -->

Searches a sublist column for a value and returns the line number of the first match, or `-1` if
no line matches.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | No | |

- `options.value`: the value to look for. It is marked optional, but the source does not state
  what the method searches for when it is left out; always pass it.

**Returns:** `number`, the first matching line, or `-1`.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.selectLine(options)
<!-- id: N/currentRecord.CurrentRecord#selectLine -->

Selects an existing sublist line, making it the current line so you can read or change it with the
`...CurrentSublist...` methods.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.selectNewLine(options)
<!-- id: N/currentRecord.CurrentRecord#selectNewLine -->

Starts a new line at the end of the sublist and makes it the current line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getCurrentSublistIndex(options)
<!-- id: N/currentRecord.CurrentRecord#getCurrentSublistIndex -->

Returns the index of the line currently selected in a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `number`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.sublistId` is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getCurrentSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#getCurrentSublistValue -->

Reads a field's value on the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `number | Date | string | array | boolean`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getCurrentSublistText(options)
<!-- id: N/currentRecord.CurrentRecord#getCurrentSublistText -->

Reads a field on the currently selected line as display text; multi-select fields give an array.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `string` (an array for multi-select fields).

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setCurrentSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#setCurrentSublistValue -->

Sets a field on the currently selected line. Rate fields take a number. Setting the value also runs
NetSuite's internal line validation; if that validation fails (for example because the line falls
in a closed catch-up period) the script fails.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |

- `options.ignoreFieldChange`: `true` skips the field change event and its secondary events.
- `options.forceSyncSourcing` (since 2019.1): `true` sources empty dependent fields synchronously,
  avoiding browser sourcing timing issues.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid for the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.
- Editing a field on a sublist that is in read-only mode raises an error whose message says the
  sublist is read-only and cannot be edited. The source gives only that message, not an error code.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setCurrentSublistText(options)
<!-- id: N/currentRecord.CurrentRecord#setCurrentSublistText -->

Sets a field on the currently selected line by display text. Rate fields take a string with the
percent sign.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.text` | string | Yes | |

- `options.ignoreFieldChange`: `true` skips the field change event and its secondary events.
- `options.forceSyncSourcing` (since 2019.1): `true` sources empty dependent fields synchronously.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.
- Editing a read-only sublist field, or any field of a sublist in read-only mode, raises an error
  saying the sublist cannot be edited. The source gives the message only, not an error code.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getCurrentSublistSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#getCurrentSublistSubrecord -->

Returns the subrecord held in a sublist field on the current line, creating one if there is none.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `currentRecord.CurrentRecord`, the subrecord.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.hasCurrentSublistSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#hasCurrentSublistSubrecord -->

Tells you whether a sublist field on the current line holds a subrecord. Applies to dynamic mode,
which is the mode the current record is always in.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `boolean`, `true` if there is a subrecord.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.removeCurrentSublistSubrecord(options)
<!-- id: N/currentRecord.CurrentRecord#removeCurrentSublistSubrecord -->

Removes the subrecord held in a sublist field on the current line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `currentRecord.CurrentRecord`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.commitLine(options)
<!-- id: N/currentRecord.CurrentRecord#commitLine -->

Commits the currently selected line into the sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreRecalc` | boolean | No | |
| `options.sublistId` | string | Yes | |

- `options.ignoreRecalc`: `true` makes the commit ignore the recalculation that scripting would
  otherwise trigger. The source does not state a default for this method (`insertLine` and
  `removeLine` both give `false` for their option of the same name).

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.cancelLine(options)
<!-- id: N/currentRecord.CurrentRecord#cancelLine -->

Cancels the currently selected sublist line, discarding the changes made on it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `currentRecord.CurrentRecord`, the same record, for chaining.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: `options.sublistId` is missing.

**Governance:** 0 units.

**Availability:** client scripts. The method's own entry carries no availability, but the
`N/currentRecord` module as a whole is client-side only, so this method is available in client
scripts like the rest of the module.

**Since:** 2016.2

#### CurrentRecord.insertLine(options)
<!-- id: N/currentRecord.CurrentRecord#insertLine -->

Inserts a line into a sublist at the given position.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreRecalc` | boolean | No | `false` |
| `options.sublistId` | string | Yes | |
| `options.line` | number | Yes | |

- `options.ignoreRecalc`: `true` skips the recalculation that scripting would normally trigger.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.removeLine(options)
<!-- id: N/currentRecord.CurrentRecord#removeLine -->

Removes a line from a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreRecalc` | boolean | No | `false` |
| `options.sublistId` | string | Yes | |
| `options.line` | number | Yes | |

- `options.ignoreRecalc`: `true` skips the recalculation that scripting would normally trigger.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.moveLine(options)
<!-- id: N/currentRecord.CurrentRecord#moveLine -->

Moves a sublist line to another position; the other lines keep their relative order. Line indexes
start at 0. This works only on sublists whose machine allows moving lines, that have a sequence
field, and whose type is an edit machine. Only the current line can be moved.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.from` | number | Yes | |
| `options.to` | number | Yes | |

- All three options date from 2020.2.

**Returns:** `currentRecord.CurrentRecord`

**Errors:**

- `SSS_NOT_YET_SUPPORTED`: the line index is invalid, in particular when you try to move a line
  other than the current one.
- `SSS_SUBLIST_DOESNT_SUPPORT_MOVING_LINES`: the sublist does not allow lines to be moved in this
  context.
- `SSS_INVALID_SUBLIST_OPERATION`: an argument value is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** listed for both client and server scripts.

> [!SOURCE-ERROR] The source lists this method as available in both client and server scripts,
> although the `N/currentRecord` module is available only in client scripts. Treat it as a
> client-script method, and do not count on calling it from a server script.
<!-- source-error: N/currentRecord.CurrentRecord#moveLine | - | moveline_client_and_server_in_client_only_module -->

**Since:** 2020.2

#### CurrentRecord.getMatrixHeaderCount(options)
<!-- id: N/currentRecord.CurrentRecord#getMatrixHeaderCount -->

Returns how many columns a matrix field has.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `number`, the number of matrix columns.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getMatrixHeaderField(options)
<!-- id: N/currentRecord.CurrentRecord#getMatrixHeaderField -->

Returns the [`Field`](#field-object) for a matrix header in a given column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

**Returns:** `currentRecord.Field`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getMatrixHeaderValue(options)
<!-- id: N/currentRecord.CurrentRecord#getMatrixHeaderValue -->

Reads the header value of a matrix column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

**Returns:** `number | Date | string | array | boolean`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setMatrixHeaderValue(options)
<!-- id: N/currentRecord.CurrentRecord#setMatrixHeaderValue -->

Sets the header value of a matrix column. Rate fields take a number.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

- `options.ignoreFieldChange`: `true` skips the field change event and its secondary events.
- `options.forceSyncSourcing` (since 2019.1): `true` sources empty dependent fields synchronously.
- `options.value`: text, radio and select fields take a string; checkboxes a boolean; date and
  date/time fields a `Date`; integer, float, currency and percent fields a number.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid, for example its type does not match the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getMatrixSublistField(options)
<!-- id: N/currentRecord.CurrentRecord#getMatrixSublistField -->

Returns the [`Field`](#field-object) for a matrix cell at a given line and column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.line` | number | Yes | |

**Returns:** `currentRecord.Field`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getMatrixSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#getMatrixSublistValue -->

Reads a matrix cell's value at a given line and column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.line` | number | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

- `options.line`: zero-based line index.

**Returns:** `number | Date | string | array | boolean`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setMatrixSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#setMatrixSublistValue -->

Sets a matrix cell at a given line and column. Rate fields take a number.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.line` | number | Yes | |

- `options.value`: text, radio and select fields take a string; checkboxes a boolean; date and
  date/time fields a `Date`; integer, float, currency and percent fields a number.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid, for example its type does not match the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.getCurrentMatrixSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#getCurrentMatrixSublistValue -->

Reads a matrix cell on the currently selected line. Rate and high-precision rate fields come back
as numbers.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

**Returns:** `number | Date | string | array | boolean`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.setCurrentMatrixSublistValue(options)
<!-- id: N/currentRecord.CurrentRecord#setCurrentMatrixSublistValue -->

Sets a matrix cell on the currently selected line. Rate fields take a number. Not supported on
standard records.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.column` | number | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

- `options.column`: zero-based column index.
- `options.ignoreFieldChange`: `true` skips the field change event and its secondary events.
- `options.forceSyncSourcing` (since 2019.1): `true` sources empty dependent fields synchronously.
- `options.value`: text, radio and select fields take a string; checkboxes a boolean; date and
  date/time fields a `Date`; integer, float, currency and percent fields a number.

**Returns:** `currentRecord.CurrentRecord`, for chaining.

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid, for example its type does not match the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts; not supported on standard records.

**Since:** 2016.2

#### CurrentRecord.findMatrixSublistLineWithValue(options)
<!-- id: N/currentRecord.CurrentRecord#findMatrixSublistLineWithValue -->

Searches one matrix column for a value and returns the line of the first match.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.column` | number | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.value` | number | Yes | |

- `options.column`: zero-based column index.
- `options.value`: typed as `number` only, unlike `findSublistLineWithValue`, which accepts any field
  value type.

**Returns:** `number`, the first matching line. The source does not state what is returned when no
line matches; do not assume it mirrors the `-1` of `findSublistLineWithValue` without testing.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### CurrentRecord.type
<!-- id: N/currentRecord.CurrentRecord#type -->

`string`, read-only. The record type: for standard records a [`record.Type`](N_record.md) value,
for custom records the custom record type's string ID. Client scripts, since 2016.2.

#### CurrentRecord.isDynamic
<!-- id: N/currentRecord.CurrentRecord#isDynamic -->

`boolean`, read-only. `true` when the record is in dynamic mode; the mode is set when the record is
created or accessed (and for the current record it is always dynamic). Client scripts, since
2016.2.

#### CurrentRecord.id
<!-- id: N/currentRecord.CurrentRecord#id -->

`number`, read-only. The record's internal ID. Client scripts, since 2016.2.

### Field (object)
<!-- id: N/currentRecord.Field -->

Describes one body or sublist field of the current record and lets you adjust how it appears on the
form. Get it from [`CurrentRecord.getField`](#currentrecordgetfieldoptions) (body fields) or
[`CurrentRecord.getSublistField`](#currentrecordgetsublistfieldoptions) (sublist fields). Client
scripts, since 2016.2.

#### Field.getSelectOptions(options)
<!-- id: N/currentRecord.Field#getSelectOptions -->

Lists the options of a select, multi-select or radio field, filtered by text. Works in dynamic
mode.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filter` | string | Yes | |
| `options.operator` | string | Yes | `contains` |

- `options.filter`: text to match; matching is not case-sensitive.
- `options.operator`: how to match, one of `contains`, `is` or `startswith`.

> [!SOURCE-ERROR] The source marks `options.operator` as required and at the same time gives it a
> default of `contains`, which cannot both be true. Passing an operator explicitly works either
> way; whether you may omit it is not settled.
<!-- source-error: N/currentRecord.Field#getSelectOptions | options.operator | marked_required_but_has_default -->

**Returns:** `array` of objects pairing each option's value and text. You get `null` instead when
the field is a popup select field, when it is not on the form, or when it has more options than the
"maximum entries in dropdowns" set preference allows (past that setting the list turns into a
popup). At most 500 options are returned, and the result can differ from role to role.

**Errors:**

- `TypeError`: the field's type does not support select options.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### Field.insertSelectOption(options)
<!-- id: N/currentRecord.Field#insertSelectOption -->

Adds an option to a select or multi-select field. Only works on fields whose ID starts with
`custpage`, that is, fields added by a front-end Suitelet or a `beforeLoad` user event script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | Yes | |
| `options.text` | string | Yes | |
| `options.isSelected` | boolean | No | `false` |

- `options.value`: the option's internal identifier, not shown to the user.
- `options.text`: the label the user sees.
- `options.isSelected`: `true` makes this option the selected one by default.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_UI_OBJECT_TYPE`: an argument has the wrong type, in particular when the field is not
  a supported type or its ID lacks the `custpage` prefix.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### Field.removeSelectOption(options)
<!-- id: N/currentRecord.Field#removeSelectOption -->

Removes an option from a select field. Same restriction as `insertSelectOption`: only `custpage`
fields added by a front-end Suitelet or a `beforeLoad` user event script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | Yes | |

- `options.value`: the option's ID (the value not shown in the UI, not the label). Passing `null`
  removes every option.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_UI_OBJECT_TYPE`: an argument has the wrong type, in particular when the field is not
  a supported type or its ID lacks the `custpage` prefix.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### Field.id
<!-- id: N/currentRecord.Field#id -->

`string`, read-only. Internal ID of the body or sublist field. Client scripts, since 2016.2.

#### Field.label
<!-- id: N/currentRecord.Field#label -->

`string`, read-only. The field's label as shown in the UI. Client scripts, since 2016.2.

#### Field.type
<!-- id: N/currentRecord.Field#type -->

`string`, read-only. The field type, as a [`format.Type`](N_format.md#formattype) value. The source
also records, on this property, a maximum length of 801 characters for select fields. Client
scripts, since 2016.2.

> [!SOURCE-ERROR] The 801-character limit for select fields is attached to the `type` property, where
> a length limit makes little sense, and the source does not say what the limit really applies to.
> Treat the figure as unconfirmed and do not read it as a limit on the `type` string.
<!-- source-error: N/currentRecord.Field#type | - | select_field_length_limit_on_type_property -->

#### Field.sublistId
<!-- id: N/currentRecord.Field#sublistId -->

`string`, read-only. For a sublist field, the ID of the sublist it belongs to. Client scripts,
since 2016.2.

#### Field.isMandatory
<!-- id: N/currentRecord.Field#isMandatory -->

`boolean`. `true` when the field is mandatory on the form. Client scripts, since 2016.2.

#### Field.isDisabled
<!-- id: N/currentRecord.Field#isDisabled -->

`boolean`. `true` when the field is disabled. Setting it lets you change the display type of a body
field; on a sublist field it affects the whole column, not just one line. Client scripts, since
2016.2.

#### Field.isDisplay
<!-- id: N/currentRecord.Field#isDisplay -->

`boolean`. `true` when the field is displayed on the form; a field can exist on the record without
being displayed. For sublist fields this property is read-only. Client scripts, since 2016.2.

#### Field.isVisible
<!-- id: N/currentRecord.Field#isVisible -->

`boolean`, read-only. `true` when the field is visible on the form. Client scripts, since 2016.2.

#### Field.isReadOnly
<!-- id: N/currentRecord.Field#isReadOnly -->

`boolean`. `true` when the field cannot be edited on the form. You can set it only on text area
fields. Client scripts, since 2016.2.

#### Field.isPopup
<!-- id: N/currentRecord.Field#isPopup -->

`boolean`, read-only. `true` when the field is a popup list field. Client scripts, since 2016.2.

### Sublist (object)
<!-- id: N/currentRecord.Sublist -->

Describes a sublist of the current record. Get it from
[`CurrentRecord.getSublist`](#currentrecordgetsublistoptions). Client scripts, since 2016.2.

#### Sublist.getColumn(options)
<!-- id: N/currentRecord.Sublist#getColumn -->

Returns the [`Column`](#column-object) for a field of this sublist, or `null` if there is none.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `currentRecord.Column | null`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.2

#### Sublist.id
<!-- id: N/currentRecord.Sublist#id -->

`string`, read-only. The sublist's internal ID. Client scripts, since 2016.2.

#### Sublist.type
<!-- id: N/currentRecord.Sublist#type -->

`string`, read-only. The sublist's type. The source does not state which values this property can
take. Client scripts, since 2016.2.

#### Sublist.isChanged
<!-- id: N/currentRecord.Sublist#isChanged -->

`boolean`, read-only. `true` when the sublist has been changed on the form. Client scripts, since
2016.2.

#### Sublist.isDisplay
<!-- id: N/currentRecord.Sublist#isDisplay -->

`boolean`, read-only. `true` when the sublist is displayed on the form. Client scripts, since
2016.2.

### Column (object)
<!-- id: N/currentRecord.Column -->

Describes one column of a sublist on the current record. Get it from
[`Sublist.getColumn`](#sublistgetcolumnoptions). Client scripts, since 2016.2.

#### Column.id
<!-- id: N/currentRecord.Column#id -->

`string`, read-only. The column's internal ID. Since 2016.2.

#### Column.label
<!-- id: N/currentRecord.Column#label -->

`string`, read-only. The column's label. Since 2016.2.

> [!SOURCE-ERROR] The source describes this property as the column's internal ID, which is what
> `Column.id` holds; by its name it is the label. Expect the visible label here and use `Column.id`
> for the internal ID.
<!-- source-error: N/currentRecord.Column#label | - | label_described_as_internal_id -->

#### Column.sublistId
<!-- id: N/currentRecord.Column#sublistId -->

`string`, read-only. Internal ID of the sublist that contains the column. Since 2016.2.

#### Column.type
<!-- id: N/currentRecord.Column#type -->

`string`, read-only. The column's type. The source does not state which values this property can
take. Since 2016.2.

#### Column.isDisabled
<!-- id: N/currentRecord.Column#isDisabled -->

`boolean`. `true` when the column is disabled. Since 2020.2.

#### Column.isMandatory
<!-- id: N/currentRecord.Column#isMandatory -->

`boolean`. `true` when the column is required. Since 2020.2.

## Examples

A client script on purchase orders that checks each item line before it is added. It reads the
current line's quantity and rate, rejects lines with no quantity, and tags large lines in the
description without retriggering `fieldChanged`. Feedback goes through the browser's `alert`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define([], () => {
  const REVIEW_THRESHOLD = 25000;

  const validateLine = (scriptContext) => {
    if (scriptContext.sublistId !== 'item') {
      return true;
    }
    const po = scriptContext.currentRecord;
    const qty = Number(po.getCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity' }));
    const rate = Number(po.getCurrentSublistValue({ sublistId: 'item', fieldId: 'rate' }));

    if (!qty || qty <= 0) {
      alert('Enter a quantity greater than zero before adding this line.');
      return false;
    }

    const extended = qty * rate;
    const note = po.getCurrentSublistValue({ sublistId: 'item', fieldId: 'description' }) || '';
    if (extended >= REVIEW_THRESHOLD && !note.startsWith('[REVIEW]')) {
      po.setCurrentSublistValue({
        sublistId: 'item',
        fieldId: 'description',
        value: `[REVIEW] ${note}`.trim(),
        ignoreFieldChange: true
      });
    }
    return true;
  };

  return { validateLine };
});
```

A helper module called from a custom button: it finds an item already on the order and bumps its
quantity on that line, or adds a new line if the item is missing. If the user changes their mind,
`cancelLine` discards the pending line instead. Because it is not an entry point, it fetches the
record with `currentRecord.get()`.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/currentRecord'], (currentRecord) => {
  const addOrIncrement = (itemId, extraQty) => {
    const po = currentRecord.get();
    const line = po.findSublistLineWithValue({ sublistId: 'item', fieldId: 'item', value: itemId });

    if (line === -1) {
      po.selectNewLine({ sublistId: 'item' });
      po.setCurrentSublistValue({ sublistId: 'item', fieldId: 'item', value: itemId, forceSyncSourcing: true });
      po.setCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity', value: extraQty });
    } else {
      po.selectLine({ sublistId: 'item', line });
      const qty = Number(po.getCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity' }));
      po.setCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity', value: qty + extraQty });
    }

    if (!window.confirm('Apply this quantity change?')) {
      po.cancelLine({ sublistId: 'item' });
      return po.getLineCount({ sublistId: 'item' });
    }
    po.commitLine({ sublistId: 'item' });
    return po.getLineCount({ sublistId: 'item' });
  };

  return { addOrIncrement };
});
```

The promise form, used from a page-level helper that should not block while the record handle is
created:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/currentRecord'], (currentRecord) => {
  const summarizeLines = () => currentRecord.get.promise()
    .then((rec) => {
      const count = rec.getLineCount({ sublistId: 'item' });
      const lines = [];
      for (let i = 0; i < count; i += 1) {
        lines.push({
          item: rec.getSublistText({ sublistId: 'item', fieldId: 'item', line: i }),
          amount: rec.getSublistValue({ sublistId: 'item', fieldId: 'amount', line: i })
        });
      }
      return lines;
    })
    .catch((err) => {
      console.error(`Could not read the record: ${err.name}`);
      return [];
    });

  return { summarizeLines };
});
```
