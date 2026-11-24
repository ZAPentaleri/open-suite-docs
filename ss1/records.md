# Records and subrecords (SuiteScript 1.0)

> This page documents the SuiteScript 1.0 record and subrecord functions so that tools and
> developers can **read, convert and minimally repair** legacy scripts. It is not a guide for new
> development: new code belongs in SuiteScript 2.1 (see the conversion guide below).

1.0 functions take **positional** arguments, so the order in each parameter table is the order a
call must use. Line numbers are 1-based throughout 1.0. A `**Since:**` line appears wherever the
source states the release that introduced a function; where it is missing, the source does not say.

Concepts behind these functions:

- [SuiteScript 1.0 API overview](../topics/ss1_api.md)
- [Records in 1.0](../topics/ss1_api.records.md) (record modes, standard vs dynamic)
- [Subrecords in 1.0](../topics/ss1_api.subrecords.md)
- [Initialization defaults](../topics/ss1_api.initialization_defaults.md) (the `initializeValues`
  argument) and the [record initialization defaults catalog](../gen/catalogs/record_initialization_defaults.md)
- [CSV import](../topics/csv_import.md) (background for `nlapiCreateCSVImport` and
  `nlapiSubmitCSVImport`) and the [CSV import sublists catalog](../gen/catalogs/csv_import_sublists.md)
- [Converting 1.0 to 2.1](../conversion/ss1-to-ss21.md)

Record type and field IDs for the record types themselves are in the
[generated record reference](../gen/records/index.md).

## Record functions

### nlapiAttachRecord(type, id, type2, id2, attributes)
<!-- id: ss1:nlapiAttachRecord -->

Links two records together: for example a contact to a company, a File Cabinet file to a
transaction, or an entity to a static group.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `type2` | string | Yes | |
| `id2` | number | Yes | |
| `attributes` | Object | No | |

- `type`: the kind of record being attached. Pass `'file'` when the thing being attached is a File
  Cabinet file.
- `id`: internal ID of the record being attached.
- `type2`: the kind of record it is attached to. Pass `'entitygroup'` to put an entity into a static
  entity group.
- `id2`: internal ID of the record it is attached to.
- `attributes`: optional extra settings. Two keys are known: `role`, used when attaching a contact
  to a company, and `field`, used when attaching a custom child record to its parent.

**Returns:** `void`

**Governance:** 10 units.

**Availability:** client, user event, scheduled and Suitelet scripts only.

**Since:** 2008.1

Relationships the source lists as supported:

- a support case to an issue;
- a contact to a customer, partner, vendor or lead, and a contact to a prospect or project;
- a file to a transaction, item or activity, and a file to a custom record or an entity record;
- a custom child record to its parent record;
- an entity to a static entity group.

2.x equivalent: `N/record.attach` in [N/record](../modules/N_record.md); the five positional
arguments become members of an options object.

### nlapiCopyRecord(type, id, initializeValues)
<!-- id: ss1:nlapiCopyRecord -->

Produces a new, unsaved record that starts as a copy of an existing one. Nothing reaches the
database until the copy is handed to
[`nlapiSubmitRecord`](#nlapisubmitrecordrecord-dosourcing-ignoremandatoryfields).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `initializeValues` | Object | No | |

- `initializeValues`: name/value pairs taken from the record initialization defaults (see
  [Initialization defaults](../topics/ss1_api.initialization_defaults.md)).

**Returns:** `nlobjRecord`, the unsaved copy.

**Governance:** by record class: transaction 10 units, standard 5 units, custom 2 units.

**Availability:** all script types.

2.x equivalent: `N/record.copy` in [N/record](../modules/N_record.md) (options object).

### nlapiCreateCSVImport()
<!-- id: ss1:nlapiCreateCSVImport -->

Returns an empty `nlobjCSVImport` object in which a script describes an import job. Configuring it
does nothing by itself; the object is then passed to
[`nlapiSubmitCSVImport`](#nlapisubmitcsvimportnlobjcsvimport), which queues the job.

**Returns:** `nlobjCSVImport`

**Governance:** 0 units.

**Availability:** bundle installation, scheduled and RESTlet scripts only; calling it from any other
script type raises an error.

**Since:** 2012.2

- It cannot set up imports for the record types that need the two-step import assistant: budgets,
  single journal entries, single inventory worksheets, project tasks and website redirects.

**Sublists in a scripted CSV import.** Whether a sublist can travel in the import, and on what
terms, is decided per sublist. The [CSV import sublists catalog](../gen/catalogs/csv_import_sublists.md)
lists all 173 sublists with their facts. The kinds of fact recorded there, with an example of each
(names quoted exactly as the source gives them):

- **Required features.** A sublist can depend on a feature being enabled; for example the Items
  "Locations" sublist needs `"Multi-Location Inventory"`. Some sublists list alternatives, and any
  one of them is enough: the Items "Bin Numbers" sublist accepts either `"Bin Management"` or
  `"Advanced Bin/Numbered Inventory Management"`. A few sublists need a particular feature to be
  switched **off**.
- **Required preferences and other conditions.** The Purchase Order "Expenses" sublist needs the
  preference `"Allow Expenses on Purchase Orders"`; the Expense Report "Accounting Book Detail"
  sublist needs `"Multi-Book Accounting"` and more than one active accounting book.
- **Key fields for selective updates.** When an import updates existing lines, these fields
  identify which line to change; for Items "Locations" they are `"Internal ID"`, `"External ID"`
  and `"Location"`.
- **Add-only or update-only sublists.** The Customers "Credit Cards" sublist can be filled when
  adding new records but its existing lines cannot be updated; the Customers "Contact Access"
  sublist works only when updating existing records.
- **Separate files.** Some sublists must come from their own CSV file rather than sharing one with
  the main record data (the Purchase Order "Expenses" sublist is one).
- **Which import types apply.** Some sublists exist only in certain item or record imports; Items
  "Bin Numbers" appears only in the `"Assembly/BOM Item"` and `"Inventory Item"` imports, and
  bins must be imported before items that refer to bin numbers.
- **Minimum lines.** Some transactions need at least one line when a new record is created; the
  Advanced Intercompany Journal Entry line sublist needs at least four lines, with debits and
  credits for both the originating and the receiving subsidiary, and they must balance.

2.x equivalent: `N/task.create` in [N/task](../modules/N_task.md). The mapping calls this a rename:
the import becomes a task.

### nlapiCreateEmailMerger(templateId)
<!-- id: ss1:nlapiCreateEmailMerger -->

Starts an email merge based on a scriptable email template. It is the replacement for the older
[`nlapiMergeRecord`](#nlapimergerecordid-basetype-baseid-alttype-altid-fields) and
[`nlapiMergeTemplate`](#nlapimergetemplateid-basetype-baseid-alttype-altid-fields).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `templateId` | number | Yes | |

**Returns:** `nlobjEmailMerger`

**Errors:**

- `SSS_MERGER_ERROR_OCCURRED`: the template is of the wrong kind, i.e. a system template or a
  CRMSDK template was passed instead of a scriptable one.

**Governance:** 0 units.

**Since:** 2015.1

- Only scriptable email templates are accepted.
- Most checking of the inputs is postponed until the merge itself runs (`nlobjEmailMerger.merge`), so
  a bad setup usually surfaces there, not at this call.

2.x equivalent: `N/render.mergeEmail` in [N/render](../modules/N_render.md) (options object).

### nlapiCreateRecord(type, initializeValues)
<!-- id: ss1:nlapiCreateRecord -->

Creates an in-memory record of the given type, already filled with that type's default field
values. It stays in memory until saved with
[`nlapiSubmitRecord`](#nlapisubmitrecordrecord-dosourcing-ignoremandatoryfields).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `initializeValues` | Object | No | |

- `initializeValues`: name/value pairs from the record initialization defaults (see
  [Initialization defaults](../topics/ss1_api.initialization_defaults.md)).

**Returns:** `nlobjRecord`, a new record carrying default values.

**Errors:**

- `SSS_INVALID_RECORD_TYPE`: `type` holds an invalid record type.
- `SSS_TYPE_ARG_REQD`: `type` was not supplied.

**Governance:** by record class: transaction 10 units, standard 5 units, custom 2 units.

**Availability:** all script types.

- Every required field needs a value before the record is submitted. A custom form can make further
  fields required, so the set to fill can be larger than the record type alone suggests.
- Note records cannot be created on their own with this function.
- Message records need both an author and a recipient.

2.x equivalent: `N/record.create` in [N/record](../modules/N_record.md).

### nlapiDeleteRecord(type, id, initializeValues)
<!-- id: ss1:nlapiDeleteRecord -->

Deletes a record. The deletion is permanent.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `initializeValues` | Object | No | |

- `initializeValues`: name/value defaults used when the record is initialized. The source does not
  explain what effect they have on a delete.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_RECORD_TYPE`: `type` holds an invalid record type.
- `SSS_TYPE_ARG_REQD`: `type` was not supplied.
- `SSS_INVALID_INTERNAL_ID`: `id` holds an invalid internal ID.
- `SSS_ID_ARG_REQD`: `id` was not supplied.

**Governance:** by record class: transaction 20 units, standard 10 units, custom 4 units.

**Availability:** all script types.

2.x equivalent: `N/record.delete` in [N/record](../modules/N_record.md). The mapping carries only
`type` and `id` across; the third argument has no counterpart there.

### nlapiDetachRecord(type, id, type2, id2, attributes)
<!-- id: ss1:nlapiDetachRecord -->

The reverse of [`nlapiAttachRecord`](#nlapiattachrecordtype-id-type2-id2-attributes): removes the
link between two records.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `type2` | string | Yes | |
| `id2` | number | Yes | |
| `attributes` | Object | No | |

- `type2`: pass `'entitygroup'` when taking an entity out of a static entity group.

**Returns:** `void`

**Governance:** 10 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts only.

**Since:** 2008.1

Relationships the source lists as supported: an issue from a support case; a contact from a
customer, partner or vendor; a contact from a lead, prospect or project; a file from a record; a
custom child record from its parent; an entity from a static entity group.

2.x equivalent: `N/record.detach` in [N/record](../modules/N_record.md).

### nlapiGetNewRecord()
<!-- id: ss1:nlapiGetNewRecord -->

Inside a user event, gives the record as it is being written (the "new" side of the event).

**Returns:** `nlobjRecord`. When the user event `type` is `xedit` (an inline edit), the object holds
only the fields and lines that were actually edited, not the whole record.

**Governance:** 0 units.

**Availability:** user event scripts only, in the before load, before submit and after submit
functions.

- The returned record cannot be passed to `nlapiSubmitRecord`; it is a view of the record in
  flight, not something to save.

2.x equivalent: no direct function. The mapping marks it as approximate and points at the user
event entry points (before load, before submit, after submit) in
[UserEventScript](../script-types/UserEventScript.md), where the record arrives on the context.

### nlapiGetOldRecord()
<!-- id: ss1:nlapiGetOldRecord -->

Inside a user event, gives the record as it was before this write.

**Returns:** `nlobjRecord` holding the values from before the current write.

**Governance:** 0 units.

**Availability:** user event scripts only, in the before load, before submit and after submit
functions.

- As with the new record, this object cannot be submitted.

2.x equivalent: approximate; the mapping points at the before submit and after submit entry points
of [UserEventScript](../script-types/UserEventScript.md).

### nlapiGetRecordId()
<!-- id: ss1:nlapiGetRecordId -->

Returns the internal ID of the record the script is running against.

**Returns:** `number`; `-1` when there is no current record, or when the record is new and not yet
saved.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

2.x equivalent: a pattern change to the `id` property of `N/record.Record` or
`N/currentRecord.CurrentRecord` (see [N/record](../modules/N_record.md),
[N/currentRecord](../modules/N_currentRecord.md)).

### nlapiGetRecordType()
<!-- id: ss1:nlapiGetRecordType -->

Returns the type ID of the current record, for example `salesorder`, `customer` or
`promotioncode`.

**Returns:** `string`; `null` when there is no current record.

**Governance:** 0 units.

**Availability:** client and user event scripts only.

2.x equivalent: a pattern change to the `type` property of `N/record.Record` or
`N/currentRecord.CurrentRecord`.

### nlapiLoadRecord(type, id, initializeValues)
<!-- id: ss1:nlapiLoadRecord -->

Reads an existing record from the database into an `nlobjRecord` that can be inspected, changed and
then saved with [`nlapiSubmitRecord`](#nlapisubmitrecordrecord-dosourcing-ignoremandatoryfields).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `initializeValues` | Object | No | |

- `type`: not case-sensitive. Some IDs cover several record kinds: `inventoryitem` also loads lot
  and serialized inventory items, `assemblyitem` also loads lot and serialized assembly items, and
  `customer` covers customers, leads and prospects. Script and deployment records can be loaded
  too, with IDs such as `usereventscript`, `suitelet`, `scheduledscript`, `clientscript`,
  `restlet`, `massupdatescript`, `bundleinstallationscript`, `workflowactionscript`, `portlet` and
  `scriptdeployment`.
- `initializeValues`: name/value pairs from the record initialization defaults. The
  [Records topic](../topics/ss1_api.records.md) explains how an initialization value selects
  dynamic mode.

**Returns:** `nlobjRecord`

**Errors:**

- `SSS_INVALID_RECORD_TYPE`: `type` holds an invalid record type.
- `SSS_TYPE_ARG_REQD`: `type` was not supplied.
- `SSS_INVALID_INTERNAL_ID`: `id` holds an invalid internal ID.
- `SSS_ID_ARG_REQD`: `id` was not supplied.

**Governance:** by record class: transaction 10 units, standard 5 units, custom 2 units.

**Availability:** all script types, client and server.

- The record type must be one that SuiteScript supports.
- A record locked by a Lock Record workflow action cannot be loaded.
- Field changes made by a before load user event are not present in the loaded record; before load
  changes shape what a user sees on the form, not what a script loads.

2.x equivalent: `N/record.load` in [N/record](../modules/N_record.md).

### nlapiMergeRecord(id, baseType, baseId, altType, altId, fields)
<!-- id: ss1:nlapiMergeRecord -->

Legacy mail-merge function that works only with CRMSDK templates. Treat any call you find as a
candidate for replacement.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | any | No | |
| `baseType` | any | No | |
| `baseId` | any | No | |
| `altType` | any | No | |
| `altId` | any | No | |
| `fields` | any | No | |

- The source gives none of the six parameters a type or a required/optional flag. The `any` and
  "No" in the table are placeholders for "not stated", not facts; don't infer types from them.

**Returns:** the source does not state what this function returns.

**Governance:** 10 units.

**Deprecated:** since 2015.1, and no longer supported from 2016.1. Replaced by
[`nlapiCreateEmailMerger`](#nlapicreateemailmergertemplateid) together with the `nlobjEmailMerger`
and `nlobjMergeResult` objects.

- Supports CRMSDK templates only.

2.x equivalent: the source gives no 2.x mapping for this function. Convert it by first moving to the
email merger approach, whose 2.x counterpart is `N/render.mergeEmail`.

### nlapiMergeTemplate(id, baseType, baseId, altType, altId, fields)
<!-- id: ss1:nlapiMergeTemplate -->

An even older merge function, also limited to CRMSDK templates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | any | No | |
| `baseType` | any | No | |
| `baseId` | any | No | |
| `altType` | any | No | |
| `altId` | any | No | |
| `fields` | any | No | |

- As with `nlapiMergeRecord`, the source gives no types or optionality for these parameters; `any`
  and "No" are placeholders, not facts.

**Returns:** the source does not state what this function returns.

**Governance:** 0 units.

**Deprecated:** since 2008.1. It is still supported but gets no further enhancements. Replaced by
[`nlapiCreateEmailMerger`](#nlapicreateemailmergertemplateid).

- Supports CRMSDK templates only.

2.x equivalent: the source gives no 2.x mapping for this function; move to the email merger approach
(`N/render.mergeEmail` in 2.x).

### nlapiPrintRecord(type, id, mode, properties)
<!-- id: ss1:nlapiPrintRecord -->

Renders a printable version of a record (a transaction form, a statement, a packing slip, a picking
ticket or a bill of materials) and returns it as a file object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `mode` | string | No | |
| `properties` | Object | No | |

- `type`: one of `TRANSACTION`, `STATEMENT`, `PACKINGSLIP`, `PICKINGTICKET`, `BILLOFMATERIAL`.
- `id`: internal ID of the record to print.
- `mode`: `PDF`, `HTML` or `DEFAULT`; `DEFAULT` follows the user's or the company's print
  preference.
- `properties`: extra settings that depend on `type`:
  - `TRANSACTION`: `formnumber`;
  - `STATEMENT`: `openonly`, `startdate`, `statementdate`, `formnumber`;
  - `PACKINGSLIP`: `formnumber`, `itemfulfillment`;
  - `PICKINGTICKET`: `formnumber`, `shipgroup`, `location`.

**Returns:** `nlobjFile`

**Governance:** 0 units.

**Availability:** user event, scheduled and Suitelet scripts only; not supported in client scripts.

**Since:** 2008.1

- The generated file can be at most 10 MB.
- It only produces the file. It does not send anything to a printer and does not open a viewer.
- When advanced PDF/HTML templates are enabled in the account, the output uses the advanced PDF
  template attached to the record's custom form.

2.x equivalent: split by output type into `N/render.transaction`, `N/render.statement`,
`N/render.packingSlip`, `N/render.pickingTicket` and `N/render.bom` (see
[N/render](../modules/N_render.md)).

### nlapiSubmitCSVImport(nlobjCSVImport)
<!-- id: ss1:nlapiSubmitCSVImport -->

Queues the import described by an `nlobjCSVImport` object (built with
[`nlapiCreateCSVImport`](#nlapicreatecsvimport)). The import then runs asynchronously as a job.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `nlobjCSVImport` | nlobjCSVImport | Yes | |

**Returns:** the ID of the queued CSV import job. The source does not state the type of that value.

**Errors:**

- If the validation that runs before the import starts fails, the usual inline CSV import
  validation errors are thrown right away from this call. The source names no specific codes.
- Errors that happen while records are being imported are not thrown; they are written to the CSV
  response file of the job.

**Governance:** 100 units.

**Availability:** bundle installation, scheduled and RESTlet scripts only; calling it elsewhere
raises an error.

**Since:** 2012.2

- One import can carry at most 25,000 records.
- Record types that need the two-step import assistant cannot be imported this way.
- Script-submitted imports run on a single thread and a single queue, whatever the saved import map
  says about threads or queues.
- Sublist data in the import is subject to the per-sublist conditions summarised under
  [`nlapiCreateCSVImport`](#nlapicreatecsvimport): required features (by their exact names) and
  preferences, selective-update key fields, add-only or update-only sublists, separate-file
  requirements, the import types each sublist belongs to, and minimum line counts. The full list is
  the [CSV import sublists catalog](../gen/catalogs/csv_import_sublists.md); the general rules are
  in the [CSV import topic](../topics/csv_import.md).

2.x equivalent: approximate. The mapping names only the [N/task](../modules/N_task.md) module, not a
specific member (the CSV import becomes a task there; its sibling `nlapiCreateCSVImport` maps to
`N/task.create`).

### nlapiSubmitRecord(record, doSourcing, ignoreMandatoryFields)
<!-- id: ss1:nlapiSubmitRecord -->

Saves an `nlobjRecord` (new, copied, transformed or loaded) to the database and returns its ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `record` | nlobjRecord | Yes | |
| `doSourcing` | boolean | No | `false` |
| `ignoreMandatoryFields` | boolean | No | |

- `doSourcing`: a real boolean, not `'T'`/`'F'`. When `true`, empty fields that depend on other
  fields are sourced before saving. It is ignored for records in dynamic mode, which are always
  sourced.
- `ignoreMandatoryFields`: when `true`, fields made mandatory by customization or by preferences
  are not enforced. The source recommends keeping this mainly to scheduled scripts.

**Returns:** `number`, the internal ID of the saved record.

**Errors:**

- `SSS_INVALID_RECORD_OBJ`: `record` is not a valid record object.
- `SSS_RECORD_OBJ_REQD`: `record` was not supplied.
- `SSS_INVALID_SOURCE_ARG`: `doSourcing` has an invalid value.

**Governance:** by record class: transaction 20 units, standard 10 units, custom 4 units.

**Availability:** all script types.

- Risk in user events: if a user event saves a *different* record and the save of the record that
  triggered the event then fails, the other record stays saved. Do such work in after submit,
  where the triggering record is already committed.

2.x equivalent: `N/record.Record#save` in [N/record](../modules/N_record.md).

### nlapiTransformRecord(type, id, transformType, transformValues)
<!-- id: ss1:nlapiTransformRecord -->

Creates a new, unsaved record from an existing one, the way the UI turns a sales order into an
invoice or a purchase order into an item receipt. The result still needs
[`nlapiSubmitRecord`](#nlapisubmitrecordrecord-dosourcing-ignoremandatoryfields).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `id` | number | Yes | |
| `transformType` | string | Yes | |
| `transformValues` | Object | No | |

- `type` / `id`: the source record.
- `transformType`: the record type to produce. Supported source-to-target pairs are listed in the
  [record transformations catalog](../gen/catalogs/record_transformations.md).
- `transformValues`: can be used to open the result in dynamic mode. It also carries `shipgroup`,
  which defaults to `1`; when a sales order uses multiple shipping routes and is transformed into an
  item fulfillment, set `shipgroup` to choose which shipping group to fulfil.

**Returns:** `nlobjRecord`, the unsaved target record.

**Errors** (as listed in the source, but see the note below):

- `SSS_INVALID_URL_CATEGORY`: an invalid URL.
- `SSS_CATEGORY_ARG_REQD`: a missing category argument.
- `SSS_INVALID_TASK_ID`: an invalid task ID value.
- `SSS_TASK_ID_REQD`: a missing task ID argument.
- `SSS_INVALID_INTERNAL_ID`: an invalid internal ID value.
- `SSS_INVALID_EDITMODE_ARG`: an invalid edit-mode argument value.

> [!SOURCE-ERROR] This error list appears to have been copied from the URL-resolving function
> (codes such as `SSS_INVALID_URL_CATEGORY` have nothing to do with transforms). The source does
> not give the real error set, so don't build error handling for a transform on these codes.
<!-- source-error: ss1:nlapiTransformRecord | - | throws_list_copied_from_url_api -->

**Governance:** by record class: transaction 10 units, standard 5 units, custom 2 units.

**Availability:** client, user event, scheduled and Suitelet scripts only.

> [!SOURCE-ERROR] The source's table of supported transformations lists one row twice (the
> intercompany transfer order to item fulfillment pair). The duplicate is harmless, but treat the
> table as not carefully checked.
<!-- source-error: ss1:nlapiTransformRecord | - | transformation_table_repeats_a_row -->

> [!SOURCE-ERROR] In the same table the target for a cash refund is labelled as a cash sale, which
> looks wrong. The source does not say what the correct target is; verify against the
> [transformations catalog](../gen/catalogs/record_transformations.md) and a test account.
<!-- source-error: ss1:nlapiTransformRecord | - | cashrefund_target_named_cash_sale -->

2.x equivalent: `N/record.transform` in [N/record](../modules/N_record.md).

### nlapiVoidTransaction(transactionType, recordId)
<!-- id: ss1:nlapiVoidTransaction -->

Voids a transaction. A void does not delete anything: the transaction's total and its lines are set
to zero. How the void is carried out depends on the account preference for using reversing
journals.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `transactionType` | string | Yes | |
| `recordId` | number | Yes | |

**Returns:** an ID that tells you which kind of void happened; the source does not state its type.
With a direct void you get the voided transaction's own ID back; with a reversing-journal void you
get the ID of the newly created reversing journal. Comparing the result with `recordId` therefore
tells you which path was taken.

**Errors:**

- `CANT_VOID_TRANS`: the transaction is in a state that prevents voiding, namely it is linked to
  other transactions.
- `VOIDING_REVERSAL_DISALLWD`: voiding by reversal is not allowed because the transaction has an
  inventory impact.
- `INVALID_RCRD_TYPE`: `transactionType` holds an invalid record type.
- `RCRD_DSNT_EXIST`: no record exists with `recordId`.
- `THIS_TRANSACTION_HAS_ALREADY_BEEN_VOIDED`: the transaction was voided earlier.

**Governance:** 10 units.

**Availability:** client, user event, scheduled, Suitelet, RESTlet and workflow action scripts only.

**Since:** 2013.2

- Reversing-journals preference off: a direct void. Preference on: a reversing journal is created.
- Types that can be voided directly: `cashrefund`, `cashsale`, `creditmemo`, `customerdeposit`,
  `customerpayment`, `customerrefund`, `estimate`, `expensereport`, `intercompanyjournalentry`,
  `invoice`, `journalentry`, `paycheckjournal`, `returnauthorization`, `salesorder`,
  `transferorder`, `vendorbill`, `vendorcredit`, `vendorpayment`, `vendorreturnauthorization`,
  `workorder`.
- Types that can be voided with a reversing journal: `check`, `vendorpayment`, `customerrefund` and
  custom transaction types (`customtransaction_*`).
- After a void, fields that affect the general ledger can no longer be changed on that transaction.

2.x equivalent: `N/transaction.void` in [N/transaction](../modules/N_transaction.md).

## Subrecord functions

These functions act on the current record (or the current line of one of its sublists). See
[Subrecords in 1.0](../topics/ss1_api.subrecords.md). All nine were introduced in 2011.2. The
create and edit functions share one rule: changes to a subrecord only stick if you call
`nlobjSubrecord.commit()` on it and the parent record is then saved (with `nlapiSubmitRecord` where
the script saves it itself); skip either step and the changes are lost.

### nlapiCreateCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlapiCreateCurrentLineItemSubrecord -->

Creates a subrecord in a subrecord field on the currently selected sublist line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Availability:** user event scripts on the parent record only; not in before load, and not in
client scripts on the parent record.

**Since:** 2011.2

- Commit the subrecord, then save the parent, or the new subrecord is lost.

2.x equivalent: `getCurrentSublistSubrecord` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiCreateSubrecord(fldname)
<!-- id: ss1:nlapiCreateSubrecord -->

Creates a subrecord in a body field of the current record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Availability:** user event scripts on the parent record only; not in before load, and not in
client scripts on the parent record.

**Since:** 2011.2

- Commit the subrecord, then save the parent, or the new subrecord is lost.

2.x equivalent: `getSubrecord` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiEditCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlapiEditCurrentLineItemSubrecord -->

Opens the existing subrecord in a subrecord field on the currently selected sublist line for
editing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Availability:** user event scripts on the parent record only; not in client scripts on the parent
record.

**Since:** 2011.2

- Commit the subrecord, then save the parent, or the edits are lost.

2.x equivalent: `getCurrentSublistSubrecord` on `N/record.Record` or
`N/currentRecord.CurrentRecord`.

### nlapiEditSubrecord(fldname)
<!-- id: ss1:nlapiEditSubrecord -->

Opens the existing subrecord in a body field for editing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`

**Governance:** 0 units.

**Availability:** user event scripts on the parent record only; not in client scripts on the parent
record.

**Since:** 2011.2

- Commit the subrecord, then save the parent, or the edits are lost.

2.x equivalent: `getSubrecord` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiRemoveCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlapiRemoveCurrentLineItemSubrecord -->

Removes the subrecord held in a subrecord field on the currently selected sublist line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `void`

> [!SOURCE-ERROR] The source's description says this function returns a subrecord, while its
> declared return is void. Treat it as returning nothing; don't write code that uses a return value.
<!-- source-error: ss1:nlapiRemoveCurrentLineItemSubrecord | - | description_says_returns_subrecord_returns_void -->

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2011.2

2.x equivalent: `removeCurrentSublistSubrecord` on `N/record.Record` or
`N/currentRecord.CurrentRecord`.

### nlapiRemoveSubrecord(fldname)
<!-- id: ss1:nlapiRemoveSubrecord -->

Removes the subrecord held in a body field. The source ties its use to the advanced bin / numbered
inventory feature; it does not give that feature's exact name here.

> [!SOURCE-ERROR] The source writes that feature's name in a cut-off form (it stops after
> "Numbered Inventory"). Look up the full feature name in the account's feature settings rather
> than relying on the text.
<!-- source-error: ss1:nlapiRemoveSubrecord | - | feature_name_truncated_numbered_inventory -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `void`

> [!SOURCE-ERROR] As with the line-level remove function, the description claims a subrecord is
> returned although the declared return is void. Rely on `void`.
<!-- source-error: ss1:nlapiRemoveSubrecord | - | description_says_returns_subrecord_returns_void -->

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2011.2

2.x equivalent: `removeSubrecord` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

### nlapiViewCurrentLineItemSubrecord(sublist, fldname)
<!-- id: ss1:nlapiViewCurrentLineItemSubrecord -->

Gives read-only access to the subrecord on the currently selected sublist line.

> [!SOURCE-ERROR] The source's text names the related advanced bin / numbered inventory feature in a
> truncated form. Check the exact feature name in the account instead of copying it from the source.
<!-- source-error: ss1:nlapiViewCurrentLineItemSubrecord | - | feature_name_truncated_numbered_inventory -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`, read-only.

**Errors:**

- An error is thrown if the script tries to change the returned subrecord. The source does not state
  the error code.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2011.2

- It reads the line that is currently selected, so select the line first.

2.x equivalent: `getCurrentSublistSubrecord` on `N/currentRecord.CurrentRecord` or
`N/record.Record`.

### nlapiViewLineItemSubrecord(sublist, fldname, linenum)
<!-- id: ss1:nlapiViewLineItemSubrecord -->

Gives read-only access to the subrecord on any sublist line, without having to select that line
first.

> [!SOURCE-ERROR] The inventory feature this function relates to (advanced bin / numbered
> inventory) is named in a truncated form in the source. Check the exact name in the account.
<!-- source-error: ss1:nlapiViewLineItemSubrecord | - | feature_name_truncated_numbered_inventory -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `fldname` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: 1-based (the first line is 1).

**Returns:** `nlobjSubrecord`, read-only.

**Errors:**

- An error is thrown if the script tries to change the returned subrecord. The source does not state
  the error code.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2011.2

2.x equivalent: `N/record.Record#getSublistSubrecord` in [N/record](../modules/N_record.md).
Remember that 2.x line indexes start at 0.

### nlapiViewSubrecord(fldname)
<!-- id: ss1:nlapiViewSubrecord -->

Gives read-only access to the subrecord held in a body field.

> [!SOURCE-ERROR] The related advanced bin / numbered inventory feature is named in a cut-off form in
> the source; verify the exact feature name in the account.
<!-- source-error: ss1:nlapiViewSubrecord | - | feature_name_truncated_numbered_inventory -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldname` | string | Yes | |

**Returns:** `nlobjSubrecord`, read-only.

**Errors:**

- An error is thrown if the script tries to change the returned subrecord. The source does not state
  the error code.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2011.2

2.x equivalent: `getSubrecord` on `N/record.Record` or `N/currentRecord.CurrentRecord`.

## Examples

These are 1.0 snippets of the kind you meet in legacy accounts, shown so you can recognise and
repair them. Convert them to 2.1 rather than extending them.

**Telling which kind of void happened.** The return value of `nlapiVoidTransaction` is the
transaction's own ID for a direct void, or a new journal's ID for a reversing-journal void.

```js
function voidStaleVendorBill(billId) {
  var resultId = nlapiVoidTransaction('vendorbill', billId);
  if (String(resultId) === String(billId)) {
    nlapiLogExecution('DEBUG', 'Void', 'Bill ' + billId + ' voided directly');
  } else {
    nlapiLogExecution('DEBUG', 'Void', 'Reversing journal ' + resultId + ' created for bill ' + billId);
  }
  return resultId;
}
```

**Repair: related record saved from before submit.** The legacy version of this user event created
an item receipt in before submit; if the purchase order save then failed, the receipt remained. The
fix moves the work to after submit and guards against the `-1` ID of an unsaved record. (The
transformations catalog notes that this pair is not available for drop-ship purchase orders.)

```js
function afterSubmitReceiveOnCreate(type) {
  if (type != 'create') {
    return;
  }
  var poId = nlapiGetRecordId();
  if (poId == -1) {
    return;
  }
  var receipt = nlapiTransformRecord('purchaseorder', poId, 'itemreceipt');
  receipt.setFieldValue('memo', 'Auto-received at creation');
  var receiptId = nlapiSubmitRecord(receipt, false, true);
  nlapiLogExecution('DEBUG', 'Receipt', 'Created item receipt ' + receiptId);
}
```

**Fulfilling one shipping group.** For a sales order with multiple shipping routes, pass
`shipgroup` in `transformValues`; otherwise group `1` is used.

```js
function fulfilShipGroup(salesOrderId, groupNumber) {
  var fulfilment = nlapiTransformRecord('salesorder', salesOrderId, 'itemfulfillment', {
    shipgroup: groupNumber
  });
  return nlapiSubmitRecord(fulfilment);
}
```

**Repair: subrecord edits that never stuck.** The legacy code edited the inventory detail on an item
fulfillment line but never committed the subrecord, so the change was silently dropped. The commit
must come before the line and the parent are saved.

```js
function beforeSubmitTouchInventoryDetail(type) {
  if (type != 'edit') {
    return;
  }
  nlapiSelectLineItem('item', 1);
  var detail = nlapiEditCurrentLineItemSubrecord('item', 'inventorydetail');
  // ... changes to the detail's inventoryassignment lines go here ...
  detail.commit(); // missing in the original script
  nlapiCommitLineItem('item');
}
```
