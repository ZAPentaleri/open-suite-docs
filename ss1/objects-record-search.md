# Record and search objects (SuiteScript 1.0)

> **Legacy material.** This page describes the SuiteScript 1.0 record and search objects so that
> existing 1.0 scripts can be read, converted to 2.1 and minimally repaired. It is not a guide to
> writing new 1.0 code. For new work use [N/record](../modules/N_record.md) and
> [N/search](../modules/N_search.md), and see the
> [1.0 to 2.1 conversion guide](../conversion/ss1-to-ss21.md) for the mapping of each member.

Everything in 1.0 is positional: the parameter tables below list arguments **in the order the call
expects them**, and swapping two arguments is a classic source of silent bugs in legacy code.
Sublist line numbers and matrix column numbers start at **1** (2.x starts at 0), checkbox values are
the strings `'T'` and `'F'`, and many getters return `null` instead of throwing when the field is
not there.

Usage units: every member costs 0 units except `nlobjSearch.saveSearch` (5),
`nlobjSearchResultSet.forEachResult` (10) and `nlobjSearchResultSet.getResults` (10). The source
gives no release ("since") for the objects themselves; each member shows its own **Since:** where
the source gives one, and says so where it does not.

Related pages:

- [1.0 records](../topics/ss1_api.records.md), [1.0 sublists](../topics/ss1_api.sublists.md),
  [1.0 subrecords](../topics/ss1_api.subrecords.md), [1.0 searches](../topics/ss1_api.search.md),
  [records and subrecords in 2.x](../topics/records_and_subrecords.md)
- Catalogs: [search date filters](../gen/catalogs/search_date_filters.md),
  [search operators by field type](../gen/catalogs/search_operator_field_types.md)

## Contents

| Object | What it represents |
|---|---|
| [`nlobjRecord`](#nlobjrecord-object) | A record loaded, created, copied or transformed in a script |
| [`nlobjSubrecord`](#nlobjsubrecord-object) | A subrecord reached through a body or sublist field |
| [`nlobjSearch`](#nlobjsearch-object) | A search definition, ad hoc or saved |
| [`nlobjSearchColumn`](#nlobjsearchcolumn-object) | One result column of a search |
| [`nlobjSearchFilter`](#nlobjsearchfilter-object) | One filter condition of a search |
| [`nlobjSearchResult`](#nlobjsearchresult-object) | One row of search results |
| [`nlobjSearchResultSet`](#nlobjsearchresultset-object) | The runnable result set of a search |
| [`nlobjSelectOption`](#nlobjselectoption-object) | One choice of a select field |

## Records

### nlobjRecord (object)
<!-- id: ss1:nlobjRecord -->

A record held in memory. You get one from `nlapiCopyRecord`, `nlapiCreateRecord`,
`nlapiGetNewRecord`, `nlapiGetOldRecord`, `nlapiLoadRecord` or `nlapiTransformRecord`. It works for
standard records and for instances of custom records.

Notes that apply to several members below:

- **Time zones.** The date/time members take an optional `timeZone`, given either as an Olson value
  string or as its integer key, and matched without regard to case; when you leave it out, the
  user's time zone preference applies. Their `fieldId` must name a date/time field. They raise
  `SSS_INVALID_ARG_TYPE` when an argument value is invalid.
- **Date/time strings** passed to the setters use the pattern `mm/dd/yyyy hh:mm:ss am|pm`, for
  example `09/25/2013 06:00:01 am`.
- **`ignoreRecalc`** (on `commitLineItem`, `insertLineItem`, `removeLineItem`): `true` skips
  recalculating the record's totals, which speeds up bulk line edits. Only server scripts honour
  it, and the last call in a series must recalculate (leave it out or pass `false`); otherwise
  submitting the record fails.
- **Matrix members** work on matrix sublists, and pricing is the only matrix sublist supported.
- **Custom column fields** on sublists can be read or written only when the field is set to show on
  the form; hidden fields are always available, and showing the field with an empty label works.
- **Subrecord members**: subrecord scripting in 2.x works in a fundamentally different way, so do not
  convert these one-to-one; see [records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.commitLineItem(group, ignoreRecalc)
<!-- id: ss1:nlobjRecord#commitLineItem -->

Commits the line currently selected in a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `ignoreRecalc` | boolean | No | |

- `ignoreRecalc`: `true` leaves the totals unrecalculated (for faster bulk line edits); server
  scripts only; the last call in a series must recalculate or the submit fails.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.createCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlobjRecord#createCurrentLineItemSubrecord -->

Creates a subrecord in a subrecord field of the currently selected sublist line, for example the
`inventorydetail` field on the `item` sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.createSubrecord(fldname)
<!-- id: ss1:nlobjRecord#createSubrecord -->

Creates a subrecord in a subrecord field on the record body.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.editCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlobjRecord#editCurrentLineItemSubrecord -->

Opens for editing the subrecord held in a subrecord field of the currently selected sublist line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.editSubrecord(fldname)
<!-- id: ss1:nlobjRecord#editSubrecord -->

Opens for editing the subrecord held in a body subrecord field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.findLineItemMatrixValue(group, fldnam, column, val)
<!-- id: ss1:nlobjRecord#findLineItemMatrixValue -->

Looks for a value in one column of a matrix field and returns the number of the first line that
holds it. Matrix sublists only (pricing is the only one supported).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |
| `val` | string | Yes | |

- `column`: matrix column number, counted from 1.

**Returns:** `number`, the first matching line number.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.findLineItemValue(group, fldnam, value)
<!-- id: ss1:nlobjRecord#findLineItemValue -->

Returns the number of the first line whose field holds the given value. Works on `editor`,
`inlineeditor` and `list` sublists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `value` | string | Yes | |

**Returns:** `number`, the first matching line number.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.getAllFields()
<!-- id: ss1:nlobjRecord#getAllFields -->

Returns the names of all body fields. Expect a different number of fields in an ordinary edit than
in an inline edit (`xedit`) context.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjRecord.getCurrentLineItemDateTimeValue(type, fieldId, timeZone)
<!-- id: ss1:nlobjRecord#getCurrentLineItemDateTimeValue -->

Reads a date/time field on the currently selected line, converted to the time zone you name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `timeZone` | string \| number | No | |

- `type`: the sublist ID.
- `fieldId`: must be a date/time field.
- `timeZone`: Olson value string or its integer key, case-insensitive; the user's time zone
  preference when omitted.

**Returns:** `string`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument value is invalid.

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjRecord.getCurrentLineItemMatrixValue(group, fldnam, column)
<!-- id: ss1:nlobjRecord#getCurrentLineItemMatrixValue -->

Reads a matrix field value on the currently selected line. Matrix sublists only (pricing is the only
one supported).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |

- `column`: matrix column number, counted from 1.

**Returns:** `string`, or null when the field does not exist.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.getCurrentLineItemValue(type, fldnam)
<!-- id: ss1:nlobjRecord#getCurrentLineItemValue -->

Reads a field on the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |

- `type`: the sublist ID.

**Returns:** `string`, or null when the field does not exist.

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.getCurrentLineItemValues(type, fldnam)
<!-- id: ss1:nlobjRecord#getCurrentLineItemValues -->

Reads a multi-value field on the currently selected line, for example `serialnumbers` on the `item`
sublist. Intended for user event scripts and not supported in client scripts. When converting: the
2.x target, `N/record.Record#getCurrentSublistValue`, returns an array for multiselect fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** server scripts (user event scripts); not client scripts.

**Since:** 2012.1

#### nlobjRecord.getDateTimeValue(fieldId, timeZone)
<!-- id: ss1:nlobjRecord#getDateTimeValue -->

Reads a body date/time field, converted to the time zone you name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fieldId` | string | Yes | |
| `timeZone` | string \| number | No | |

- `fieldId`: must be a date/time field.
- `timeZone`: Olson value string or its integer key, case-insensitive; the user's time zone
  preference when omitted.

**Returns:** `string`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument value is invalid.

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjRecord.getField(fldnam)
<!-- id: ss1:nlobjRecord#getField -->

Returns the field object of a body field, which gives access to its metadata. Supported in server
scripts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `nlobjField` (see [objects-forms.md](objects-forms.md)).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.getFieldText(name)
<!-- id: ss1:nlobjRecord#getFieldText -->

Returns the display text of a select field. Supported in server scripts; the client-side
counterpart is `nlapiGetFieldText`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`, or null when the field does not exist or access to it is restricted.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.getFieldTexts(name)
<!-- id: ss1:nlobjRecord#getFieldTexts -->

Returns the display texts of a multiselect field. Supported in server scripts; the client-side
counterpart is `nlapiGetFieldTexts`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.getFieldValue(name)
<!-- id: ss1:nlobjRecord#getFieldValue -->

Returns a body field's value; for a select field that is the internal ID, as a string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.getFieldValues(name)
<!-- id: ss1:nlobjRecord#getFieldValues -->

Returns the values of a multiselect field. With one option selected you get a plain string; with
several, an array of strings. Check which one came back before using it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string | string[] | null`; null when the field is not on the record.

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.getId()
<!-- id: ss1:nlobjRecord#getId -->

Returns the record's internal ID.

**Returns:** `number | null`; null for a new record that has not been saved yet.

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.getLineItemCount(group)
<!-- id: ss1:nlobjRecord#getLineItemCount -->

Returns the number of lines in a sublist. Lines are numbered from 1, so the count is also the
number of the last line.

> [!SOURCE-ERROR] The source's API map row shows this method without its `group` argument, while the
> method's own entry requires the sublist ID. Always pass it.
<!-- source-error: ss1:nlobjRecord#getLineItemCount | - | api_map_row_signature_omits_group_param -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |

**Returns:** `number`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.getLineItemDateTimeValue(type, fieldId, lineNum, timeZone)
<!-- id: ss1:nlobjRecord#getLineItemDateTimeValue -->

Reads a date/time field on a given sublist line, converted to the time zone you name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `lineNum` | number | Yes | |
| `timeZone` | string \| number | No | |

- `fieldId`: must be a date/time field.
- `lineNum`: line number, counted from 1.
- `timeZone`: Olson value string or its integer key, case-insensitive; the user's time zone
  preference when omitted.

**Returns:** `string`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument value is invalid.

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjRecord.getLineItemField(group, fldnam, linenum)
<!-- id: ss1:nlobjRecord#getLineItemField -->

Returns the field object of a sublist field. Supported in server scripts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: counted from 1; it only matters for `list` sublists.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.getLineItemMatrixField(group, fldnam, linenum, column)
<!-- id: ss1:nlobjRecord#getLineItemMatrixField -->

Returns the field object of a matrix field on a given line and column. Matrix sublists only
(pricing is the only one supported).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |
| `column` | number | Yes | |

- `linenum`, `column`: both counted from 1.

**Returns:** `nlobjField`, or null when the field does not exist.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.getLineItemMatrixValue(group, fldnam, linenum, column)
<!-- id: ss1:nlobjRecord#getLineItemMatrixValue -->

Reads a matrix field value on a given line and column. Matrix sublists only (pricing is the only one
supported).

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjRecord#getLineItemMatrixValue | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |
| `column` | number | Yes | |

- `linenum`, `column`: both counted from 1.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.getLineItemText(group, fldnam, linenum)
<!-- id: ss1:nlobjRecord#getLineItemText -->

Returns the display text of a select field on a given line. Supported in server scripts; the
client-side counterpart is `nlapiGetLineItemText`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: line number, counted from 1.

**Returns:** `string`, or null when the field does not exist or access to it is restricted.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.getLineItemValue(group, name, linenum)
<!-- id: ss1:nlobjRecord#getLineItemValue -->

Reads a field on a given line. Custom column fields must be set to show on the form (an empty label
works); hidden fields are always available.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `name` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: line number, counted from 1.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjRecord.getLineItemValues(type, fldnam, linenum)
<!-- id: ss1:nlobjRecord#getLineItemValues -->

Reads a multi-value field on a given line. Intended for user event scripts and not supported in
client scripts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: line number, counted from 1.

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** server scripts (user event scripts); not client scripts.

**Since:** 2012.1

#### nlobjRecord.getMatrixCount(group, fldnam)
<!-- id: ss1:nlobjRecord#getMatrixCount -->

Returns how many columns a matrix field has. Matrix sublists only (pricing is the only one
supported).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |

**Returns:** `number`, the column count.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.getMatrixField(group, fldnam, column)
<!-- id: ss1:nlobjRecord#getMatrixField -->

Returns the field object of a matrix header field (the field that heads a matrix column). Matrix
sublists only (pricing is the only one supported); supported in server scripts.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjRecord#getMatrixField | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |

- `column`: matrix column number, counted from 1.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Availability:** server scripts. The quantity header fields exist only when the feature
"Quantity Pricing" is enabled.

**Since:** 2009.2

#### nlobjRecord.getMatrixValue(group, fldnam, column)
<!-- id: ss1:nlobjRecord#getMatrixValue -->

Reads the value of a matrix header field. Matrix sublists only (pricing is the only one supported).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |

- `column`: matrix column number, counted from 1.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.getRecordType()
<!-- id: ss1:nlobjRecord#getRecordType -->

Returns the record type ID, such as `salesorder` or `assemblyunbuild`.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.insertLineItem(group, linenum, ignoreRecalc)
<!-- id: ss1:nlobjRecord#insertLineItem -->

Inserts a line at a given position. Works on `editor` and `inlineeditor` sublists and on `list`
sublists that were added through `nlobjSubList`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `linenum` | number | Yes | |
| `ignoreRecalc` | boolean | No | |

- `linenum`: counted from 1; it must not exceed the current line count.
- `ignoreRecalc`: `true` leaves the totals unrecalculated (for faster bulk line edits); server
  scripts only; the last call in a series must recalculate or the submit fails.

**Returns:** `void`

**Errors:**

- An error is raised when `linenum` is greater than the number of lines. The source does not give
  the error code.

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.removeLineItem(group, linenum, ignoreRecalc)
<!-- id: ss1:nlobjRecord#removeLineItem -->

Removes a line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `linenum` | number | Yes | |
| `ignoreRecalc` | boolean | No | |

- `linenum`: line number, counted from 1.
- `ignoreRecalc`: `true` leaves the totals unrecalculated (for faster bulk line edits); server
  scripts only; the last call in a series must recalculate or the submit fails.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.removeCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlobjRecord#removeCurrentLineItemSubrecord -->

Removes the subrecord held in a subrecord field of the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `void`

> [!SOURCE-ERROR] The source's description says this method returns the subrecord, while its return
> type is void. Do not rely on a return value from it.
<!-- source-error: ss1:nlobjRecord#removeCurrentLineItemSubrecord | - | description_says_returns_subrecord_returns_void -->

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.removeSubrecord(fldname)
<!-- id: ss1:nlobjRecord#removeSubrecord -->

Removes the subrecord held in a body subrecord field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `void`

> [!SOURCE-ERROR] The source's description says this method returns the subrecord, while its return
> type is void. Do not rely on a return value from it.
<!-- source-error: ss1:nlobjRecord#removeSubrecord | - | description_says_returns_subrecord_returns_void -->

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.selectLineItem(group, linenum)
<!-- id: ss1:nlobjRecord#selectLineItem -->

Makes an existing line the current line, so that the `...CurrentLineItem...` members act on it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: line number, counted from 1.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.selectNewLineItem(group)
<!-- id: ss1:nlobjRecord#selectNewLineItem -->

Inserts a new line and makes it the current line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.setCurrentLineItemDateTimeValue(type, fieldId, dateTime, timeZone)
<!-- id: ss1:nlobjRecord#setCurrentLineItemDateTimeValue -->

Sets a date/time field on the currently selected line, reading the value in the time zone you name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `dateTime` | string | Yes | |
| `timeZone` | string \| number | No | |

- `fieldId`: must be a date/time field.
- `dateTime`: `mm/dd/yyyy hh:mm:ss am|pm`, for example `09/25/2013 06:00:01 am`.
- `timeZone`: Olson value string or its integer key, case-insensitive; the user's time zone
  preference when omitted.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument value is invalid.

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjRecord.setCurrentLineItemMatrixValue(group, fldnam, column, value)
<!-- id: ss1:nlobjRecord#setCurrentLineItemMatrixValue -->

Sets a matrix field value on the currently selected line. Matrix sublists only (pricing is the only
one supported).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |
| `value` | string \| number | Yes | |

- `column`: matrix column number, counted from 1.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.setCurrentLineItemValue(group, name, value)
<!-- id: ss1:nlobjRecord#setCurrentLineItemValue -->

Sets a field on the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `name` | string | Yes | |
| `value` | string | Yes | |

- `value`: for checkboxes, `'T'` or `'F'`.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjRecord.setDateTimeValue(fieldId, dateTime, timeZone)
<!-- id: ss1:nlobjRecord#setDateTimeValue -->

Sets a body date/time field, reading the value in the time zone you name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fieldId` | string | Yes | |
| `dateTime` | string | Yes | |
| `timeZone` | string \| number | No | |

- `fieldId`: must be a date/time field.
- `dateTime`: `mm/dd/yyyy hh:mm:ss am|pm`, for example `09/25/2013 06:00:01 am`.
- `timeZone`: Olson value string or its integer key, case-insensitive; the user's time zone
  preference when omitted.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument value is invalid.

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjRecord.setFieldText(name, text)
<!-- id: ss1:nlobjRecord#setFieldText -->

Sets a select field by its display text instead of its internal ID. Supported in server scripts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `text` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.setFieldTexts(name, texts)
<!-- id: ss1:nlobjRecord#setFieldTexts -->

Sets a multiselect field by display texts. Supported in server scripts.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjRecord#setFieldTexts | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `texts` | string[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2009.1

#### nlobjRecord.setFieldValues(name, value)
<!-- id: ss1:nlobjRecord#setFieldValues -->

Sets a multiselect field from an array of values.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjRecord.setLineItemDateTimeValue(type, fieldId, lineNum, dateTime, timeZone)
<!-- id: ss1:nlobjRecord#setLineItemDateTimeValue -->

Sets a date/time field on a given line, reading the value in the time zone you name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fieldId` | string | Yes | |
| `lineNum` | number | Yes | |
| `dateTime` | string | Yes | |
| `timeZone` | string \| number | No | |

- `fieldId`: must be a date/time field.
- `lineNum`: line number, counted from 1.
- `dateTime`: `mm/dd/yyyy hh:mm:ss am|pm`, for example `09/25/2013 06:00:01 am`.
- `timeZone`: Olson value string or its integer key, case-insensitive; the user's time zone
  preference when omitted.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG_TYPE`: an argument value is invalid.

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjRecord.setLineItemValue(group, name, linenum, value)
<!-- id: ss1:nlobjRecord#setLineItemValue -->

Sets a field on a given line without selecting the line first. Custom column fields must be set to
show on the form (an empty label works); hidden fields are always available.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `name` | string | Yes | |
| `linenum` | number | Yes | |
| `value` | string | Yes | |

- `linenum`: line number, counted from 1.

**Returns:** `void`

**Errors:**

- An error is raised when `value` is not valid for the field. The source does not give the error
  code.

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjRecord.setMatrixValue(type, fldnam, column, value)
<!-- id: ss1:nlobjRecord#setMatrixValue -->

Sets a matrix header field, for example the quantity levels of the pricing matrix. Matrix sublists
only (pricing is the only one supported).

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjRecord#setMatrixValue | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fldnam` | string | Yes | |
| `column` | number | Yes | |
| `value` | string | Yes | |

- `type`: the sublist ID.
- `column`: matrix column number, counted from 1.
- `value`: for checkboxes, `'T'` or `'F'`.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** setting quantity levels needs the feature "Quantity Pricing" to be enabled.

**Since:** 2009.2

#### nlobjRecord.viewCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlobjRecord#viewCurrentLineItemSubrecord -->

Returns a read-only view of the subrecord on the currently selected line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`, read-only.

**Errors:**

- An error is raised if you try to edit the returned subrecord. The source does not give the error
  code.

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.viewLineItemSubrecord(sublist, fldname, linenum)
<!-- id: ss1:nlobjRecord#viewLineItemSubrecord -->

Returns a read-only view of the subrecord on any line, without selecting that line first.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: line number, counted from 1.

**Returns:** `nlobjSubrecord`, read-only.

**Errors:**

- An error is raised if you try to edit the returned subrecord. The source does not give the error
  code.

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.viewSubrecord(fldname)
<!-- id: ss1:nlobjRecord#viewSubrecord -->

Returns a read-only view of the subrecord in a body subrecord field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`, read-only.

**Errors:**

- An error is raised if you try to edit the returned subrecord. The source does not give the error
  code.

**Governance:** 0 units.

**Since:** 2011.2

2.x subrecord scripting is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjRecord.getAllLineItemFields(group)
<!-- id: ss1:nlobjRecord#getAllLineItemFields -->

Returns the field names of a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRecord.setFieldValue(name, value)
<!-- id: ss1:nlobjRecord#setFieldValue -->

Sets a body field's value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

The source states no release for this method.

### nlobjSubrecord (object)
<!-- id: ss1:nlobjSubrecord -->

A subrecord, reached through a body field or a sublist field of a parent record; the parent must
have been created or loaded first (see the `createSubrecord`, `editSubrecord`, `viewSubrecord` and
`...LineItemSubrecord` members above). Subrecord scripting in 2.x is fundamentally different; see
[records and subrecords](../topics/records_and_subrecords.md).

#### nlobjSubrecord.cancel()
<!-- id: ss1:nlobjSubrecord#cancel -->

Throws away the changes and returns the subrecord to its last committed state. The instance is no
longer usable afterwards; get it again from the parent record.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2011.2

#### nlobjSubrecord.commit()
<!-- id: ss1:nlobjSubrecord#commit -->

Commits the subrecord into its parent record; nothing reaches the database yet. For a subrecord on a
sublist line, commit the line too, and then submit the parent with `nlapiSubmitRecord`.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2011.2

## Searches

### nlobjSearch (object)
<!-- id: ss1:nlobjSearch -->

A search definition, returned by `nlapiCreateSearch` (ad hoc) or `nlapiLoadSearch` (saved). A search
created in a script is private by default. Persist it with `saveSearch`.

#### nlobjSearch.addColumn(column)
<!-- id: ss1:nlobjSearch#addColumn -->

Adds a column and leaves the existing ones as they are.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `column` | nlobjSearchColumn | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.addColumns(columns)
<!-- id: ss1:nlobjSearch#addColumns -->

Adds several columns and leaves the existing ones as they are.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `columns` | nlobjSearchColumn[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.addFilter(filter)
<!-- id: ss1:nlobjSearch#addFilter -->

Adds a filter and leaves the existing ones as they are. It accepts an `nlobjSearchFilter` only;
filter expressions are not supported here.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `filter` | nlobjSearchFilter | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.addFilters(filters)
<!-- id: ss1:nlobjSearch#addFilters -->

Adds several filters and leaves the existing ones as they are. It accepts `nlobjSearchFilter`
objects only; filter expressions are not supported here.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `filters` | nlobjSearchFilter[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.deleteSearch()
<!-- id: ss1:nlobjSearch#deleteSearch -->

Deletes a saved search, whether it was saved in the UI or by a script. The search must have been
loaded or saved first.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.getFilters()
<!-- id: ss1:nlobjSearch#getFilters -->

Returns the filters as a list of filter objects, not as an expression. When you need the expression
form, call `getFilterExpression` instead.

**Returns:** `nlobjSearchFilter[]`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.getId()
<!-- id: ss1:nlobjSearch#getId -->

Returns the search's numeric internal ID (not its `customsearch...` script ID). It is available once
the search has been loaded or saved.

**Returns:** `string | null`; null for an ad hoc search.

> [!SOURCE-ERROR] The source gives the search internal ID as a string here but as a number from
> `saveSearch`. Convert both to the same type before comparing IDs.
<!-- source-error: ss1:nlobjSearch#getId | - | search_internal_id_string_in_getid_number_in_savesearch -->

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.getIsPublic()
<!-- id: ss1:nlobjSearch#getIsPublic -->

Returns `true` when the search is public.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.getScriptId()
<!-- id: ss1:nlobjSearch#getScriptId -->

Returns the search's script ID, such as `customsearch_mysearch`. It is available once the search has
been loaded or saved.

**Returns:** `string | null`; null for an ad hoc search.

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.getSearchType()
<!-- id: ss1:nlobjSearch#getSearchType -->

Returns the record type being searched, such as `customer` or `salesorder`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.runSearch()
<!-- id: ss1:nlobjSearch#runSearch -->

Runs a search built with `nlapiCreateSearch` or `nlapiLoadSearch` and returns a result set that you
can iterate or read in slices. Running a search does not save it.

**Returns:** `nlobjSearchResultSet`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.saveSearch(title, scriptId)
<!-- id: ss1:nlobjSearch#saveSearch -->

Saves the search and returns its internal ID. Saving a loaded search again under a new title renames
it; it does not create a copy.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | No | |
| `scriptId` | string | No | |

- `title`: required when the search is new.
- `scriptId`: must start with `customsearch` (for example `customsearch_my_new_search`); when you
  leave it out, the system generates one on the first save.

**Returns:** `number`, the search internal ID.

**Governance:** 5 units.

**Since:** 2012.1

#### nlobjSearch.setColumns(columns)
<!-- id: ss1:nlobjSearch#setColumns -->

Replaces all columns. Passing null, or an empty array, removes them all.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `columns` | nlobjSearchColumn[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.setFilterExpression(filterExpression)
<!-- id: ss1:nlobjSearch#setFilterExpression -->

Replaces all filters with a filter expression. Passing null, or an empty array, removes them all.
Filters you add afterwards with `addFilter` or `addFilters` are joined to the expression with AND.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `filterExpression` | Object[] | Yes | |

> [!SOURCE-ERROR] The source describes the expression as an array of strings, yet an expression holds
> nested arrays (conditions and operators). The spec types it as `Object[]`; pass the nested-array
> form.
<!-- source-error: ss1:nlobjSearch#setFilterExpression | filterExpression | expression_described_as_string_array_holds_arrays -->

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjSearch.setFilters(filters)
<!-- id: ss1:nlobjSearch#setFilters -->

Replaces all filters. Passing null, or an empty array, removes them all. It accepts
`nlobjSearchFilter` objects only; filter expressions are not supported here.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `filters` | nlobjSearchFilter[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.setIsPublic(type)
<!-- id: ss1:nlobjSearch#setIsPublic -->

Makes the search public or private.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | boolean | Yes | |

- `type`: `true` makes it public, `false` private.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.setRedirectURLToSearch()
<!-- id: ss1:nlobjSearch#setRedirectURLToSearch -->

Sends the user to the search definition page. It works for saved searches, ad hoc searches, and
loaded searches that were changed but not saved. Nothing comes back: no URL is returned.

**Returns:** `void` (undefined).

**Governance:** 0 units.

**Availability:** `afterSubmit` user events, Suitelets and client scripts only.

**Since:** 2012.1

#### nlobjSearch.setRedirectURLToSearchResults()
<!-- id: ss1:nlobjSearch#setRedirectURLToSearchResults -->

Sends the user to the search results page. It works for saved searches, ad hoc searches, and loaded
searches that were changed but not saved. Nothing comes back: no URL is returned.

**Returns:** `void` (undefined).

**Governance:** 0 units.

**Availability:** `afterSubmit` user events, Suitelets and client scripts only.

**Since:** 2012.1

#### nlobjSearch.getColumns()
<!-- id: ss1:nlobjSearch#getColumns -->

Returns the search's columns.

**Returns:** `nlobjSearchColumn[]`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearch.getFilterExpression()
<!-- id: ss1:nlobjSearch#getFilterExpression -->

Returns the filters in expression form.

**Returns:** `Object[]`

**Governance:** 0 units.

**Since:** 2012.2

### nlobjSearchColumn (object)
<!-- id: ss1:nlobjSearchColumn -->

One result column, created with `new`. When a search groups its results, every column needs a
summary type.

#### new nlobjSearchColumn(name, join, summary)
<!-- id: ss1:nlobjSearchColumn#constructor -->

Creates a column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `join` | string | No | |
| `summary` | string | No | |

- `join`: the join ID, to take the column from a joined record.
- `summary`: `group`, `sum`, `count`, `avg`, `min` or `max`.

**Returns:** the source states no return value; the object is created with `new`.

**Governance:** 0 units.

The source states no release for the constructor.

#### nlobjSearchColumn.getLabel()
<!-- id: ss1:nlobjSearchColumn#getLabel -->

Returns only a custom label set on the column.

**Returns:** `string | null`; null when the column uses its built-in label.

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjSearchColumn.getSummary()
<!-- id: ss1:nlobjSearchColumn#getSummary -->

Returns the summary type: `group`, `sum`, `count`, `avg`, `min` or `max`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjSearchColumn.setFormula(formula)
<!-- id: ss1:nlobjSearchColumn#setFormula -->

Sets the formula of a formula column, that is one named `formulatext`, `formulanumeric`,
`formuladatetime`, `formulapercent` or `formulacurrency`. Returns the same column, so calls can be
chained. See [search formulas](../topics/search_formulas.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `formula` | string | Yes | |

**Returns:** `nlobjSearchColumn` (the same column).

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchColumn.setFunction(functionid)
<!-- id: ss1:nlobjSearchColumn#setFunction -->

Applies a special function to the column's values. Returns the same column, so calls can be
chained.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `functionid` | string | Yes | |

- `functionid`: one of `percentOfTotal`, `absoluteValue`, `ageInDays`, `ageInHours`, `ageInMonths`,
  `ageInWeeks`, `ageInYears`, `calendarWeek`, `day`, `month`, `negate`, `numberAsTime`, `quarter`,
  `rank`, `round`, `roundToHundredths`, `roundToTenths`, `weekOfYear`, `year`.

**Returns:** `nlobjSearchColumn` (the same column).

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchColumn.setLabel(label)
<!-- id: ss1:nlobjSearchColumn#setLabel -->

Sets a custom label. Returns the same column, so calls can be chained.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `nlobjSearchColumn` (the same column).

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchColumn.setSort(order)
<!-- id: ss1:nlobjSearchColumn#setSort -->

Sorts the results by this column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `order` | boolean | No | `false` |

- `order`: `true` sorts descending, `false` ascending.

**Returns:** `nlobjSearchColumn`

**Governance:** 0 units.

**Since:** 2010.1

#### nlobjSearchColumn.setWhenOrderedBy(name, join)
<!-- id: ss1:nlobjSearchColumn#setWhenOrderedBy -->

For a column summarized as `min` or `max`, returns this column's value taken from the row that holds
the minimum or maximum of another column (named by `name` and `join`). The column's summary type
must be `min` or `max`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | No | |
| `join` | string | No | |

> [!SOURCE-ERROR] The source does not say whether `name` and `join` are required; the spec marks
> them optional only because nothing is stated. When repairing a call, keep the arguments it
> already passes rather than dropping either one.
<!-- source-error: ss1:nlobjSearchColumn#setWhenOrderedBy | name | param_requiredness_not_stated -->
<!-- source-error: ss1:nlobjSearchColumn#setWhenOrderedBy | join | param_requiredness_not_stated -->

**Returns:** `nlobjSearchColumn`

**Governance:** 0 units.

**Since:** 2012.1

#### nlobjSearchColumn.getFormula()
<!-- id: ss1:nlobjSearchColumn#getFormula -->

Returns the column's formula.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjSearchColumn.getFunction()
<!-- id: ss1:nlobjSearchColumn#getFunction -->

Returns the special function applied to the column.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjSearchColumn.getJoin()
<!-- id: ss1:nlobjSearchColumn#getJoin -->

Returns the column's join ID.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjSearchColumn.getName()
<!-- id: ss1:nlobjSearchColumn#getName -->

Returns the column's name.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjSearchColumn.getSort()
<!-- id: ss1:nlobjSearchColumn#getSort -->

Returns the column's sort order.

**Returns:** `string`. The source does not state which strings it returns. Note that `setSort`
takes a boolean, so do not compare the result with `true` or `false` without checking first.

**Governance:** 0 units.

**Since:** 2011.1

### nlobjSearchFilter (object)
<!-- id: ss1:nlobjSearchFilter -->

One filter condition. Filters passed as a list are combined with an implicit AND. A checkbox filter
compares against `'T'` or `'F'`, and the special value `@NONE@` matches an empty value. Operators
valid for each field type are in the
[search operator catalog](../gen/catalogs/search_operator_field_types.md); date keywords are in the
[date filter catalog](../gen/catalogs/search_date_filters.md).

#### new nlobjSearchFilter(name, join, operator, value1, value2)
<!-- id: ss1:nlobjSearchFilter#constructor -->

The object's constructor: call it with `new` to create a filter.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | No | |
| `join` | string | No | |
| `operator` | string | No | |
| `value1` | string \| Date \| string[] \| number | No | |
| `value2` | string \| Date | No | |

- `name`: the field to filter on, for example `quantityavailable`.
- `join`: the join ID, which is the record type of the joined field.
- `operator`: when `contains` times out, use `haskeywords` instead.
- `value1`: a plain value, a special date value, an array (for select fields) or an integer.
- `value2`: the second value for operators such as `between` or `within`; matched without regard to
  case.

> [!SOURCE-ERROR] The source does not say which of these arguments are required; the spec marks each
> one optional only because nothing is stated. Keep the arguments an existing script passes, in
> their positions.
<!-- source-error: ss1:nlobjSearchFilter#constructor | name | param_requiredness_not_stated -->
<!-- source-error: ss1:nlobjSearchFilter#constructor | join | param_requiredness_not_stated -->
<!-- source-error: ss1:nlobjSearchFilter#constructor | operator | param_requiredness_not_stated -->
<!-- source-error: ss1:nlobjSearchFilter#constructor | value1 | param_requiredness_not_stated -->
<!-- source-error: ss1:nlobjSearchFilter#constructor | value2 | param_requiredness_not_stated -->

> [!SOURCE-ERROR] The source's cell for `value2` carries a stray bullet with a date keyword
> (`lastbusinessweek`) that does not belong to this parameter. Take date keywords from the date
> filter catalog instead.
<!-- source-error: ss1:nlobjSearchFilter#constructor | value2 | cell_has_stray_bullet_lastbusinessweek -->

**Returns:** `nlobjSearchFilter`

**Governance:** 0 units.

The source states no release for the constructor.

#### nlobjSearchFilter.setFormula(formula)
<!-- id: ss1:nlobjSearchFilter#setFormula -->

Sets the formula of a formula filter, that is one named `formulatext`, `formulanumeric`,
`formuladatetime`, `formulapercent` or `formulacurrency`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `formula` | string | Yes | |

**Returns:** `nlobjSearchFilter`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchFilter.setSummaryType(type)
<!-- id: ss1:nlobjSearchFilter#setSummaryType -->

Makes the filter apply to summarized values. The filter's name must match one of the search's
columns.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

- `type`: `max`, `min`, `avg`, `sum` or `count`; `avg` and `sum` work on numeric or currency fields
  only.

**Returns:** `nlobjSearchFilter`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchFilter.getFormula()
<!-- id: ss1:nlobjSearchFilter#getFormula -->

Returns the filter's formula.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchFilter.getJoin()
<!-- id: ss1:nlobjSearchFilter#getJoin -->

Returns the filter's join ID.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjSearchFilter.getName()
<!-- id: ss1:nlobjSearchFilter#getName -->

Returns the name of the field the filter tests.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjSearchFilter.getSummaryType()
<!-- id: ss1:nlobjSearchFilter#getSummaryType -->

Returns the filter's summary type.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjSearchFilter.getOperator()
<!-- id: ss1:nlobjSearchFilter#getOperator -->

Returns the filter's operator.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjSearchResult (object)
<!-- id: ss1:nlobjSearchResult -->

One result row, returned by `nlapiSearchDuplicate`, `nlapiSearchGlobal`, `nlapiSearchRecord` and
`nlobjSearchResultSet.getResults`.

#### nlobjSearchResult.getRecordType()
<!-- id: ss1:nlobjSearchResult#getRecordType -->

Returns the record type of the row, such as `customer`, `assemblyitem`, `contact` or `projecttask`.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjSearchResult.getText(column)
<!-- id: ss1:nlobjSearchResult#getText -->

Returns the display text of a column, for select fields and for non-stored select, image and
document fields. It has two forms: `getText(column)`, taking an `nlobjSearchColumn` (shown in the
table), and `getText(name, join, summary)`, where `name` is a required string and `join` and
`summary` are optional strings.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `column` | nlobjSearchColumn | Yes | |

**Returns:** `string`, the display text.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjSearchResult.getValue(name, join, summary)
<!-- id: ss1:nlobjSearchResult#getValue -->

Returns a column's value. It has two forms: `getValue(name, join, summary)` (shown in the table) and
`getValue(column)`, which takes a required `nlobjSearchColumn`. The column form works for formula
columns as well as standard ones.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `join` | string | No | |
| `summary` | string | No | |

- `summary`: `group`, `sum`, `count`, `avg`, `min` or `max`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjSearchResult.getAllColumns()
<!-- id: ss1:nlobjSearchResult#getAllColumns -->

Returns the columns of the row.

**Returns:** `nlobjSearchColumn[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjSearchResult.getId()
<!-- id: ss1:nlobjSearchResult#getId -->

Returns the internal ID of the record behind the row.

**Returns:** `number`

**Governance:** 0 units.

The source states no release for this method.

### nlobjSearchResultSet (object)
<!-- id: ss1:nlobjSearchResultSet -->

The result set returned by `nlobjSearch.runSearch`. You can walk it with an iterator
(`forEachResult`) or read slices of up to 1,000 rows (`getResults`).

#### nlobjSearchResultSet.forEachResult(callback)
<!-- id: ss1:nlobjSearchResultSet#forEachResult -->

Calls `callback` once per row, for at most 4,000 rows. Whatever the callback does counts toward the
script's usage limit.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `callback` | Function | Yes | |

- `callback`: receives an `nlobjSearchResult` and returns a boolean: `true` to carry on, `false` to
  stop.

**Returns:** `void`

**Governance:** 10 units.

**Since:** 2012.1

#### nlobjSearchResultSet.getResults(start, end)
<!-- id: ss1:nlobjSearchResultSet#getResults -->

Returns a slice of rows. One call returns at most 1,000 rows, but by moving the window you can read
any number of rows in total. You get fewer entries when fewer results exist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `start` | number | Yes | |
| `end` | number | Yes | |

- `start`: index of the first row, counted from 0, included.
- `end`: index after the last row, excluded.

**Returns:** `nlobjSearchResult[]`

**Errors:**

- `SSS_INVALID_SEARCH_RESULT_INDEX`: an argument value is invalid.
- `SSS_SEARCH_RESULT_LIMIT_EXCEEDED`: the slice exceeds the per-call limit.

**Governance:** 10 units.

**Since:** 2012.1

#### nlobjSearchResultSet.getColumns()
<!-- id: ss1:nlobjSearchResultSet#getColumns -->

Returns the columns of the result set.

**Returns:** `nlobjSearchColumn[]`

**Governance:** 0 units.

**Since:** 2012.1

## Select options

### nlobjSelectOption (object)
<!-- id: ss1:nlobjSelectOption -->

One choice of a select field, returned by `nlobjField.getSelectOptions` (see
[objects-forms.md](objects-forms.md)).

#### nlobjSelectOption.getId()
<!-- id: ss1:nlobjSelectOption#getId -->

Returns the option's internal ID.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjSelectOption.getText()
<!-- id: ss1:nlobjSelectOption#getText -->

Returns the option's display label.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

## Examples

These are 1.0 snippets of the kind found in legacy accounts, shown so you can recognise the
patterns. Convert them with the [conversion guide](../conversion/ss1-to-ss21.md) rather than copying
them into new scripts.

Reading purchase order lines. The loop starts at 1 and runs to the count inclusive; a legacy loop
written `for (i = 0; i < count; i++)` misses the last line and asks for a line 0 that does not
exist, which is the usual repair.

```js
function listUnclosedPoLines(poId) {
  var po = nlapiLoadRecord('purchaseorder', poId);
  var count = po.getLineItemCount('item');
  var open = [];
  for (var line = 1; line <= count; line++) {
    if (po.getLineItemValue('item', 'isclosed', line) === 'T') {
      continue;
    }
    var ordered = parseFloat(po.getLineItemValue('item', 'quantity', line)) || 0;
    var received = parseFloat(po.getLineItemValue('item', 'quantityreceived', line)) || 0;
    open.push({
      line: line,
      item: po.getLineItemText('item', 'item', line),
      outstanding: ordered - received
    });
  }
  return open;
}
```

Removing several lines in a server script with `ignoreRecalc`. Every removal except the final one
skips the total recalculation; the last one recalculates so the submit succeeds. Working from the
bottom of the sublist upwards keeps the remaining line numbers valid (general JavaScript logic, not
a SuiteScript rule).

```js
function dropEmptyPoLines(poId) {
  var po = nlapiLoadRecord('purchaseorder', poId);
  var emptyLines = [];
  for (var line = 1; line <= po.getLineItemCount('item'); line++) {
    if ((parseFloat(po.getLineItemValue('item', 'quantity', line)) || 0) === 0) {
      emptyLines.push(line);
    }
  }
  for (var i = emptyLines.length - 1; i >= 0; i--) {
    var isFinalRemoval = (i === 0);
    po.removeLineItem('item', emptyLines[i], !isFinalRemoval);
  }
  return nlapiSubmitRecord(po);
}
```

An ad hoc support case search read in pages with `getResults`. Each page costs 10 units, and the
loop stops when a page comes back short. The `@NONE@` filter finds unassigned cases.

```js
function unassignedCasesForPriority(priorityId) {
  var filters = [
    new nlobjSearchFilter('priority', null, 'anyof', [priorityId]),
    new nlobjSearchFilter('assigned', null, 'anyof', '@NONE@'),
    new nlobjSearchFilter('isinactive', null, 'is', 'F')
  ];
  var columns = [
    new nlobjSearchColumn('casenumber'),
    new nlobjSearchColumn('title'),
    new nlobjSearchColumn('startdate').setSort(false)
  ];
  var resultSet = nlapiCreateSearch('supportcase', filters, columns).runSearch();
  var summaries = [];
  var start = 0;
  var page;
  do {
    page = resultSet.getResults(start, start + 1000);
    for (var i = 0; i < page.length; i++) {
      summaries.push(page[i].getValue('casenumber') + ': ' + page[i].getValue('title'));
    }
    start += page.length;
  } while (page.length === 1000);
  return summaries;
}
```

The same kind of walk with `forEachResult`, which stops at 4,000 rows. The callback must return
`true` to keep going.

```js
function countCasesForCompany(companyId) {
  var search = nlapiCreateSearch('supportcase', [
    new nlobjSearchFilter('company', null, 'anyof', [companyId])
  ], [new nlobjSearchColumn('casenumber')]);
  var seen = 0;
  search.runSearch().forEachResult(function (result) {
    seen += 1;
    return true;
  });
  return seen;
}
```
