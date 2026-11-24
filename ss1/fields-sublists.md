# Fields, sublists and matrix fields (SuiteScript 1.0)

> This page documents the SuiteScript 1.0 field, sublist (line item) and matrix functions so that
> tools and developers can **read, convert and minimally repair** legacy scripts. It is not a guide
> for new development: new code belongs in SuiteScript 2.1.

Things that trip people up when converting these functions:

- Arguments are **positional**; the parameter tables list them in the order a call must use.
- Sublist line numbers and matrix column numbers are **1-based** in 1.0. The 2.x equivalents use
  0-based indexes, so every converted loop and index needs adjusting.
- Checkbox **values** are the strings `'T'` and `'F'`. The `firefieldchanged` and `synchronous`
  **flags** are real booleans (`true`/`false`), not `'T'`/`'F'`.
- The global `nlapi...` functions here work on the *current* record (the one the client script or
  user event is running on). For a record loaded or created in code, use the matching `nlobjRecord`
  methods instead.
- A `**Since:**` line appears where the source states the release that introduced a function.
- Where a section says the source does not state availability, the source gives no list of script
  types for that function.

The two trailing flags on the setter functions behave the same way wherever they appear:

- `firefieldchanged` (default `true`): when `true`, setting the value runs the field changed client
  script. It only matters in client scripts.
- `synchronous` (default `false`): when `true`, the call waits for any sourcing the change triggers
  before the script continues. It only matters in client scripts; on the server the call is always
  synchronous.

Concepts behind these functions:

- [SuiteScript 1.0 API overview](../topics/ss1_api.md)
- [Fields in 1.0](../topics/ss1_api.fields.md)
- [Sublists in 1.0](../topics/ss1_api.sublists.md)
- [Inline editing](../topics/ss1_api.inline_editing.md) (relevant to `nlapiSubmitField`)
- [Records in 1.0](../topics/ss1_api.records.md)
- [Converting 1.0 to 2.1](../conversion/ss1-to-ss21.md)

Field and sublist IDs per record type are in the [generated record reference](../gen/records/index.md).

## Multi-value sublist read

### nlapiGetLineItemValues(type, fldnam, linenum)
<!-- id: ss1:nlapiGetLineItemValues -->

Reads a multi-value field on a given sublist line and returns all of its values as an array.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `type`: the sublist ID.
- `fldnam`: the field ID on that sublist.
- `linenum`: 1-based line number.

> [!SOURCE-ERROR] The source spells the second parameter two ways: its API index calls it `fldname`,
> while the function's own page calls it `fldnam` (this mismatch is flagged twice in the source
> material). Because 1.0 arguments are positional, the spelling does not change how a call works;
> this page keeps `fldnam`.
<!-- source-error: ss1:nlapiGetLineItemValues | - | index_spells_parameter_fldname -->
<!-- source-error: ss1:nlapiGetLineItemValues | - | index_spells_parameter_fldname -->

> [!SOURCE-ERROR] On the function's page, the heading and the parameter list also use different
> parameter names. Rely on position, not on either name.
<!-- source-error: ss1:nlapiGetLineItemValues | - | heading_and_parameter_list_names_differ -->

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** meant for user event scripts; not supported in client scripts.

**Since:** 2012.1

2.x equivalent: `getSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord` (see
[N/record](../modules/N_record.md)).

## Date/time values with a time zone

These six functions read or write date/time fields while converting between the stored value and a
time zone you name. They are intended for user event scripts, and all were added in 2013.2. Their
2.x counterpart is only approximate: the mapping names just the [N/format](../modules/N_format.md)
module (for the time-zone conversion), not a specific member.

For every `timeZone` argument below: it accepts either an Olson time zone name as a string or the
numeric key of that zone (the values of `N/format.Timezone`). The string form is not case-sensitive.
If `timeZone` is left out, the conversion uses the default time zone.

For every `dateTime` argument below: the value must look like `mm/dd/yyyy hh:mm:ss am` (or `pm`).

### nlapiGetCurrentLineItemDateTimeValue(type, fieldId, timeZone)
<!-- id: ss1:nlapiGetCurrentLineItemDateTimeValue -->

Reads a date/time field on the currently selected line of a sublist, converted into the given time
zone (or the default one).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `timeZone` | string \| number | No | |

- `timeZone`: Olson name (case-insensitive) or Olson key number; default time zone when omitted.

**Returns:** `string`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument has an invalid value.

**Governance:** 0 units.

**Availability:** intended for user event scripts.

**Since:** 2013.2

### nlapiGetDateTimeValue(fieldId, timeZone)
<!-- id: ss1:nlapiGetDateTimeValue -->

Reads a body date/time field, converted into the given time zone (or the default one).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fieldId` | string | Yes | |
| `timeZone` | string \| number | No | |

- `timeZone`: Olson name (case-insensitive) or Olson key number; default time zone when omitted.

**Returns:** `string`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument has an invalid value.

**Governance:** 0 units.

**Availability:** intended for user event scripts.

**Since:** 2013.2

### nlapiGetLineItemDateTimeValue(type, fieldId, lineNum, timeZone)
<!-- id: ss1:nlapiGetLineItemDateTimeValue -->

Reads a date/time field on a given sublist line, converted into the given time zone (or the default
one).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `lineNum` | number | Yes | |
| `timeZone` | string \| number | No | |

- `lineNum`: 1-based line number.
- `timeZone`: Olson name (case-insensitive) or Olson key number; default time zone when omitted.

> [!SOURCE-ERROR] The source's API mapping labels the third argument as a value rather than as the
> line number. The function's own parameter list makes it the line number; treat position 3 as
> `lineNum`.
<!-- source-error: ss1:nlapiGetLineItemDateTimeValue | - | api_map_names_linenum_arg_value -->

**Returns:** `string`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument has an invalid value.

**Governance:** 0 units.

**Availability:** intended for user event scripts.

**Since:** 2013.2

### nlapiSetCurrentLineItemDateTimeValue(type, fieldId, dateTime, timeZone)
<!-- id: ss1:nlapiSetCurrentLineItemDateTimeValue -->

Writes a date/time field on the currently selected sublist line. The value you pass is read as being
in the given time zone (or the default one) and converted from there.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `dateTime` | string | Yes | |
| `timeZone` | string \| number | No | |

- `dateTime`: format `mm/dd/yyyy hh:mm:ss am`/`pm`.
- `timeZone`: Olson name (case-insensitive) or Olson key number; default time zone when omitted.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument has an invalid value.

**Governance:** 0 units.

**Availability:** intended for user event scripts.

**Since:** 2013.2

### nlapiSetDateTimeValue(fieldId, dateTime, timeZone)
<!-- id: ss1:nlapiSetDateTimeValue -->

Writes a body date/time field, reading the value as being in the given time zone (or the default
one).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fieldId` | string | Yes | |
| `dateTime` | string | Yes | |
| `timeZone` | string \| number | No | |

- `dateTime`: format `mm/dd/yyyy hh:mm:ss am`/`pm`.
- `timeZone`: Olson name (case-insensitive) or Olson key number; default time zone when omitted.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument has an invalid value.

**Governance:** 0 units.

**Availability:** intended for user event scripts.

**Since:** 2013.2

### nlapiSetLineItemDateTimeValue(type, fieldId, lineNum, dateTime, timeZone)
<!-- id: ss1:nlapiSetLineItemDateTimeValue -->

Writes a date/time field on a given sublist line, reading the value as being in the given time zone
(or the default one).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `lineNum` | number | Yes | |
| `dateTime` | string | Yes | |
| `timeZone` | string \| number | No | |

- `lineNum`: 1-based line number.
- `dateTime`: format `mm/dd/yyyy hh:mm:ss am`/`pm`.
- `timeZone`: Olson name (case-insensitive) or Olson key number; default time zone when omitted.

> [!SOURCE-ERROR] As with the matching getter, the source's API mapping labels the line-number
> argument as a value. The parameter list makes position 3 the line number and position 4 the
> date/time; follow that order.
<!-- source-error: ss1:nlapiSetLineItemDateTimeValue | - | api_map_names_linenum_arg_value -->

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument has an invalid value.

**Governance:** 0 units.

**Availability:** intended for user event scripts.

**Since:** 2013.2

## Body field functions

### nlapiDisableField(fldnam, val)
<!-- id: ss1:nlapiDisableField -->

Switches a body field's disabled state on or off on the current form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |
| `val` | boolean | Yes | |

- `val`: `true` disables the field.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client scripts only.

2.x equivalent: a pattern change; the mapping points at `N/currentRecord.Field#isDisabled` (see
[N/currentRecord](../modules/N_currentRecord.md)).

### nlapiGetField(fldnam)
<!-- id: ss1:nlapiGetField -->

Returns an object describing a body field (its metadata, not its value).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `nlobjField` with the body field's metadata. In client scripts this object is read-only:
only its getter methods can be used.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.1

- For a field on a sublist, use [`nlapiGetLineItemField`](#nlapigetlineitemfieldtype-fldnam-linenum)
  instead.

2.x equivalent: `getField` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetFieldText(fldnam)
<!-- id: ss1:nlapiGetFieldText -->

Returns the display text of the option currently chosen in a select field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `string`, the text the user sees for the selected option.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

- Not supported on subrecords, on hidden fields, or on fields that are not select fields.

2.x equivalent: `getText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetFieldTexts(fldnam)
<!-- id: ss1:nlapiGetFieldTexts -->

Returns the display texts of all options chosen in a multiselect field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `Array` of the selected options' display texts.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.1

2.x equivalent: `getText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetFieldValue(fldnam)
<!-- id: ss1:nlapiGetFieldValue -->

Returns the stored value of a body field as a string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `string`. For a select field this is the chosen option's internal ID, as a string. An
unticked inline checkbox reads back as `'F'`. The result is `null` when the field does not exist on
the record or the user is not allowed to see it.

**Errors:**

- A user error is raised if this is called while a delete event is being handled. The source does
  not state the error code.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

- Not supported on subrecords.
- On a multiselect field the values come back joined by the character with code 5. Prefer
  [`nlapiGetFieldValues`](#nlapigetfieldvaluesfldnam), which returns them as an array.

2.x equivalent: `getValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetFieldValues(fldnam)
<!-- id: ss1:nlapiGetFieldValues -->

Returns the internal IDs of all options chosen in a multiselect field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `Array` of internal IDs; `null` when the field does not exist or the user is not
allowed to see it.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.1

2.x equivalent: `getValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiInsertSelectOption(fldnam, value, text, selected)
<!-- id: ss1:nlapiInsertSelectOption -->

Adds an option to a select field. It only works on select fields that a script added through the UI
objects (on a Suitelet form or in a before load user event), not on standard select fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |
| `value` | string \| number | Yes | |
| `text` | string | Yes | |
| `selected` | boolean | No | `false` |

- `value`: the internal value of the new option.
- `text`: the label shown for it.
- `selected`: `true` makes the new option the default selection.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function beyond the
field-origin rule above.

2.x equivalent: `N/currentRecord.Field#insertSelectOption`.

### nlapiLookupField(type, id, fields, text)
<!-- id: ss1:nlapiLookupField -->

Reads one or more body fields of a record without loading the whole record. Its write counterpart
is [`nlapiSubmitField`](#nlapisubmitfieldtype-id-fields-values-dosourcing).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `fields` | string \| string[] | Yes | |
| `text` | boolean | No | `false` |

- `fields`: a single field ID or an array of them. A field on a joined record is written as the
  join ID, a dot and the field name (`join_id.field_name`).
- `text`: `true` returns display text instead of internal IDs. It affects select, image and document
  fields.

**Returns:** `string | Object`. Asking for one field gives back that field's value; asking for an
array of fields gives back an object keyed by field ID.

**Governance:** by record class: transaction 10 units, standard 5 units, custom 2 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts only.

- Long text values are cut off: at 1,300 characters when the account has multiple languages
  enabled, and at 3,900 characters in a single-language account.

2.x equivalent: `N/search.lookupFields` in [N/search](../modules/N_search.md).

### nlapiRemoveSelectOption(fldnam, value)
<!-- id: ss1:nlapiRemoveSelectOption -->

Removes an option from a select field that a script added through the UI objects (on a Suitelet
form or in a before load user event).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | No | |
| `value` | string \| null | No | |

> [!SOURCE-ERROR] The source does not say whether `fldnam` is required. The "No" in the table only
> records that gap; the function cannot know which field to change without it, so pass it.
<!-- source-error: ss1:nlapiRemoveSelectOption | fldnam | param_requiredness_not_stated -->

- `value`: the option to remove. Passing `null` removes every option from the field.

> [!SOURCE-ERROR] The source does not say whether `value` is required either; the "No" is a
> placeholder. Pass the option value, or `null` on purpose to clear the field.
<!-- source-error: ss1:nlapiRemoveSelectOption | value | param_requiredness_not_stated -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function beyond the
field-origin rule above.

2.x equivalent: `N/currentRecord.Field#removeSelectOption`.

### nlapiSetFieldText(fldname, txt, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetFieldText -->

Sets a select field by the option's display text rather than its internal ID. Typical uses are
initialising fields on new records in a before load user event, and filling fields that are not
stored.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |
| `txt` | string | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `fldname`: this function names its first parameter `fldname`, unlike the `fldnam` of its
  siblings; position is what counts.
- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

2.x equivalent: `setText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetFieldTexts(fldname, txts, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetFieldTexts -->

Sets a multiselect field from a list of display texts. Used for the same purposes as
`nlapiSetFieldText`: before load initialisation on new records and non-stored fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |
| `txts` | string[] | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.1

2.x equivalent: `setText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetFieldValue(fldnam, value, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetFieldValue -->

Sets a body field's value. Typical uses are initialising fields on new records in a before load user
event (or a client page init function) and filling fields that are not stored.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |
| `value` | string | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `value`: checkboxes take `'T'` or `'F'`.
- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

2.x equivalent: `setValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetFieldValues(fldnam, value, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetFieldValues -->

Sets a multiselect field to several internal IDs at once, for the same purposes as
`nlapiSetFieldValue`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |
| `value` | string | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `value`: described as an array of values.

> [!SOURCE-ERROR] The source types `value` as a string but describes it as an array of values. This
> page keeps the `string` type as given; legacy calls normally pass an array here, so don't "fix"
> such a call into a string without testing.
<!-- source-error: ss1:nlapiSetFieldValues | value | value_param_typed_string_but_described_as_array -->

- `firefieldchanged`: `true` runs the field changed script; it only matters in client scripts.
- `synchronous`: client-only real boolean, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.1

2.x equivalent: `setValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSubmitField(type, id, fields, values, doSourcing)
<!-- id: ss1:nlapiSubmitField -->

Updates one or more body fields of a record without loading and saving the whole record. Its read
counterpart is [`nlapiLookupField`](#nlapilookupfieldtype-id-fields-text).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `fields` | string \| string[] | Yes | |
| `values` | string \| string[] | Yes | |
| `doSourcing` | boolean | No | `false` |

- `fields` / `values`: each takes a single string or an array of strings.
- `values`: checkboxes take `'T'` or `'F'`.
- `doSourcing`: `true` sources empty fields that depend on the changed ones.

**Returns:** `void`

**Governance:** by record class: transaction 10 units, standard 5 units, custom 2 units. The cost is
charged per call, not per field, so updating several fields in one call costs the same as one.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts only.

- Meant for records that support inline editing, and for their body fields and custom fields (see
  [Inline editing](../topics/ss1_api.inline_editing.md)).
- It cannot update fields on sublist lines.
- Fields that are not inline-editable can be passed, but that is not the intended use and it costs
  more.

2.x equivalent: `N/record.submitFields` in [N/record](../modules/N_record.md).

## Sublist (line item) functions

### nlapiCancelLineItem(type)
<!-- id: ss1:nlapiCancelLineItem -->

Throws away any changes made to the current line that have not been committed.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `cancelLine` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiCommitLineItem(type)
<!-- id: ss1:nlapiCommitLineItem -->

Commits the current line to the sublist; the scripted equivalent of the line's Done button.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `commitLine` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiDisableLineItemField(type, fldnam, val)
<!-- id: ss1:nlapiDisableLineItemField -->

Switches a sublist field's disabled state on or off.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `val` | boolean | Yes | |

- `val`: `true` disables the field.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client scripts only.

2.x equivalent: a pattern change; the mapping points at `N/currentRecord.Field#isDisabled`.

### nlapiFindLineItemMatrixValue(type, fldnam, val, column)
<!-- id: ss1:nlapiFindLineItemMatrixValue -->

Searches a matrix sublist for the first line whose matrix field holds a given value in a given
column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `val` | string | Yes | |
| `column` | number | Yes | |

- `column`: 1-based matrix column number.

**Returns:** `number`, the line number of the first matching line. The source does not state what is
returned when nothing matches.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Matrix functions work only on the pricing and demand plan detail sublists.

2.x equivalent: `findMatrixSublistLineWithValue` on `N/record.Record` or
`N/currentRecord.CurrentRecord`.

### nlapiFindLineItemValue(type, fldnam, val)
<!-- id: ss1:nlapiFindLineItemValue -->

Finds the first line of a sublist where a field holds a given value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `val` | string | Yes | |

**Returns:** `number`, the line number of the first match. The source does not state what is
returned when nothing matches.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Not supported on subrecords.

2.x equivalent: `findSublistLineWithValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetCurrentLineItemIndex(type)
<!-- id: ss1:nlapiGetCurrentLineItemIndex -->

Returns which line of the sublist is currently selected.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `number`, the selected line's number, counted from 1.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `getCurrentSublistIndex` on `N/record.Record` or `N/currentRecord.CurrentRecord`
(0-based there).

### nlapiGetCurrentLineItemMatrixValue(type, fldnam, column)
<!-- id: ss1:nlapiGetCurrentLineItemMatrixValue -->

Reads a matrix field on the currently selected line, for a given column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |

- `column`: 1-based matrix column number.

**Returns:** `string`; `null` when the field does not exist.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Matrix functions work only on the pricing and demand plan detail sublists.

2.x equivalent: `getCurrentMatrixSublistValue` on `N/currentRecord.CurrentRecord` or
`N/record.Record`.

### nlapiGetCurrentLineItemText(type, fldnam)
<!-- id: ss1:nlapiGetCurrentLineItemText -->

Reads the display text of a select field on the currently selected line. A typical place for it is a
client validate line function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |

**Returns:** `string`; `null` when the field does not exist.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

- Not supported on subrecords.

2.x equivalent: `getCurrentSublistText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetCurrentLineItemValue(type, fldnam)
<!-- id: ss1:nlapiGetCurrentLineItemValue -->

Reads a field on the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |

**Returns:** `string`, the field's value as a string; `null` when the field does not exist.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

- Not supported on subrecords.

2.x equivalent: `getCurrentSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetCurrentLineItemValues(type, fldnam)
<!-- id: ss1:nlapiGetCurrentLineItemValues -->

Reads all values of a multi-value field on the currently selected line, for example the
`serialnumbers` field on an item sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** meant for user event scripts; not supported in client scripts.

**Since:** 2012.1

2.x equivalent: `getCurrentSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetLineItemCount(type)
<!-- id: ss1:nlapiGetLineItemCount -->

Returns how many lines a sublist has.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `number`. For sublists of custom child records, when the script runs in a CSV import or
SuiteTalk context, the result is `undefined`.

**Governance:** 0 units.

**Availability:** client and user event scripts only. In a Suitelet, use
`nlobjSubList.getLineItemCount` instead (see [UI objects](objects-forms.md)).

2.x equivalent: `getLineCount` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetLineItemField(type, fldnam, linenum)
<!-- id: ss1:nlapiGetLineItemField -->

Returns an object describing a sublist field (its metadata). For a body field use
[`nlapiGetField`](#nlapigetfieldfldnam).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | No | |

- `linenum`: optional, 1-based.

> [!SOURCE-ERROR] The source's API mapping spells the second parameter `fldnamm` [sic], while its own
> reference uses `fldnam`. The extra letter is a typo in one of the two places; position 2 is the
> field ID either way.
<!-- source-error: ss1:nlapiGetLineItemField | - | api_map_spells_param_fldnamm_reference_fldnam -->

**Returns:** `nlobjField` with the sublist field's metadata.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2009.1

2.x equivalent: `getSublistField` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetLineItemMatrixField(type, fldnam, linenum, column)
<!-- id: ss1:nlapiGetLineItemMatrixField -->

Returns the metadata object for a matrix field on a given line and column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |
| `column` | number | Yes | |

- `linenum`, `column`: both 1-based.

**Returns:** `nlobjField`; `null` when the field does not exist.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Matrix functions work only on the pricing and demand plan detail sublists.

2.x equivalent: `getMatrixSublistField` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetLineItemMatrixValue(type, fldnam, linenum, column)
<!-- id: ss1:nlapiGetLineItemMatrixValue -->

Reads a matrix field on a given line and column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |
| `column` | number | Yes | |

- `linenum`, `column`: both 1-based.

**Returns:** `string`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Matrix functions work only on the pricing and demand plan detail sublists.

2.x equivalent: `getMatrixSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetLineItemText(type, fldnam, linenum)
<!-- id: ss1:nlapiGetLineItemText -->

Reads the display text of a select field on a given line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: 1-based.

**Returns:** `string`; `null` when the field does not exist or the user may not see it.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

- Not supported on subrecords.

2.x equivalent: `getSublistText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetLineItemValue(type, fldnam, linenum)
<!-- id: ss1:nlapiGetLineItemValue -->

Reads a field on a given sublist line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: 1-based.

**Returns:** `string`, the value as a string.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

- Not supported on subrecords.
- A custom column field can only be read this way if it is set to show on the form; hidden fields
  are always available.
- The source also says default line values cannot be set outside edit mode.

> [!SOURCE-ERROR] That last caveat is about setting values, yet it sits on this getter's entry; it
> looks carried over from a setter. Don't treat it as a restriction on reading.
<!-- source-error: ss1:nlapiGetLineItemValue | - | getter_entry_carries_setter_edit_mode_caveat -->

2.x equivalent: `getSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetMatrixCount(type, fldnam)
<!-- id: ss1:nlapiGetMatrixCount -->

Returns how many columns a matrix field has.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |

**Returns:** `number`, the column count of the matrix field.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Matrix functions work only on the pricing and demand plan detail sublists.

2.x equivalent: `getMatrixHeaderCount` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetMatrixField(type, fldnam, column)
<!-- id: ss1:nlapiGetMatrixField -->

Returns the metadata object for a pricing sublist header field (such as a quantity break) in a given
column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |

- `column`: 1-based.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Availability:** client and user event scripts only. The quantity header fields require the
`"Quantity Pricing"` feature.

**Since:** 2009.2

2.x equivalent: `getMatrixHeaderField` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiGetMatrixValue(type, fldnam, column)
<!-- id: ss1:nlapiGetMatrixValue -->

Reads a pricing sublist header field in a given column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |

- `column`: 1-based.

**Returns:** `number`. (The other matrix getters return strings; the source gives this one as a
number, which looks inconsistent.)

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

2.x equivalent: `getMatrixHeaderValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiInsertLineItem(type, line)
<!-- id: ss1:nlapiInsertLineItem -->

Inserts a new, empty line into a sublist; it goes in above the selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `line` | number | Yes | |

- `line`: 1-based.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

2.x equivalent: `insertLine` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiInsertLineItemOption(type, fldnam, value, text, selected)
<!-- id: ss1:nlapiInsertLineItemOption -->

Adds an option to a select field on a sublist. Like the body-field version, it works only on select
fields a script added through the UI objects.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `value` | string \| number | Yes | |
| `text` | string | Yes | |
| `selected` | boolean | No | `false` |

- `selected`: `true` makes the new option the default.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

- When adding many options, disable the drop-down while you add them.

2.x equivalent: `N/currentRecord.Field#insertSelectOption`.

### nlapiIsLineItemChanged(type)
<!-- id: ss1:nlapiIsLineItemChanged -->

Tells whether the selected line has been edited.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `boolean`; `true` when the selected line has been edited.

**Governance:** 0 units.

**Availability:** client scripts only.

2.x equivalent: a pattern change; the mapping points at `N/record.Sublist#isChanged`.

### nlapiRefreshLineItems(type)
<!-- id: ss1:nlapiRefreshLineItems -->

Refreshes a sublist on the form. What happens depends on the sublist's kind: a static list is
reloaded from the server, an editor or inline editor sublist is redrawn, and a plain list is not
affected at all.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: none. The mapping lists **no equivalent**.

### nlapiRemoveLineItem(type, line)
<!-- id: ss1:nlapiRemoveLineItem -->

Removes a line from a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `line` | number | No | |

- `line`: 1-based. Required in user event scripts and Suitelets; in a client script you can select
  the line first and leave it out.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client, user event and Suitelet scripts only. In a scheduled script, use
`nlobjRecord.removeLineItem` on a loaded record instead (see [record and search
objects](objects-record-search.md)).

2.x equivalent: `removeLine` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiRemoveLineItemOption(type, fldnam, value)
<!-- id: ss1:nlapiRemoveLineItemOption -->

Removes an option from a select field on a sublist that a script added through the UI objects.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `value` | string | Yes | |

- `value`: the option to remove. Passing `null` removes all options, even though the parameter is
  marked required and typed as a string; that combination looks inconsistent.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `N/currentRecord.Field#removeSelectOption`.

### nlapiSelectLineItem(type, linenum)
<!-- id: ss1:nlapiSelectLineItem -->

Selects an existing line so that the current-line functions act on it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: 1-based.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `selectLine` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSelectNewLineItem(type)
<!-- id: ss1:nlapiSelectNewLineItem -->

Starts a new line for adding to the sublist; the scripted equivalent of clicking the sublist's tab to
begin a fresh line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `selectNewLine` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetCurrentLineItemMatrixValue(type, fldnam, column, value, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetCurrentLineItemMatrixValue -->

Sets a matrix field on the currently selected line, for a given column. A typical place for it is a
client validate line function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |
| `value` | string \| number | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `column`: 1-based.
- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only.

**Since:** 2009.2

- Matrix functions work only on the pricing and demand plan detail sublists.

2.x equivalent: `setCurrentMatrixSublistValue` on `N/record.Record` or
`N/currentRecord.CurrentRecord`.

### nlapiSetCurrentLineItemText(type, fldnam, text, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetCurrentLineItemText -->

Sets a select field on the currently selected line by display text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `text` | string | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `setCurrentSublistText` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetCurrentLineItemValue(type, fldnam, value, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetCurrentLineItemValue -->

Sets a field on the currently selected line. A typical place for it is a client validate line
function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `value` | string | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `value`: checkboxes take `'T'` or `'F'`.
- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

2.x equivalent: `setCurrentSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetCurrentLineItemValues(type, fldnam, values, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetCurrentLineItemValues -->

Sets a multi-value field on the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `values` | Array | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `values`: internal IDs. The exception is the `serialnumbers` field, which takes the serial number
  text itself.
- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client scripts only.

**Since:** 2012.1

2.x equivalent: `setCurrentSublistValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiSetLineItemValue(type, fldnam, linenum, value)
<!-- id: ss1:nlapiSetLineItemValue -->

Sets a field on a given line without selecting it. It is meant for initialising non-stored sublist
fields on new records in a before load user event; on a record that already exists it has no effect.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |
| `value` | string | Yes | |

- `linenum`: 1-based.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** the source does not give a script-type list. In client scripts only custom fields
and the description field are supported.

> [!SOURCE-ERROR] The source says this function is for user events only, yet in the same place
> describes how it behaves in client scripts. The source does not resolve which is right; test before
> relying on it in a client script.
<!-- source-error: ss1:nlapiSetLineItemValue | - | says_user_event_only_but_describes_client_use -->

2.x equivalent: `N/record.Record#setSublistValue` in [N/record](../modules/N_record.md) (no
`N/currentRecord` target is mapped).

### nlapiSetMatrixValue(type, fldnam, column, value, firefieldchanged, synchronous)
<!-- id: ss1:nlapiSetMatrixValue -->

Sets the value of a pricing sublist header field (for example a quantity break) for a given column.
Typically called in a client page init function or a before load user event.

The spec lists six positional parameters, in this order: `type` (1), `fldnam` (2), `column` (3),
`value` (4), `firefieldchanged` (5) and `synchronous` (6). The 2.x mapping uses the same order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |
| `value` | string | Yes | |
| `firefieldchanged` | boolean | No | `true` |
| `synchronous` | boolean | No | `false` |

- `type`: the sublist ID.
- `fldnam`: the header field ID.
- `column`: 1-based matrix column number.
- `value`: checkboxes take `'T'` or `'F'`.
- `firefieldchanged`, `synchronous`: client-only real booleans, as described at the top of the page.

> [!SOURCE-ERROR] In the source, the function's heading and its parameter list give the arguments in
> different orders; the SDK signature agrees with the heading. The table above follows the spec's
> parameter list. Check the argument order against known-good legacy calls before converting or
> editing one.
<!-- source-error: ss1:nlapiSetMatrixValue | - | heading_and_parameter_list_order_differ -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and user event scripts only. Requires the `"Quantity Pricing"` feature.

**Since:** 2009.2

2.x equivalent: `setMatrixHeaderValue` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

## Examples

Legacy 1.0 snippets of the sort found in older accounts, with the repairs that are typically needed.
Convert them to 2.1 rather than extending them.

**Repair: checkbox compared with a boolean.** In 1.0 a checkbox reads back as `'T'` or `'F'`, so a
test against `true` never passes, and setting `true` does not tick the box.

```js
function pageInitHoldUnapproved(type) {
  // was: if (nlapiGetFieldValue('supervisorapproval') == true) { ... }
  var approved = nlapiGetFieldValue('supervisorapproval') === 'T';
  if (!approved) {
    nlapiSetFieldValue('memo', 'Awaiting supervisor approval', false);
  }
}
```

**Repair: off-by-one sublist loop.** A loop that starts at 0 asks for a line that does not exist and
skips the last one. 1.0 lines run from 1 to the count, inclusive.

```js
function countOpenPurchaseLines() {
  var open = 0;
  var lineCount = nlapiGetLineItemCount('item');
  // was: for (var i = 0; i < lineCount; i++)
  for (var line = 1; line <= lineCount; line++) {
    if (nlapiGetLineItemValue('item', 'isclosed', line) !== 'T') {
      open++;
    }
  }
  return open;
}
```

**Client validate line: keep quantity and rate sane.** Reads and writes the current line; the flags
are real booleans, and `false` stops the field changed script from firing for this scripted change.

```js
function validateLineMinimumRate(type) {
  if (type != 'item') {
    return true;
  }
  var qty = parseFloat(nlapiGetCurrentLineItemValue('item', 'quantity')) || 0;
  if (qty <= 0) {
    alert('Enter a quantity greater than zero.');
    return false;
  }
  var rate = parseFloat(nlapiGetCurrentLineItemValue('item', 'rate')) || 0;
  if (rate < 0) {
    nlapiSetCurrentLineItemValue('item', 'rate', '0', false, true);
  }
  return true;
}
```

**One lookup, one update.** Reading two fields in one call returns an object keyed by field ID
instead of loading the purchase order; the write then updates only the memo, again without a full
load and save.

```js
function stampPurchaseOrderNote(poId, note) {
  var current = nlapiLookupField('purchaseorder', poId, ['memo', 'duedate']);
  var text = current.memo ? current.memo + ' | ' + note : note;
  nlapiSubmitField('purchaseorder', poId, 'memo', text);
  return current.duedate;
}
```
