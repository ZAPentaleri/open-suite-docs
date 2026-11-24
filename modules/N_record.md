# N/record
<!-- id: N/record -->

`N/record` is the module that works on whole records: you create, copy, load, transform, change
and delete them through it. It handles both standard record types and instances of custom record
types. Everything goes through SuiteScript calls; a script never reaches into the page's DOM to read
or change a form, even in a browser.

Most of the work happens on a [`Record`](#record-object) object, which runs in one of two modes
(see [record modes](../topics/record_modes.md)):

- **Standard mode** (the default for `create`, `copy`, `load` and `transform`): you set body and
  line values in any order. Sourcing, calculations and validation are held back until the record is
  saved, and NetSuite sorts out the order at that point. Lines are addressed by index with
  `setSublistValue` and friends. Some source pages call this mode "deferred dynamic"; it is the same
  thing.
- **Dynamic mode** (`isDynamic: true`): the record behaves like a form in the UI. Sourcing,
  calculation and validation happen as each value is set, so you must set values in the order a
  user would. Lines are edited by selecting one, setting values on the "current" line, and
  committing it.

Initialization defaults: [`record.create`](#recordcreateoptions), [`record.copy`](#recordcopyoptions),
[`record.transform`](#recordtransformoptions) and [`record.load`](#recordloadoptions) all accept
`options.defaultValues`, a set of field/value pairs used to initialize the record. On any record
whose form can be customized you can pass `customform` with the internal id of the custom form to
use. Tax group and tax type records can be initialized through `countrycodeid`. Which defaults each
record type accepts is listed in [record initialization defaults](../gen/catalogs/record_initialization_defaults.md).

Select options: the module-level material says [`Field.getSelectOptions`](#fieldgetselectoptionsoptions)
returns at most 1,000 options.

> [!SOURCE-ERROR] The source disagrees with itself about the cap on `getSelectOptions`: the module
> overview says 1,000 options, while the method's own pages say 500 (the figure kept on the method
> below). The spec does not say which is right; do not rely on getting more than 500 back.
<!-- source-error: N/record | - | getselectoptions_pages_say_500_max -->

- **Availability:** client and server scripts.
- **Since:** the source does not give a release for the module as a whole; its members date from
  2015.2 onward (per member below).
- **Load it as:** `define(['N/record'], (record) => { ... })`
- Related: [record modes](../topics/record_modes.md), [record transformations](../gen/catalogs/record_transformations.md),
  [generated record reference](../gen/records/index.md), [N/currentRecord](N_currentRecord.md),
  [N/search](N_search.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`record.create(options)`](#recordcreateoptions) | function | Start a new record |
| [`record.copy(options)`](#recordcopyoptions) | function | Start a new record from a copy of an existing one |
| [`record.load(options)`](#recordloadoptions) | function | Open an existing record |
| [`record.transform(options)`](#recordtransformoptions) | function | Build a record of another type from an existing record |
| [`record.delete(options)`](#recorddeleteoptions) | function | Delete a record |
| [`record.submitFields(options)`](#recordsubmitfieldsoptions) | function | Update body fields without loading the record |
| [`record.attach(options)`](#recordattachoptions) | function | Attach one record to another |
| [`record.detach(options)`](#recorddetachoptions) | function | Detach one record from another |
| `record.<fn>.promise(options)` | function | Client-side Promise forms of the eight functions above |
| [`Record`](#record-object) | object | One record, in standard or dynamic mode |
| [`Field`](#field-object) | object | Metadata for a body or sublist field |
| [`Sublist`](#sublist-object) | object | Metadata for a sublist |
| [`Column`](#column-object) | object | Metadata for a sublist column |
| [`Macro`](#macro-object) | object | A record macro |
| [`record.Type`](#recordtype-1) | enum | Record type ids for standard records |

## Functions

### record.create(options)
<!-- id: N/record.create -->

Starts a brand-new record of the given type and hands it back unsaved; nothing is written until you
call [`Record.save`](#recordsaveoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

- `options.type`: a [`record.Type`](#recordtype-1) value for standard records, or the string id of
  the custom record type. It becomes the record's [`Record.type`](#recordtype), which cannot be
  changed afterwards.
- `options.isDynamic`: `true` creates the record in dynamic mode; otherwise it is in standard mode
  ([record modes](../topics/record_modes.md)).
- `options.defaultValues`: field-name/value pairs to initialize the record with. If omitted
  (`null`), no defaults are applied. Some record types cannot be created without particular
  defaults, so check which defaults a type supports before relying on them
  ([catalog](../gen/catalogs/record_initialization_defaults.md)). For example, creating a script
  deployment record requires `script` set to the internal id of an existing script (such as a
  deployment for script `205`).

**Returns:** `record.Record`, the new unsaved record.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client and server scripts. Client scripts can also use [`record.create.promise`](#recordcreatepromiseoptions).

**Since:** 2015.2

### record.create.promise(options)
<!-- id: N/record.create.promise -->

The asynchronous form of [`record.create`](#recordcreateoptions). It takes the same parameters and
raises the same errors, but returns a Promise that resolves to the new record. Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.copy(options)
<!-- id: N/record.copy -->

Creates a new, unsaved record whose contents start as a copy of an existing record of the same
type.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

- `options.type`: a [`record.Type`](#recordtype-1) value, or a custom record type's string id.
- `options.id`: internal id of the record to copy.
- `options.isDynamic`: `true` gives a dynamic-mode record.
- `options.defaultValues`: field/value pairs used to initialize the copy; the allowed values are
  the ones in the N/record default-values table
  ([record initialization defaults](../gen/catalogs/record_initialization_defaults.md)).

**Returns:** `record.Record`, the unsaved copy.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### record.copy.promise(options)
<!-- id: N/record.copy.promise -->

Promise form of [`record.copy`](#recordcopyoptions), with the same parameters and errors. Client
scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.load(options)
<!-- id: N/record.load -->

Opens an existing record so you can read or change it. Only records that have already been saved
can be loaded, so save a new record before trying to load it. A loaded record can hold at most
10,000 lines per sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

- `options.type`: a [`record.Type`](#recordtype-1) value, or a custom record type's string id.
- `options.id`: the record's internal id, which is the id shown on the list page for that record
  type.
- `options.isDynamic`: `true` loads the record in dynamic mode.
- `options.defaultValues`: field/value pairs used to initialize the record.
- The source gives no release for `options.id` or `options.defaultValues`; the method and its other
  parameters date from 2015.2.

**Returns:** `record.Record`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client and server scripts. The Promise version
([`record.load.promise`](#recordloadpromiseoptions)) is for client scripts only.

**Since:** 2015.2

### record.load.promise(options)
<!-- id: N/record.load.promise -->

Promise form of [`record.load`](#recordloadoptions), with the same parameters and errors; resolves to
the loaded record. Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.defaultValues` | Object | No | |
| `options.type` | string | Yes | |
| `options.id` | number | Yes | |
| `options.isDynamic` | boolean | No | `false` |

- `options.defaultValues`: when omitted, no defaults are applied; this page describes that default
  as empty.
- The source gives no release for `options.id` or `options.defaultValues`.

> [!SOURCE-ERROR] The Promise page describes the default of `options.defaultValues` as empty, while
> the synchronous `record.load` page lists `null`. Either way nothing is pre-filled; do not test for
> a specific default value in your code.
<!-- source-error: N/record.load.promise | options.defaultValues | defaultvalues_default_empty_vs_load_page_null -->

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.transform(options)
<!-- id: N/record.transform -->

Produces a new, unsaved record of a different type, pre-filled from an existing record. This is
the usual way to automate order processing, for instance turning a sales order into an item
fulfillment or an invoice. Which source/target pairs are allowed is listed in the
[record transformations catalog](../gen/catalogs/record_transformations.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fromType` | string | Yes | |
| `options.fromId` | number | Yes | |
| `options.toType` | string | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

- `options.fromType`: the source record's type, from [`record.Type`](#recordtype-1). The spec links it
  to the [`Record.type`](#recordtype) property, which is read-only once the record is loaded.
- `options.fromId`: internal id of the source record.
- `options.toType`: the type of the record you want back.
- `options.isDynamic`: `true` returns the new record in dynamic mode.
- `options.defaultValues`: field/value pairs used to initialize the result.

**Returns:** `record.Record`, the transformed record (not yet saved).

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

Caveats:

- When transforming a customer into a customer payment, do not set the customer field yourself.
- Not supported: creating an assembly build from an outsourcing work order, and creating an item
  receipt from a drop-ship purchase order.
- To transform into a revenue commitment reversal, the return authorization must already be
  approved and received.

### record.transform.promise(options)
<!-- id: N/record.transform.promise -->

Promise form of [`record.transform`](#recordtransformoptions), with the same parameters and errors.
The same [transformation list](../gen/catalogs/record_transformations.md) applies. Client scripts
only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fromType` | string | Yes | |
| `options.fromId` | number | Yes | |
| `options.toType` | string | Yes | |
| `options.isDynamic` | boolean | No | `false` |
| `options.defaultValues` | Object | No | `null` |

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.delete(options)
<!-- id: N/record.delete -->

Deletes a record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number \| string | Yes | |

- `options.type`: a [`record.Type`](#recordtype-1) value, or a custom record type's string id.
- `options.id`: internal id of the record to delete.

**Returns:** `number`, the internal id of the record that was deleted.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 20 units; custom records 4 units; other standard records 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### record.delete.promise(options)
<!-- id: N/record.delete.promise -->

Promise form of [`record.delete`](#recorddeleteoptions), with the same parameters and errors. Client
scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number \| string | Yes | |

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 20 units; custom records 4 units; other standard records 10 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.submitFields(options)
<!-- id: N/record.submitFields -->

Writes new values into body fields of an existing record in one call, so you do not have to load
the record and save it yourself. It works for standard and custom body fields that support inline
editing, including select and multi-select fields. It cannot change sublist line fields or fields
on subrecords.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number \| string | Yes | |
| `options.values` | Object | Yes | |
| `options.options` | Object | No | |
| `options.options.enableSourcing` | boolean | No | `true` |
| `options.options.ignoreMandatoryFields` | boolean | No | `false` |

- `options.type`: for a standard record, a [`record.Type`](#recordtype-1) value; for a custom record,
  the custom record type's script id.
- `options.values`: an object mapping field ids to new values. Pass a string for text, radio,
  select and multi-select fields; a boolean for checkboxes; a `Date` for date and date/time fields;
  a number for integer, float, currency and percent fields.
- `options.options.ignoreMandatoryFields`: even when `true`, standard fields that are mandatory by
  default are still enforced.

**Returns:** `number`, the internal id of the parent record that was updated.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### record.submitFields.promise(options)
<!-- id: N/record.submitFields.promise -->

Promise form of [`record.submitFields`](#recordsubmitfieldsoptions), with the same parameters and
errors; available in client scripts only. It resolves to the internal id of the parent record, and
like the synchronous version spares you loading and submitting that record. It edits standard and
custom body fields that support inline editing, and cannot edit sublist line item fields or
subrecord fields. This variant's material also lists select fields as not editable.

> [!SOURCE-ERROR] The Promise version's description excludes select fields, while the synchronous
> `submitFields` lists select and multi-select fields as supported. The spec does not say which is
> right; test select fields in the client before relying on this variant for them.
<!-- source-error: N/record.submitFields.promise | - | select_fields_excluded_unlike_sync_version -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | number \| string | Yes | |
| `options.values` | Object | Yes | |
| `options.options` | Object | No | |
| `options.options.enableSourcing` | boolean | No | `true` |
| `options.options.ignoreMandatoryFields` | boolean | No | `false` |

**Returns:** `Promise`, resolving to the parent record's internal id.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** transaction records 10 units; custom records 2 units; other standard records 5 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.attach(options)
<!-- id: N/record.attach -->

Attaches one record to another. Either side can also be a File Cabinet file by using the type
`'file'`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.record` | record.Record | Yes | |
| `options.record.type` | string | Yes | |
| `options.record.id` | number \| string | Yes | |
| `options.to` | record.Record | Yes | |
| `options.to.type` | string | Yes | |
| `options.to.id` | number \| string | Yes | |
| `options.attributes` | Object | No | `null` |

- `options.record`: the record being attached. It must be active. When given as a plain object it
  needs both `type` and `id`.

> [!SOURCE-ERROR] `options.record` is typed as a `record.Record`, but the source uses it as a plain
> object with `type` and `id`. Passing `{ type, id }` matches how the parameter is described; verify
> before passing a loaded `Record` instead.
<!-- source-error: N/record.attach | options.record | record_Record_type_but_used_as_type_id_object -->

- `options.to`: the record that receives the attachment.

> [!SOURCE-ERROR] `options.to` has the same mismatch: typed as `record.Record` but used as a
> `{ type, id }` object. Prefer the plain object form and verify in your account.
<!-- source-error: N/record.attach | options.to | record_Record_type_but_used_as_type_id_object -->

- `options.record.type` / `options.to.type`: a [`record.Type`](#recordtype-1) value, or `'file'` to
  attach a File Cabinet file.
- `options.attributes`: name/value pairs describing the attachment.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 10 units.

**Availability:** the spec gives no client/server availability for this function.

> [!SOURCE-ERROR] The source page for `attach` is missing its "supported script types" row, so which
> script types may call it is not documented. Check it in your account before using it in a
> particular script type.
<!-- source-error: N/record.attach | - | supported_script_types_row_missing -->

**Since:** 2015.2

### record.attach.promise(options)
<!-- id: N/record.attach.promise -->

Promise form of [`record.attach`](#recordattachoptions), with the same parameters and errors.
Client scripts only. `options.record` and `options.to` carry the same `record.Record` typing as the
synchronous version while being described as `{ type, id }` objects.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.record` | record.Record | Yes | |
| `options.record.type` | string | Yes | |
| `options.record.id` | number \| string | Yes | |
| `options.to` | record.Record | Yes | |
| `options.to.type` | string | Yes | |
| `options.to.id` | number \| string | Yes | |
| `options.attributes` | Object | No | `null` |

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 10 units.

**Availability:** client scripts.

**Since:** 2015.2

### record.detach(options)
<!-- id: N/record.detach -->

Removes an attachment between two records.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.record` | record.Record | Yes | |
| `options.record.type` | string | Yes | |
| `options.record.id` | number \| string | Yes | |
| `options.from` | record.Record | Yes | |
| `options.from.type` | string | Yes | |
| `options.from.id` | number \| string | Yes | |
| `options.attributes` | Object | No | `null` |

- `options.record`: the record to detach. Its `type` comes from [`record.Type`](#recordtype-1).

> [!SOURCE-ERROR] `options.record` is typed as `record.Record`, yet it is described as an object
> with `type` and `id`. Pass `{ type, id }` and verify before passing a loaded `Record`.
<!-- source-error: N/record.detach | options.record | record_Record_type_but_used_as_type_id_object -->

- `options.from`: the record it is detached from. Its `type` comes from [`record.Type`](#recordtype-1).

> [!SOURCE-ERROR] `options.from` has the same mismatch between its `record.Record` type and its
> `{ type, id }` description; prefer the plain object and verify.
<!-- source-error: N/record.detach | options.from | record_Record_type_but_used_as_type_id_object -->

- `options.attributes`: an optional object.

> [!SOURCE-ERROR] The source describes `options.attributes` here as default values for a new record,
> which looks copied from `create` and makes no sense for a detach. Treat its meaning as
> undocumented and verify before passing anything.
<!-- source-error: N/record.detach | options.attributes | described_as_default_values_of_new_record -->

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### record.detach.promise(options)
<!-- id: N/record.detach.promise -->

Promise form of [`record.detach`](#recorddetachoptions), with the same parameters and errors.
Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.record` | record.Record | Yes | |
| `options.record.type` | string | Yes | |
| `options.record.id` | number \| string | Yes | |
| `options.from` | record.Record | Yes | |
| `options.from.type` | string | Yes | |
| `options.from.id` | number \| string | Yes | |
| `options.attributes` | Object | No | `null` |

**Returns:** `Promise`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 10 units.

**Availability:** client scripts.

**Since:** 2015.2

## Objects

### Record (object)
<!-- id: N/record.Record -->

A single NetSuite record held in memory. You get one from [`record.create`](#recordcreateoptions),
[`record.copy`](#recordcopyoptions), [`record.load`](#recordloadoptions) or
[`record.transform`](#recordtransformoptions), and the `isDynamic` option you pass there fixes the
mode it runs in (standard or dynamic; see [record modes](../topics/record_modes.md)). Available in
client and server scripts since 2015.2.

Quick guide to the line-editing methods by mode:

| Task | Standard mode | Dynamic mode |
|---|---|---|
| Write a line field | `setSublistValue` / `setSublistText` | `selectLine` or `selectNewLine`, then `setCurrentSublistValue` / `setCurrentSublistText`, then `commitLine` |
| Read a line field | `getSublistValue` / `getSublistText` | `getCurrentSublistValue` / `getCurrentSublistText`, or `getSublistValue` |
| Line subrecord | `getSublistSubrecord`, `hasSublistSubrecord`, `removeSublistSubrecord` | `getCurrentSublistSubrecord`, `hasCurrentSublistSubrecord`, `removeCurrentSublistSubrecord` |
| Matrix cell | `setMatrixSublistValue` | `setCurrentMatrixSublistValue` |

#### Record.id
<!-- id: N/record.Record#id -->

`number`, read-only. The record's internal id. Available in both modes; not available on
subrecords. Since 2015.2.

#### Record.type
<!-- id: N/record.Record#type -->

`string`, read-only. The record type: a [`record.Type`](#recordtype-1) value for standard records, or
the custom record type's string id for custom records. Available in both modes; not available on
subrecords. Since 2015.2.

#### Record.isDynamic
<!-- id: N/record.Record#isDynamic -->

`boolean`, read-only. `true` when the record is in dynamic mode, `false` for standard mode. It is
fixed when the record is created or opened. Since 2015.2.

- In standard mode, sourcing, calculations and validation are deferred until the record is
  saved. You can set fields in any order; NetSuite reorders the work when it submits.
- In dynamic mode, sourcing, calculations and validation run immediately, as they do in the UI.
  Set values in the same order a user would fill the form; setting them out of order can produce
  wrong results.

See [record modes](../topics/record_modes.md).

#### Record.getValue(options)
<!-- id: N/record.Record#getValue -->

Reads a body field's value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `number | Date | string | array | boolean`. Date and time fields (`DATE`, `DATETIME`,
`DATETIMETZ`, `TIMEOFDAY`) come back as `Date` objects, checkboxes as booleans, multi-selects as
arrays (for example an array of customer ids), and rate fields as numbers.

**Errors:**

- `SSS_INVALID_API_USAGE`: the record is in a state where reading the value is not allowed. In
  standard mode this happens if you have already used `setText` or `getText` on the same field.
- `SSS_MISSING_REQD_ARGUMENT`: `fieldId` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

- If you need a date/time field as a string, use [`getText`](#recordgettextoptions) instead.
  Converting the returned `Date` to a string yourself uses the browser's time zone.

#### Record.setValue(options)
<!-- id: N/record.Record#setValue -->

Writes a body field's value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |

- `options.value`: match the type to the field: string for text and radio fields; string or
  number for select; an array of strings or numbers for multi-select; boolean for checkbox; `Date`
  for date and date/time; number for integer, float, currency and percent; string for inline HTML.
  Inline HTML values are shown as HTML entities.
- `options.ignoreFieldChange`: `true` suppresses the field-change event and any secondary events
  it would trigger.

**Returns:** `record.Record`, the same record.

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is the wrong type for the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

- To give a rate field a percentage string, use [`setText`](#recordsettextoptions) instead.

#### Record.getText(options)
<!-- id: N/record.Record#getText -->

Reads a body field as display text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `string`, the text of the field's value. Multi-selects return an array; rate fields
return a percentage string.

**Errors:**

- `SSS_INVALID_API_USAGE`: invalid argument or usage. In standard mode: on a record built by
  `create` or `transform`, calling `getText` on a field before `setText` has been called on it; on a
  record obtained by `copy` or `load`, calling `getText` after `setValue` on the same field.
- `SSS_MISSING_REQD_ARGUMENT`: `fieldId` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

- The same standard-mode restrictions apply to `newRecord` and `oldRecord` in user event scripts,
  which are always in standard mode.
- In dynamic mode the [record modes](../topics/record_modes.md) material places no restrictions on
  `getText`.

> [!SOURCE-ERROR] The source groups `copy` with `load` for these `getText` rules, but groups `copy`
> with `create`/`transform` for the matching `getSublistText` rules. Which rule applies to a copied
> record is therefore uncertain; on copies, avoid mixing `getText` with `setValue`/`setText` on the
> same field.
<!-- source-error: N/record.Record#getText | - | copy_grouped_differently_than_getsublisttext -->

#### Record.setText(options)
<!-- id: N/record.Record#setText -->

Sets a body field by its display text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.text` | string \| array | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |

- `options.text`: a string; for a multi-select, an array of strings, or `null` to clear every
  selection. For inline HTML fields the tags are shown as literal text.
- `options.ignoreFieldChange`: `true` suppresses the field-change event and its secondary events.

**Returns:** `record.Record`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

- Rate fields take a string with a percent sign here.

#### Record.getField(options)
<!-- id: N/record.Record#getField -->

Returns the metadata object for a body field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** [`record.Field`](#field-object)

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `fieldId` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getFields()
<!-- id: N/record.Record#getFields -->

Lists the internal ids of every body field on the record, including matrix header fields. No
parameters.

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

- Custom fields are not returned in the order set in the custom record's UI.

#### Record.getSublists()
<!-- id: N/record.Record#getSublists -->

Lists the ids of all sublists on the record. No parameters.

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getSublist(options)
<!-- id: N/record.Record#getSublist -->

Returns the metadata object for one sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** [`record.Sublist`](#sublist-object)

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getSublistFields(options)
<!-- id: N/record.Record#getSublistFields -->

Lists the field ids available in a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `string[]`, the names of the fields in the sublist.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `sublistId` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getSublistField(options)
<!-- id: N/record.Record#getSublistField -->

Returns the metadata object for a field on a specific sublist line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** [`record.Field`](#field-object)

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getLineCount(options)
<!-- id: N/record.Record#getLineCount -->

Counts the lines in a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `number`, the number of lines.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.findSublistLineWithValue(options)
<!-- id: N/record.Record#findSublistLineWithValue -->

Searches a sublist column for a value and reports the first line that holds it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | No | |

**Returns:** `number`, the zero-based index of the first matching line, or `-1` when nothing
matches.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.insertLine(options)
<!-- id: N/record.Record#insertLine -->

Inserts an empty line at a position in a sublist. Lines from that position onward move down by
one and the line count grows.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.line` | number | Yes | |
| `options.ignoreRecalc` | boolean | No | `false` |

- `options.ignoreRecalc`: `true` skips the recalculation that scripting would otherwise trigger.

**Returns:** `record.Record`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.removeLine(options)
<!-- id: N/record.Record#removeLine -->

Deletes a line from a sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.line` | number | Yes | |
| `options.ignoreRecalc` | boolean | No | `false` |
| `options.lineInstanceId` | string | No | |

- `options.ignoreRecalc`: `true` skips scripting recalculation.
- `options.lineInstanceId`: identifies the line to remove.

**Returns:** `record.Record`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.moveLine(options)
<!-- id: N/record.Record#moveLine -->

Moves a sublist line to a new position. The other lines keep their relative order. It only works
on sublists of the edit-machine type whose machine allows moving lines and that have a sequence
field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.from` | number | Yes | |
| `options.to` | number | Yes | |
| `options.sublistId` | string | Yes | |

- `options.from`: zero-based index of the line to move.
- `options.to`: zero-based index of its new position.

**Returns:** `record.Record`

**Errors:**

- `SSS_NOT_YET_SUPPORTED`: the line index is invalid; the spec also says this is raised when you
  try to move a line other than the current one.
- `SSS_INVALID_SUBLIST_OPERATION`: an argument value is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.
- `SSS_SUBLIST_DOESNT_SUPPORT_MOVING_LINES`: the sublist does not allow lines to be moved.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.2 (method and all parameters).

#### Record.getSublistValue(options)
<!-- id: N/record.Record#getSublistValue -->

Reads a field value from a given sublist line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `number | Date | string | array | boolean`, the field's value. `rate` and
`ratehighprecision` fields come back as numbers.

**Errors:**

- `SSS_INVALID_API_USAGE`: invalid state. The spec says this is raised in standard mode when the
  call comes before `setSublistValue`; it gives no more detail about that condition.
- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.setSublistValue(options)
<!-- id: N/record.Record#setSublistValue -->

Writes a field value on a given sublist line. **Standard mode only.** In dynamic mode, use
[`selectLine`](#recordselectlineoptions), [`setCurrentSublistValue`](#recordsetcurrentsublistvalueoptions)
and [`commitLine`](#recordcommitlineoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |

- `options.line`: lines are numbered from 0.
- `options.value`: string for text, radio and select fields; boolean for checkboxes; `Date` for
  date and date/time; number for integer, float, currency and percent fields (rate fields take a
  number). One exception: the project resource role multi-select accepts only a single id. To
  pick several roles, use the UI.

**Returns:** `record.Record`

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid for the field.
- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- Setting the value runs NetSuite's internal line validation. If that validation fails (for
  example, because the line falls in a closed catch-up period), the script fails.

#### Record.getSublistText(options)
<!-- id: N/record.Record#getSublistText -->

Reads a sublist line field as display text. Listed for standard mode.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `string`, the field's text. Multi-selects return an array; rate fields return a
percentage string.

**Errors:**

- `SSS_INVALID_API_USAGE`: conflicting arguments or usage. On a new record from `copy`, `create`
  or `transform`, calling this before the field's text has been set raises it; so does calling it
  after a value was set on the field of a record obtained by `load`.
- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- These rules also cover `newRecord` and `oldRecord` in user event scripts. This page calls those
  records "deferred dynamic"; the [record modes](../topics/record_modes.md) material defines
  deferred dynamic mode as another name for standard mode, so this is the same rule as on
  [`getText`](#recordgettextoptions): user event records are always in standard mode.
- The source also notes that dynamic mode imposes no ordering limits on this call.

> [!SOURCE-ERROR] The method is listed as standard-mode only, yet the same material says it can be
> used without ordering limits in dynamic mode (compare `getText`, which works in both modes). The
> spec does not settle which is right; test it in dynamic mode before relying on it there.
<!-- source-error: N/record.Record#getSublistText | - | standard_mode_only_yet_usable_without_limit_in_dynamic -->

#### Record.setSublistText(options)
<!-- id: N/record.Record#setSublistText -->

Sets a sublist line field by its display text. **Standard mode only.** In dynamic mode, use
[`selectLine`](#recordselectlineoptions), [`setCurrentSublistText`](#recordsetcurrentsublisttextoptions)
and [`commitLine`](#recordcommitlineoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |
| `options.text` | string | Yes | |

**Returns:** `record.Record`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- Rate fields take a string with a percent sign.

#### Record.selectLine(options)
<!-- id: N/record.Record#selectLine -->

Makes an existing line the current line so you can edit it. **Dynamic mode.** Standard-mode code
writes lines directly with [`setSublistValue`](#recordsetsublistvalueoptions) instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `record.Record`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.selectNewLine(options)
<!-- id: N/record.Record#selectNewLine -->

Starts a new line at the end of the sublist and makes it current. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `record.Record`

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.commitLine(options)
<!-- id: N/record.Record#commitLine -->

Commits the current line, keeping the changes made since it was selected. **Dynamic mode.** In
standard mode, use [`setSublistValue`](#recordsetsublistvalueoptions) instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.ignoreRecalc` | boolean | No | `false` |

- `options.ignoreRecalc`: `true` skips scripting recalculation.

**Returns:** `record.Record`, the same record, so calls can be chained.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.cancelLine(options)
<!-- id: N/record.Record#cancelLine -->

Abandons the currently selected line without keeping its changes. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** `record.Record`, the same record, so calls can be chained.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.getCurrentSublistIndex(options)
<!-- id: N/record.Record#getCurrentSublistIndex -->

Tells you which line is currently selected. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |

- `options.sublistId`: the sublist's id as given in the records browser (see
  [generated record pages](../gen/records/index.md)).

**Returns:** `number`, the zero-based index of the selected line.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `sublistId` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.getCurrentSublistValue(options)
<!-- id: N/record.Record#getCurrentSublistValue -->

Reads a field on the currently selected line. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `number | Date | string | array | boolean`, the value of the field on the selected line.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.setCurrentSublistValue(options)
<!-- id: N/record.Record#setCurrentSublistValue -->

Writes a field on the currently selected line. **Dynamic mode.** Standard-mode code uses
[`setSublistValue`](#recordsetsublistvalueoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |
| `options.forceSyncSourcing` | boolean | No | `false` |

- `options.value`: string for text, radio and select fields; boolean for checkboxes; `Date` for
  date and date/time; number for integer, float, currency and percent fields (rate fields take a
  number). The project resource role multi-select accepts only one id; select several roles
  through the UI.
- `options.ignoreFieldChange`: `true` suppresses the field-change event and its secondary events.
- `options.forceSyncSourcing` (since 2019.1): `true` sources dependent fields synchronously,
  which avoids timing problems with sourcing in the browser.

**Returns:** `record.Record`

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is not valid for the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.
- Editing a read-only field, or any field while the sublist is in read-only mode, raises an error
  whose message says the sublist cannot be edited; the source gives the message only, no code.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- Internal line validation runs when the value is set; if it fails (for example, for a closed
  catch-up period), the script fails.

#### Record.getCurrentSublistText(options)
<!-- id: N/record.Record#getCurrentSublistText -->

Reads a field on the current line as display text. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.forceSyncSourcing` | boolean | No | `false` |

- `options.forceSyncSourcing` (since 2019.1): described as making dependent-field sourcing
  synchronous to avoid browser timing issues.

> [!SOURCE-ERROR] `forceSyncSourcing` is a setter's option, yet the source lists it on this read
> method (and not on `setCurrentSublistText`). Do not expect it to change what a read returns; leave
> it out unless you have verified its effect.
<!-- source-error: N/record.Record#getCurrentSublistText | options.forceSyncSourcing | setter_param_forcesyncsourcing_listed_on_getter -->

**Returns:** `string`, the field's text. Multi-selects give an array; rate fields give a
percentage string.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.setCurrentSublistText(options)
<!-- id: N/record.Record#setCurrentSublistText -->

Sets a field on the current line by its display text. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.text` | string | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |

- `options.ignoreFieldChange`: `true` suppresses the field-change event and its secondary events.

**Returns:** `record.Record`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.
- Editing a read-only sublist field, or a sublist in read-only mode, raises a read-only sublist
  error; the source gives a message, not a code.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- Rate fields take a string with a percent sign.

#### Record.getCurrentSublistField(options)
<!-- id: N/record.Record#getCurrentSublistField -->

Returns the metadata object for a field on the current line. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.sublistId` | string | Yes | |

**Returns:** [`record.Field`](#field-object), the sublist field's metadata.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts, but not inside the client script `fieldChanged` or
`postSourcing` entry points.

**Since:** 2016.2

> [!SOURCE-ERROR] Both parameters are listed as available since 2015.2, a release earlier than the
> method itself (2016.2). Treat 2016.2 as the earliest release you can count on.
<!-- source-error: N/record.Record#getCurrentSublistField | - | param_since_2015.2_predates_method_since_2016.2 -->

#### Record.getSubrecord(options)
<!-- id: N/record.Record#getSubrecord -->

Returns the subrecord stored in a body field. Not available when the record you call it on is
itself a subrecord.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `record.Record`, the subrecord.

**Errors:**

- `FIELD_1_IS_DISABLED_YOU_CANNOT_APPLY_SUBRECORD_OPERATION_ON_THIS_FIELD`: the feature behind the
  field is not enabled.
- `FIELD_1_IS_NOT_A_SUBRECORD_FIELD`: the field does not hold a subrecord.
- `SSS_MISSING_REQD_ARGUMENT`: `fieldId` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.hasSubrecord(options)
<!-- id: N/record.Record#hasSubrecord -->

Checks whether a body field holds a subrecord. Not available on subrecords.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `boolean`, `true` if the field has a subrecord.

**Errors:** the source lists none for this method.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.removeSubrecord(options)
<!-- id: N/record.Record#removeSubrecord -->

Removes the subrecord held in a body field. Not available on subrecords.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `record.Record`

**Errors:** the source lists none for this method.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getSublistSubrecord(options)
<!-- id: N/record.Record#getSublistSubrecord -->

Returns the subrecord held in a sublist field on a given line. **Standard mode.** In dynamic mode,
select the line with [`selectLine`](#recordselectlineoptions) and use
[`hasCurrentSublistSubrecord`](#recordhascurrentsublistsubrecordoptions) and
[`getCurrentSublistSubrecord`](#recordgetcurrentsublistsubrecordoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

- `options.line`: the line that holds the subrecord.

> [!SOURCE-ERROR] The source's description of `line` talks about column indexing, which does not fit
> a line parameter. Pass the sublist line index; if results look wrong, check that the line numbering
> is what you expect.
<!-- source-error: N/record.Record#getSublistSubrecord | - | line_param_says_column_indexing -->

**Returns:** `record.Record`, the subrecord.

**Errors:** the source lists none for this method.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.hasSublistSubrecord(options)
<!-- id: N/record.Record#hasSublistSubrecord -->

Checks whether a sublist field on a given line holds a subrecord. **Standard mode.** Not available
on subrecords.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.removeSublistSubrecord(options)
<!-- id: N/record.Record#removeSublistSubrecord -->

Removes the subrecord from a sublist field on a given line. **Standard mode.** In dynamic mode,
select the line, then use [`hasCurrentSublistSubrecord`](#recordhascurrentsublistsubrecordoptions),
[`removeCurrentSublistSubrecord`](#recordremovecurrentsublistsubrecordoptions) and
[`commitLine`](#recordcommitlineoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.line` | number | Yes | |

**Returns:** `record.Record`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.getCurrentSublistSubrecord(options)
<!-- id: N/record.Record#getCurrentSublistSubrecord -->

Returns the subrecord in a sublist field on the current line. **Dynamic mode.** The current
sublist must have subrecord fields. If you only need the field's metadata, use
[`getCurrentSublistField`](#recordgetcurrentsublistfieldoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `record.Record`, the subrecord.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.hasCurrentSublistSubrecord(options)
<!-- id: N/record.Record#hasCurrentSublistSubrecord -->

Checks whether a sublist field on the current line holds a subrecord. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.removeCurrentSublistSubrecord(options)
<!-- id: N/record.Record#removeCurrentSublistSubrecord -->

Removes the subrecord from a sublist field on the current line. **Dynamic mode.** Not available on
subrecords.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `record.Record`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.getMatrixHeaderCount(options)
<!-- id: N/record.Record#getMatrixHeaderCount -->

Counts the columns of a matrix field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `number`, the number of matrix columns.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getMatrixHeaderField(options)
<!-- id: N/record.Record#getMatrixHeaderField -->

Returns the metadata object for one matrix header column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

- `options.column`: zero-based column index.

**Returns:** [`record.Field`](#field-object)

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getMatrixHeaderValue(options)
<!-- id: N/record.Record#getMatrixHeaderValue -->

Reads the value of a matrix header column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

- `options.column`: zero-based column index.

**Returns:** `number | Date | string | array | boolean`; `rate` and `ratehighprecision` fields come
back as numbers.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.setMatrixHeaderValue(options)
<!-- id: N/record.Record#setMatrixHeaderValue -->

Writes the value of a matrix header column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |

- `options.column`: zero-based column index.
- `options.value`: string for text, radio and select fields; boolean for checkboxes; `Date` for
  date and date/time; number for integer, float, currency and percent fields. Rate fields take a
  number.
- `options.ignoreFieldChange`: `true` suppresses the field-change event and its secondary events.

**Returns:** `record.Record`

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is the wrong type for the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getMatrixSublistField(options)
<!-- id: N/record.Record#getMatrixSublistField -->

Returns the metadata object for a matrix cell, identified by line and column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.line` | number | Yes | |

**Returns:** [`record.Field`](#field-object)

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.getMatrixSublistValue(options)
<!-- id: N/record.Record#getMatrixSublistValue -->

Reads a matrix cell on a given line and column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.line` | number | Yes | |

- `options.column` and `options.line`: both zero-based.

**Returns:** `number | Date | string | array | boolean`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2

#### Record.setMatrixSublistValue(options)
<!-- id: N/record.Record#setMatrixSublistValue -->

Writes a matrix cell on a given line and column. **Standard mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.line` | number | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |

- `options.column`: zero-based column index.
- `options.value`: string for text, radio and select fields; boolean for checkboxes; `Date` for
  date and date/time; number for integer, float, currency and percent fields. Rate fields take a
  number.

**Returns:** `record.Record`

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is the wrong type for the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.getCurrentMatrixSublistValue(options)
<!-- id: N/record.Record#getCurrentMatrixSublistValue -->

Reads a matrix cell on the currently selected line. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |

- `options.column`: zero-based column index.

**Returns:** `number | Date | string | array | boolean`; `rate` and `ratehighprecision` fields come
back as numbers.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Record.setCurrentMatrixSublistValue(options)
<!-- id: N/record.Record#setCurrentMatrixSublistValue -->

Writes a matrix cell on the currently selected line. **Dynamic mode.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.value` | number \| Date \| string \| array \| boolean | Yes | |
| `options.ignoreFieldChange` | boolean | No | `false` |

- `options.column`: zero-based column index.
- `options.value`: string for text, radio and select fields; boolean for checkboxes; `Date` for
  date and date/time; number for integer, float, currency and percent fields.
- `options.ignoreFieldChange`: `true` suppresses the field-change event and its secondary events.

**Returns:** `record.Record`

**Errors:**

- `INVALID_FLD_VALUE`: `options.value` is the wrong type for the field.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- The spec says rate fields take a string with a percent sign here, while the other matrix setters
  take a number. That looks inconsistent; test both forms.

#### Record.findMatrixSublistLineWithValue(options)
<!-- id: N/record.Record#findMatrixSublistLineWithValue -->

Finds the first line whose matrix cell in a given column holds a value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublistId` | string | Yes | |
| `options.fieldId` | string | Yes | |
| `options.column` | number | Yes | |
| `options.value` | number | Yes | |

- `options.column`: zero-based column index.

**Returns:** `number`, the zero-based index of the first matching line in that matrix column.

**Errors:**

- `SSS_INVALID_SUBLIST_OPERATION`: an argument is invalid or the sublist cannot be edited.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts; both modes.

**Since:** 2015.2 for the method; every parameter is listed as since 2016.2.

> [!SOURCE-ERROR] The method is dated 2015.2, but all of its required parameters are dated 2016.2,
> so the method could not have been called as documented before 2016.2. Treat 2016.2 as the
> earliest release you can count on.
<!-- source-error: N/record.Record#findMatrixSublistLineWithValue | - | required_param_since_2016.2_after_method_since_2015.2 -->

#### Record.getMacros()
<!-- id: N/record.Record#getMacros -->

Lists the macros available on the record. No parameters.

**Returns:** `Object` whose keys are macro ids and whose values are [`Macro`](#macro-object)
objects; an empty object when the record has none.

**Errors:**

- `SSS_INVALID_RECORD_TYPE`: the record type is invalid (the record is in a state that does not
  allow macros).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Record.getMacro(options)
<!-- id: N/record.Record#getMacro -->

Looks up one macro by id and returns a function that runs it (see [`Macro`](#macro-object)).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `Function` that executes the macro.

**Errors:**

- `SSS_INVALID_MACRO_ID`: no macro with that id was found.
- `SSS_MISSING_REQD_ARGUMENT`: `id` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Record.executeMacro(options)
<!-- id: N/record.Record#executeMacro -->

Runs a macro on the record directly by id.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.params` | Object | No | |

- `options.id`: the macro's id.
- `options.params`: the arguments the macro takes.

**Returns:** `Object | null`, the macro's result object, or `null`.

**Errors:** the source lists none for this method.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Record.executeMacro.promise(options)
<!-- id: N/record.Record#executeMacro.promise -->

Promise form of [`executeMacro`](#recordexecutemacrooptions), with the same parameters and errors.
Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.params` | Object | No | |

**Returns:** `Promise`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

#### Record.save(options)
<!-- id: N/record.Record#save -->

Submits a new record or saves changes to an existing one. Not supported on subrecords.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.enableSourcing` | boolean | No | `false` |
| `options.ignoreMandatoryFields` | boolean | No | `false` |

- `options.enableSourcing`: in standard mode, `true` sources dependent values for fields that were
  left empty. Dynamic mode ignores it because sourcing always happens there.
- `options.ignoreMandatoryFields`: `true` skips the check on fields made mandatory by customization
  or by company preferences. Use it mainly in scheduled scripts, not in user event scripts: people
  working in the UI should not be able to get around the form's business rules.

**Returns:** `number`, the internal id of the saved record.

**Governance:** transaction records 20 units; custom records 4 units; other standard records 10 units.

**Availability:** client and server scripts. Client scripts can use the asynchronous
[`save.promise`](#recordsavepromiseoptions).

**Since:** 2015.2

- In standard mode, sourced and calculated values are not on the in-memory record after the save;
  load the record again to see them.

#### Record.save.promise(options)
<!-- id: N/record.Record#save.promise -->

Promise form of [`save`](#recordsaveoptions), with the same parameters and errors. Not available on
subrecords. Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.enableSourcing` | boolean | No | `false` |
| `options.ignoreMandatoryFields` | boolean | No | `false` |

**Returns:** `Promise`

**Governance:** transaction records 20 units; custom records 4 units; other standard records 10 units.

**Availability:** client scripts.

**Since:** 2015.2

### Field (object)
<!-- id: N/record.Field -->

Describes one body field or sublist field. You get it from
[`Record.getField`](#recordgetfieldoptions), [`Record.getSublistField`](#recordgetsublistfieldoptions)
or [`Record.getCurrentSublistField`](#recordgetcurrentsublistfieldoptions), and also from
`CurrentRecord.getField` and `CurrentRecord.getSublistField` in [N/currentRecord](N_currentRecord.md).
`id`, `isMandatory`, `label` and `type` work in both modes; `isDisplay`, `sublistId` and
`getSelectOptions` only work in dynamic mode ([record modes](../topics/record_modes.md)). Client and
server scripts, since 2015.2.

#### Field.id
<!-- id: N/record.Field#id -->

`string`, read-only. Internal id of the body or sublist field. Since 2015.2.

#### Field.label
<!-- id: N/record.Field#label -->

`string`, read-only. The label the body or sublist field shows in the UI. Since 2015.2.

#### Field.type
<!-- id: N/record.Field#type -->

`string`, read-only. The field type, as one of the `N/format.Type` values (text, date, currency,
select, checkbox and so on). The spec also attaches an 801-character maximum length for select
fields to this property. Since 2015.2.

#### Field.isMandatory
<!-- id: N/record.Field#isMandatory -->

`boolean`. `true` when the field is required on the form. Since 2015.2.

#### Field.isDisplay
<!-- id: N/record.Field#isDisplay -->

`boolean`. `true` when the field is visible on the form. Dynamic mode only, and only for body
fields, not sublist fields. Since 2015.2.

#### Field.sublistId
<!-- id: N/record.Field#sublistId -->

`string`, read-only. For a sublist field, the id of its sublist. Dynamic mode only. Since 2015.2.

#### Field.getSelectOptions(options)
<!-- id: N/record.Field#getSelectOptions -->

Lists the choices of a select, multi-select or radio field, filtered by text. **Dynamic mode only.**

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filter` | string | Yes | |
| `options.operator` | string | Yes | `contains` |

- `options.filter`: text to match against option labels; matching ignores case.
- `options.operator`: how `filter` is matched: `contains`, `is` or `startswith`.

> [!SOURCE-ERROR] `options.operator` is marked required, yet the source also gives it a default
> (`contains`). Pass it explicitly so the call does not depend on which of the two is true.
<!-- source-error: N/record.Field#getSelectOptions | options.operator | marked_required_but_has_default -->

**Returns:** `Object[]`, one entry per option, each pairing a value with its text. Returns `null`
for pop-up select fields, for fields not on the form, and when the number of options is over the
account's maximum-entries-in-dropdowns setting.

**Errors:**

- `TypeError`: the field's type does not support select options. The source gives no other error
  conditions or codes for this method.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- At most 500 options come back, a cap tied to the maximum-entries-in-dropdowns setting (see the
  source-error note on the module, which gives 1,000 elsewhere).
- Results can differ depending on the user's role.

### Sublist (object)
<!-- id: N/record.Sublist -->

Describes one sublist of a standard or custom record; get it with
[`Record.getSublist`](#recordgetsublistoptions). Client and server scripts, since 2015.2.

#### Sublist.id
<!-- id: N/record.Sublist#id -->

`string`, read-only. The sublist's internal id. Since 2015.2.

#### Sublist.type
<!-- id: N/record.Sublist#type -->

`string`, read-only. The sublist type, one of the `N/ui/serverWidget.SublistType` values written in
lowercase (for example `inlineeditor`). Since 2015.2.

#### Sublist.isChanged
<!-- id: N/record.Sublist#isChanged -->

`boolean`, read-only. `true` when the sublist has been changed on the form. Since 2015.2.

#### Sublist.isDisplay
<!-- id: N/record.Sublist#isDisplay -->

`boolean`. `true` when the sublist is shown on the form. Since 2015.2.

#### Sublist.getColumn(options)
<!-- id: N/record.Sublist#getColumn -->

Returns the metadata for one column of the sublist, for example while looping over the item lines
of a sales order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** [`record.Column`](#column-object)

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### Column (object)
<!-- id: N/record.Column -->

Describes one column of a sublist on a standard or custom record; get it with
[`Sublist.getColumn`](#sublistgetcolumnoptions). Client and server scripts, since 2015.2.

#### Column.id
<!-- id: N/record.Column#id -->

`string`, read-only. The column's internal id, which is the same `fieldId` you passed to
`getColumn`. Since 2015.2.

#### Column.label
<!-- id: N/record.Column#label -->

`string`, read-only. The column's label. Since 2015.2.

> [!SOURCE-ERROR] The source describes `label` as the column's internal id, which duplicates
> `Column.id`. The property name and the spec indicate it is the display label; verify the value
> before treating it as either.
<!-- source-error: N/record.Column#label | - | label_described_as_internal_id -->

#### Column.sublistId
<!-- id: N/record.Column#sublistId -->

`string`, read-only. Internal id of the sublist that contains the column. Since 2015.2.

#### Column.type
<!-- id: N/record.Column#type -->

`string`, read-only. The column's field type, one of the `N/format.Type` values. Since 2015.2.

#### Column.isDisabled
<!-- id: N/record.Column#isDisabled -->

`boolean`. `true` when the column is disabled. Since 2020.2.

#### Column.isDisplay
<!-- id: N/record.Column#isDisplay -->

`boolean`, read-only. `true` when the column is displayed. Since 2020.2.

#### Column.isMandatory
<!-- id: N/record.Column#isMandatory -->

`boolean`. `true` when the column is required. Since 2020.2.

#### Column.isSortable
<!-- id: N/record.Column#isSortable -->

`boolean`, read-only. `true` when the column can be sorted. Since 2020.2.

### Macro (object)
<!-- id: N/record.Macro -->

A record macro: an action defined for a record type that a script can run against a record. You
get one through [`Record.getMacro`](#recordgetmacrooptions), which returns a function that executes
it (or from the map returned by [`Record.getMacros`](#recordgetmacros)). The object and all of its
members are available in client and server scripts, since 2018.2; only the `.promise` forms are
limited to client scripts.

#### new Macro(options)
<!-- id: N/record.Macro#constructor -->

You do not call `Macro` by that name: you call the macro using its own name in place of `Macro`
(in practice, you call the function `getMacro` returned).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.params` | Object | No | |

**Returns:** `Object` with a `notifications` array and a `response` object.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Macro.execute(options)
<!-- id: N/record.Macro#execute -->

Runs the macro.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.params` | Object | No | |

- `options.params`: the arguments the macro takes.

**Returns:** `Object`, a plain object with a `notifications` array and a `response` object.

**Errors:** the source lists none for this method.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Macro.execute.promise(options)
<!-- id: N/record.Macro#execute.promise -->

Promise form of [`Macro.execute`](#macroexecuteoptions), with the same parameters and errors.
Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.params` | Object | No | |

**Returns:** `Promise`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

#### Macro.promise(options)
<!-- id: N/record.Macro#promise -->

The asynchronous way to call a macro: as with the constructor, you use the macro's own name in place
of `Macro`, followed by `.promise(...)`. Same parameters and errors as the
[constructor](#new-macrooptions). Client scripts only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.params` | Object | No | |

**Returns:** `Promise`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

#### Macro.id
<!-- id: N/record.Macro#id -->

`string`. The macro's id. Client and server scripts, since 2018.2.

#### Macro.label
<!-- id: N/record.Macro#label -->

`string`. The macro's label. Like the rest of the `Macro` object, available in client and server
scripts. Since 2018.2.

#### Macro.description
<!-- id: N/record.Macro#description -->

`string`. The macro's description. Available in client and server scripts, as for the whole
`Macro` object. Since 2018.2.

#### Macro.attributes
<!-- id: N/record.Macro#attributes -->

`Object`. The attributes defined for the macro. Available in client and server scripts, as for the
whole `Macro` object. Since 2018.2.

## Enums

### record.Type
<!-- id: N/record.Type -->

String ids of standard record types, used for `options.type`, `fromType`/`toType` and
[`Record.type`](#recordtype). For a custom record type, pass the custom record type's string id
instead. The record types you use must also suit your account's settings (features and
preferences). Since 2015.2.

The source gives the enum key names, but not the string each key stands for (the one exception is
`BANK_REC_SUGGESTION`, which an example maps to the record id `bankrecsuggestion`). It also states
no separate availability for the enum; it is part of `N/record`, which is available in client and
server scripts. Record ids and fields for each type are in the
[generated record reference](../gen/records/index.md).

> [!SOURCE-ERROR] The source prints the labor-based project revenue rule entry with a stray space,
> so its exact spelling is unreliable. The spec normalizes the key to
> `LABOR_BASED_PROJECT_REVENUE_RULE` [sic]; check the enum at runtime before depending on it.
<!-- source-error: N/record.Type | - | labor_based_project_revenue_rule_value_has_space -->
<!-- source-error: N/record.Type | LABOR_BASED_PROJECT_REVENUE_RULE | key_printed_with_stray_space -->

| Value | Note |
|---|---|
| `BANK_REC_SUGGESTION` | Maps to `bankrecsuggestion` (from an example) |
| `LABOR_BASED_PROJECT_REVENUE_RULE` [sic] | Key printed with a stray space in the source; normalized here |
| `ACCOUNT` | |
| `ACCOUNTING_BOOK` | |
| `ACCOUNTING_CONTEXT` | |
| `ACCOUNTING_PERIOD` | |
| `ADV_INTER_COMPANY_JOURNAL_ENTRY` | |
| `ALLOCATION_SCHEDULE` | |
| `AMORTIZATION_SCHEDULE` | |
| `AMORTIZATION_TEMPLATE` | |
| `ASSEMBLY_BUILD` | |
| `ASSEMBLY_ITEM` | |
| `ASSEMBLY_UNBUILD` | |
| `AUTOMATED_CLEARING_HOUSE` | |
| `BALANCE_TRX_BY_SEGMENTS` | |
| `BILLING_ACCOUNT` | |
| `BILLING_CLASS` | |
| `BILLING_RATE_CARD` | |
| `BILLING_REVENUE_EVENT` | |
| `BILLING_SCHEDULE` | |
| `BIN` | |
| `BIN_TRANSFER` | |
| `BIN_WORKSHEET` | |
| `BLANKET_PURCHASE_ORDER` | |
| `BOM` | |
| `BOM_REVISION` | |
| `BONUS` | |
| `BONUS_TYPE` | |
| `BUDGET_EXCHANGE_RATE` | |
| `BULK_OWNERSHIP_TRANSFER` | |
| `BUNDLE_INSTALLATION_SCRIPT` | |
| `CALENDAR_EVENT` | |
| `CAMPAIGN` | |
| `CAMPAIGN_RESPONSE` | |
| `CAMPAIGN_TEMPLATE` | |
| `CARDHOLDER_AUTHENTICATION` | |
| `CASH_REFUND` | |
| `CASH_SALE` | |
| `CHARGE` | |
| `CHARGE_RULE` | |
| `CHECK` | |
| `CLASSIFICATION` | |
| `CLIENT_SCRIPT` | |
| `CMS_CONTENT` | |
| `CMS_CONTENT_TYPE` | |
| `CMS_PAGE` | |
| `COMMERCE_CATEGORY` | |
| `COMPETITOR` | |
| `CONSOLIDATED_EXCHANGE_RATE` | |
| `CONTACT` | |
| `CONTACT_CATEGORY` | |
| `CONTACT_ROLE` | |
| `COST_CATEGORY` | |
| `COUPON_CODE` | |
| `CREDIT_CARD_CHARGE` | |
| `CREDIT_CARD_REFUND` | |
| `CREDIT_MEMO` | |
| `CURRENCY` | |
| `CUSTOM_PURCHASE` | |
| `CUSTOM_SALE` | |
| `CUSTOMER` | |
| `CUSTOMER_CATEGORY` | |
| `CUSTOMER_DEPOSIT` | |
| `CUSTOMER_MESSAGE` | |
| `CUSTOMER_PAYMENT` | |
| `CUSTOMER_PAYMENT_AUTHORIZATION` | |
| `CUSTOM_RECORD` | |
| `CUSTOMER_REFUND` | |
| `CUSTOMER_STATUS` | |
| `CUSTOMER_SUBSIDIARY_RELATIONSHIP` | |
| `CUSTOM_TRANSACTION` | |
| `DEPARTMENT` | |
| `DEPOSIT` | |
| `DEPOSIT_APPLICATION` | |
| `DESCRIPTION_ITEM` | |
| `DISCOUNT_ITEM` | |
| `DOWNLOAD_ITEM` | |
| `EMAIL_TEMPLATE` | |
| `EMPLOYEE` | |
| `EMPLOYEE_CHANGE_REQUEST` | |
| `EMPLOYEE_CHANGE_REQUEST_TYPE` | |
| `EMPLOYEE_EXPENSE_SOURCE_TYPE` | |
| `EMPLOYEE_STATUS` | |
| `EMPLOYEE_TYPE` | |
| `ENTITY_ACCOUNT_MAPPING` | |
| `ESTIMATE` | |
| `EXPENSE_AMORTIZATION_EVENT` | |
| `EXPENSE_CATEGORY` | |
| `EXPENSE_PLAN` | |
| `EXPENSE_REPORT` | |
| `EXPENSE_REPORT_POLICY` | |
| `FAIR_VALUE_PRICE` | |
| `FINANCIAL_INSTITUTION` | |
| `FIXED_AMOUNT_PROJECT_REVENUE_RULE` | |
| `FOLDER` | |
| `FORMAT_PROFILE` | |
| `FULFILLMENT_REQUEST` | |
| `GENERAL_TOKEN` | |
| `GENERIC_RESOURCE` | |
| `GIFT_CERTIFICATE` | |
| `GIFT_CERTIFICATE_ITEM` | |
| `GLOBAL_ACCOUNT_MAPPING` | |
| `GLOBAL_INVENTORY_RELATIONSHIP` | |
| `GL_NUMBERING_SEQUENCE` | |
| `GOAL` | |
| `IMPORTED_EMPLOYEE_EXPENSE` | |
| `INBOUND_SHIPMENT` | |
| `INTERCOMP_ALLOCATION_SCHEDULE` | |
| `INTER_COMPANY_JOURNAL_ENTRY` | |
| `INTER_COMPANY_TRANSFER_ORDER` | |
| `INVENTORY_ADJUSTMENT` | |
| `INVENTORY_COST_REVALUATION` | |
| `INVENTORY_COUNT` | |
| `INVENTORY_DETAIL` | |
| `INVENTORY_ITEM` | |
| `INVENTORY_NUMBER` | |
| `INVENTORY_STATUS` | |
| `INVENTORY_STATUS_CHANGE` | |
| `INVENTORY_TRANSFER` | |
| `INVOICE` | |
| `INVOICE_GROUP` | |
| `ISSUE` | |
| `ISSUE_PRODUCT` | |
| `ISSUE_PRODUCT_VERSION` | |
| `ITEM_ACCOUNT_MAPPING` | |
| `ITEM_COLLECTION` | |
| `ITEM_COLLECTION_ITEM_MAP` | |
| `ITEM_DEMAND_PLAN` | |
| `ITEM_FULFILLMENT` | |
| `ITEM_GROUP` | |
| `ITEM_LOCATION_CONFIGURATION` | |
| `ITEM_PROCESS_FAMILY` | |
| `ITEM_PROCESS_GROUP` | |
| `ITEM_RECEIPT` | |
| `ITEM_REVISION` | |
| `ITEM_SUPPLY_PLAN` | |
| `JOB` | |
| `JOB_STATUS` | |
| `JOB_TYPE` | |
| `JOURNAL_ENTRY` | |
| `KIT_ITEM` | |
| `LEAD` | |
| `LOCATION` | |
| `LOT_NUMBERED_ASSEMBLY_ITEM` | |
| `LOT_NUMBERED_INVENTORY_ITEM` | |
| `MANUFACTURING_COST_TEMPLATE` | |
| `MANUFACTURING_OPERATION_TASK` | |
| `MANUFACTURING_ROUTING` | |
| `MAP_REDUCE_SCRIPT` | |
| `MARKUP_ITEM` | |
| `MASSUPDATE_SCRIPT` | |
| `MEM_DOC` | |
| `MERCHANDISE_HIERARCHY_LEVEL` | |
| `MERCHANDISE_HIERARCHY_NODE` | |
| `MERCHANDISE_HIERARCHY_VERSION` | |
| `MESSAGE` | |
| `MFG_PLANNED_TIME` | |
| `NEXUS` | |
| `NON_INVENTORY_ITEM` | |
| `NOTE` | |
| `NOTE_TYPE` | |
| `OPPORTUNITY` | |
| `ORDER_RESERVATION` | |
| `ORDER_SCHEDULE` | |
| `ORDER_TYPE` | |
| `OTHER_CHARGE_ITEM` | |
| `OTHER_NAME` | |
| `OTHER_NAME_CATEGORY` | |
| `PARTNER` | |
| `PARTNER_CATEGORY` | |
| `PAYCHECK` | |
| `PAYCHECK_JOURNAL` | |
| `PAYMENT_CARD` | |
| `PAYMENT_CARD_TOKEN` | |
| `PAYMENT_ITEM` | |
| `PAYMENT_METHOD` | |
| `PAYROLL_ITEM` | |
| `PCT_COMPLETE_PROJECT_REVENUE_RULE` | |
| `PERFORMANCE_METRIC` | |
| `PERFORMANCE_REVIEW` | |
| `PERFORMANCE_REVIEW_SCHEDULE` | |
| `PERIOD_END_JOURNAL` | |
| `PHONE_CALL` | |
| `PICK_STRATEGY` | |
| `PICK_TASK` | |
| `PLANNED_ORDER` | |
| `PLANNING_ITEM_CATEGORY` | |
| `PLANNING_ITEM_GROUP` | |
| `PLANNING_RULE_GROUP` | |
| `PLANNING_VIEW` | |
| `PORTLET` | |
| `PRICE_BOOK` | |
| `PRICE_LEVEL` | |
| `PRICE_PLAN` | |
| `PRICING_GROUP` | |
| `PROJECT_EXPENSE_TYPE` | |
| `PROJECT_IC_CHARGE_REQUEST` | |
| `PROJECT_TASK` | |
| `PROJECT_TEMPLATE` | |
| `PROMOTION_CODE` | |
| `PROSPECT` | |
| `PURCHASE_CONTRACT` | |
| `PURCHASE_ORDER` | |
| `PURCHASE_REQUISITION` | |
| `REALLOCATE_ITEM` | |
| `RECEIVE_INBOUND_SHIPMENT` | |
| `RESOURCE_ALLOCATION` | |
| `RESTLET` | |
| `RETURN_AUTHORIZATION` | |
| `REVENUE_ARRANGEMENT` | |
| `REVENUE_COMMITMENT` | |
| `REVENUE_COMMITMENT_REVERSAL` | |
| `REVENUE_PLAN` | |
| `REV_REC_SCHEDULE` | |
| `REV_REC_TEMPLATE` | |
| `SALES_CHANNEL` | |
| `SALES_ORDER` | |
| `SALES_ROLE` | |
| `SALES_TAX_ITEM` | |
| `SCHEDULED_SCRIPT` | |
| `SCHEDULED_SCRIPT_INSTANCE` | |
| `SCRIPT_DEPLOYMENT` | |
| `SERIALIZED_ASSEMBLY_ITEM` | |
| `SERIALIZED_INVENTORY_ITEM` | |
| `SERVICE_ITEM` | |
| `SHIP_ITEM` | |
| `SOLUTION` | |
| `STATISTICAL_JOURNAL_ENTRY` | |
| `STORE_PICKUP_FULFILLMENT` | |
| `SUBSCRIPTION` | |
| `SUBSCRIPTION_CHANGE_ORDER` | |
| `SUBSCRIPTION_LINE` | |
| `SUBSCRIPTION_PLAN` | |
| `SUBSCRIPTION_TERM` | |
| `SUBSIDIARY` | |
| `SUBSIDIARY_SETTINGS` | |
| `SUBTOTAL_ITEM` | |
| `SUITELET` | |
| `SUPPLY_CHAIN_SNAPSHOT` | |
| `SUPPLY_CHAIN_SNAPSHOT_SIMULATION` | |
| `SUPPLY_CHANGE_ORDER` | |
| `SUPPLY_PLAN_DEFINITION` | |
| `SUPPORT_CASE` | |
| `TASK` | |
| `TAX_ACCT` | |
| `TAX_GROUP` | |
| `TAX_PERIOD` | |
| `TAX_TYPE` | |
| `TERM` | |
| `TIME_BILL` | |
| `TIME_ENTRY` | |
| `TIME_OFF_CHANGE` | |
| `TIME_OFF_PLAN` | |
| `TIME_OFF_REQUEST` | |
| `TIME_OFF_RULE` | |
| `TIME_OFF_TYPE` | |
| `TIME_SHEET` | |
| `TOPIC` | |
| `TRANSFER_ORDER` | |
| `UNITS_TYPE` | |
| `UNLOCKED_TIME_PERIOD` | |
| `USAGE` | |
| `USEREVENT_SCRIPT` | |
| `VENDOR` | |
| `VENDOR_BILL` | |
| `VENDOR_CATEGORY` | |
| `VENDOR_CREDIT` | |
| `VENDOR_PAYMENT` | |
| `VENDOR_PREPAYMENT` | |
| `VENDOR_PREPAYMENT_APPLICATION` | |
| `VENDOR_RETURN_AUTHORIZATION` | |
| `VENDOR_SUBSIDIARY_RELATIONSHIP` | |
| `WAVE` | |
| `WBS` | |
| `WEBSITE` | |
| `WORKFLOW_ACTION_SCRIPT` | |
| `WORK_ORDER` | |
| `WORK_ORDER_CLOSE` | |
| `WORK_ORDER_COMPLETION` | |
| `WORK_ORDER_ISSUE` | |
| `WORKPLACE` | |
| `ZONE` | |

## Examples

### Standard mode: rewrite every line by index

A scheduled script that lines up every item line's expected receipt date with a purchase order's
new header due date after a vendor reports a delay. In standard mode, lines are written directly by
index and nothing is sourced until `save`. Saving with `ignoreMandatoryFields` fits here because no
user is involved (a scheduled script, not a user event). The order id is illustrative.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record'], (record) => {
  const DELAYED_PO_ID = 4821; // illustrative internal id

  const execute = () => {
    const po = record.load({ type: record.Type.PURCHASE_ORDER, id: DELAYED_PO_ID });
    const newDate = po.getValue({ fieldId: 'duedate' });
    if (!(newDate instanceof Date)) {
      log.audit({ title: 'Skipped', details: `PO ${DELAYED_PO_ID} has no due date` });
      return;
    }

    const lineCount = po.getLineCount({ sublistId: 'item' });
    for (let line = 0; line < lineCount; line += 1) {
      po.setSublistValue({
        sublistId: 'item',
        fieldId: 'expectedreceiptdate',
        line,
        value: newDate
      });
    }

    po.setValue({ fieldId: 'memo', value: `Receipt dates aligned to ${newDate.toDateString()}` });
    const savedId = po.save({ ignoreMandatoryFields: true });
    log.audit({ title: 'PO rescheduled', details: `id ${savedId}, ${lineCount} lines updated` });
  };

  return { execute };
});
```

### Dynamic mode: select, set, commit

A RESTlet that books a cycle-count correction as an inventory adjustment. Dynamic mode sources
values as they are set, so the header is filled in the order a user would (subsidiary first), and
each line is built with `selectNewLine` / `setCurrentSublistValue` / `commitLine`. Zero-quantity
rows are dropped with `cancelLine`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/record'], (record) => {
  // Expected body: { subsidiary, account, location, counts: [{ item, delta }] }
  const post = (body) => {
    const adj = record.create({ type: record.Type.INVENTORY_ADJUSTMENT, isDynamic: true });

    adj.setValue({ fieldId: 'subsidiary', value: body.subsidiary });
    adj.setValue({ fieldId: 'account', value: body.account });
    adj.setValue({ fieldId: 'adjlocation', value: body.location });

    body.counts.forEach(({ item, delta }) => {
      adj.selectNewLine({ sublistId: 'inventory' });
      if (!delta) {
        adj.cancelLine({ sublistId: 'inventory' });
        return;
      }
      adj.setCurrentSublistValue({ sublistId: 'inventory', fieldId: 'item', value: item });
      adj.setCurrentSublistValue({ sublistId: 'inventory', fieldId: 'location', value: body.location });
      adj.setCurrentSublistValue({ sublistId: 'inventory', fieldId: 'adjustqtyby', value: delta });
      adj.commitLine({ sublistId: 'inventory' });
    });

    const lines = adj.getLineCount({ sublistId: 'inventory' });
    const adjustmentId = adj.save();
    return { adjustmentId, lines };
  };

  return { post };
});
```

### Transform an approved order into a fulfillment

When a sales order is approved, a user event script creates its item fulfillment with
`record.transform` and marks every line for fulfillment. The transformed record is unsaved until
`save` is called.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/record'], (record) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.APPROVE) {
      return;
    }

    const fulfillment = record.transform({
      fromType: record.Type.SALES_ORDER,
      fromId: context.newRecord.id,
      toType: record.Type.ITEM_FULFILLMENT
    });

    const lineCount = fulfillment.getLineCount({ sublistId: 'item' });
    for (let line = 0; line < lineCount; line += 1) {
      fulfillment.setSublistValue({ sublistId: 'item', fieldId: 'itemreceive', line, value: true });
    }
    fulfillment.setValue({ fieldId: 'memo', value: 'Auto-created on approval' });

    const fulfillmentId = fulfillment.save();
    log.debug({ title: 'Fulfillment created', details: `${fulfillmentId} from order ${context.newRecord.id}` });
  };

  return { afterSubmit };
});
```

### Update two body fields without loading

A RESTlet used by an escalation tool: `record.submitFields` changes a support case's priority and
status in one call. This avoids loading and saving the case, and it is cheaper: for any record class,
`submitFields` costs the same as `load` alone, and `save` is never charged.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/record'], (record) => {
  // Expected body: { caseId, priorityId, statusId }
  const put = (body) => {
    const updatedId = record.submitFields({
      type: record.Type.SUPPORT_CASE,
      id: body.caseId,
      values: {
        priority: body.priorityId,
        status: body.statusId
      },
      options: { enableSourcing: false, ignoreMandatoryFields: true }
    });
    return { updatedId };
  };

  return { put };
});
```

### Promise variant in a client script

On a purchase order form, when the vendor changes, the client script loads the vendor record
asynchronously with `record.load.promise` so the form does not freeze, then logs the vendor's credit
limit. The Promise variants exist only in client scripts.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/record'], (record) => {
  const fieldChanged = (context) => {
    if (context.fieldId !== 'entity') {
      return;
    }
    const vendorId = context.currentRecord.getValue({ fieldId: 'entity' });
    if (!vendorId) {
      return;
    }

    record.load.promise({ type: record.Type.VENDOR, id: vendorId })
      .then((vendor) => {
        const limit = vendor.getValue({ fieldId: 'creditlimit' });
        log.debug({ title: 'Vendor credit limit', details: `${vendorId}: ${limit || 'none set'}` });
      })
      .catch((err) => {
        log.error({ title: 'Vendor lookup failed', details: err.message });
      });
  };

  return { fieldChanged };
});
```
