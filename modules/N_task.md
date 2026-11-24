# N/task
<!-- id: N/task -->

`N/task` hands work to NetSuite's task queue so it runs in the background, outside the script that
asked for it. Every task type in this module runs asynchronously: you build a task object, fill in
its properties, call `submit()` to queue it, and keep the returned task ID so you can ask about
progress later with [`task.checkStatus(options)`](#taskcheckstatusoptions).

Things you can queue this way: a scheduled script deployment, a map/reduce deployment, a CSV import,
a merge or delete of duplicate entity records, a saved search exported to CSV, an `N/query` query or
a SuiteQL statement exported to CSV, a document capture job, a record action run against many
records, and a workflow started on a record. The task type comes from
[`task.TaskType`](#tasktasktype).

- **Availability:** server scripts only.
- **Permission:** the spec lists the Tasks permission as a requirement for using the module.
- **Typical flow:** `task.create(...)` → set properties → `submit()` → `task.checkStatus(...)`.
- **Since:** the source does not state a version for the module as a whole; each member below
  carries its own version where the source gives one.
- **Load it as:** `define(['N/task'], (task) => { ... })`
- Related: [N/query](N_query.md), [N/search](N_search.md), [N/file](N_file.md),
  [N/documentCapture](N_documentCapture.md), [N/record](N_record.md), [N/action](N_action.md),
  [N/task/accounting/recognition](N_task_accounting_recognition.md),
  [CSV import](../topics/csv_import.md),
  [Map/reduce submission](../topics/map_reduce.submission.md),
  [Map/reduce status page](../topics/map_reduce.status_page.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`task.create(options)`](#taskcreateoptions) | function | Build a task object of a given type |
| [`task.checkStatus(options)`](#taskcheckstatusoptions) | function | Get the status object for a submitted task |
| [`CsvImportTask`](#csvimporttask-object) | object | Queue a CSV import through a saved import map |
| [`CsvImportTaskStatus`](#csvimporttaskstatus-object) | object | Status of a CSV import |
| [`DocumentCaptureTask`](#documentcapturetask-object) | object | Extract text and tables from a file in the background |
| [`DocumentCaptureTaskStatus`](#documentcapturetaskstatus-object) | object | Status of a document capture job |
| [`EntityDeduplicationTask`](#entitydeduplicationtask-object) | object | Merge or delete duplicate entity records |
| [`EntityDeduplicationTaskStatus`](#entitydeduplicationtaskstatus-object) | object | Status of a deduplication job |
| [`MapReduceScriptTask`](#mapreducescripttask-object) | object | Queue a map/reduce deployment |
| [`MapReduceScriptTaskStatus`](#mapreducescripttaskstatus-object) | object | Status and progress counters of a map/reduce run |
| [`QueryTask`](#querytask-object) | object | Run an `N/query` query to a CSV file |
| [`QueryTaskStatus`](#querytaskstatus-object) | object | Status of a query export |
| [`RecordActionTask`](#recordactiontask-object) | object | Run a record action over many records |
| [`RecordActionTaskStatus`](#recordactiontaskstatus-object) | object | Overall and per-record results of a record action run |
| [`ScheduledScriptTask`](#scheduledscripttask-object) | object | Queue a scheduled script deployment |
| [`ScheduledScriptTaskStatus`](#scheduledscripttaskstatus-object) | object | Status of a scheduled script run |
| [`SearchTask`](#searchtask-object) | object | Run a saved search to a CSV file |
| [`SearchTaskStatus`](#searchtaskstatus-object) | object | Status of a search export |
| [`SuiteQLTask`](#suiteqltask-object) | object | Run a SuiteQL string to a CSV file |
| [`SuiteQLTaskStatus`](#suiteqltaskstatus-object) | object | Status of a SuiteQL export |
| [`WorkflowTriggerTask`](#workflowtriggertask-object) | object | Start a workflow on a record in the background |
| [`WorkflowTriggerTaskStatus`](#workflowtriggertaskstatus-object) | object | Status of a workflow trigger |
| [`task.TaskType`](#tasktasktype) | enum | Kinds of task `create` can build |
| [`task.TaskStatus`](#tasktaskstatus) | enum | Status values shared by all status objects |
| [`task.MapReduceStage`](#taskmapreducestage) | enum | Stages of a map/reduce run |
| [`task.ActionCondition`](#taskactioncondition) | enum | Record selection for record action tasks |
| [`task.DedupeMode`](#taskdedupemode) | enum | What a deduplication task does |
| [`task.DedupeEntityType`](#taskdedupeentitytype) | enum | Entity types a deduplication task can process |
| [`task.MasterSelectionMode`](#taskmasterselectionmode) | enum | How the surviving (master) record is chosen |

## Functions

### task.create(options)
<!-- id: N/task.create -->

Builds a task object for the type you name in `options.taskType`. You can set the task's
properties right here in the options object, or set them on the returned object afterwards; either
way nothing runs until you call the object's `submit()`. The spec names these uses: queuing
scheduled scripts, running map/reduce scripts, importing CSV files, merging duplicate records, and
running searches, queries, SuiteQL statements and workflows asynchronously.

Most options apply only to one family of task types; the bullets under the table say which.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskType` | string | Yes | |
| `options.deploymentId` | string | No | |
| `options.params` | Object \| Array<string \| boolean \| number> | No | |
| `options.scriptId` | number \| string | No | |
| `options.importFile` | file.File \| string | No | |
| `options.linkedFiles` | Object | No | |
| `options.mappingId` | number \| string | No | |
| `options.name` | string | No | |
| `options.queueId` | number | No | |
| `options.dedupeMode` | string | No | |
| `options.entityType` | string | No | |
| `options.masterRecordId` | number | No | |
| `options.masterSelectionMode` | string | No | |
| `options.recordIds` | number[] | No | |
| `options.recordId` | number | No | |
| `options.recordType` | string | No | |
| `options.workflowId` | number \| string | No | |
| `options.fileId` | number | No | |
| `options.filePath` | string | No | |
| `options.query` | query.Query \| string | No | |
| `options.savedSearchId` | string | No | |

- `options.taskType`: a [`task.TaskType`](#tasktasktype) value.
- `options.deploymentId`: for scheduled script and map/reduce tasks. For a map/reduce task you may
  leave it out, and an available deployment of the script is picked for you.
- `options.params`: what it holds depends on the task type. For scheduled script, map/reduce and
  workflow trigger tasks it is an object of parameter values that replace the static values stored
  on the deployment for this run (for a workflow, the keys can be workflow definition fields and
  custom fields defined on the workflow or its states). For SuiteQL tasks it is the array of values
  bound into the query.
- `options.scriptId`: for scheduled script and map/reduce tasks.
- `options.importFile`, `options.linkedFiles`, `options.mappingId`, `options.name`,
  `options.queueId`: for CSV import tasks. `linkedFiles` maps sublist IDs to a file or raw CSV text.
  `mappingId` must be an import map saved with the Import Assistant. `name` appears on the CSV
  import job status page. `queueId` overrides the queue number chosen in the Import Assistant and
  needs a `SuiteCloud Plus` license (see [`CsvImportTask.queueId`](#csvimporttaskqueueid)).
- `options.dedupeMode`, `options.entityType`, `options.masterRecordId`,
  `options.masterSelectionMode`, `options.recordIds`: for entity deduplication tasks. Choosing the
  customer entity type also covers prospects and leads. `masterRecordId` is ignored unless the
  master selection mode is `SELECT_BY_ID`. `recordIds` comes from
  [`search.duplicates`](N_search.md) or is an array of IDs you build yourself.
- `options.recordId`, `options.recordType`, `options.workflowId`: for workflow trigger tasks. The
  valid record types are the ones offered by the workflow's record type field in the workflow
  manager; the valid workflow IDs are shown in the ID field of the workflow definition page.
- `options.fileId`, `options.filePath`: for search, query and SuiteQL tasks; use one or the other,
  not both.
- `options.query`: a `query.Query` object for a `QUERY` task, or a SuiteQL string for a `SUITE_QL`
  task.
- `options.savedSearchId`: for search tasks. Typed `string` here.

> [!SOURCE-ERROR] The source types `options.savedSearchId` as a string on `task.create`, but the
> matching `SearchTask.savedSearchId` property is a number. The spec does not say which type the
> runtime expects; test with the form you plan to pass.
<!-- source-error: N/task.create | options.savedSearchId | savedsearchid_string_in_create_vs_number_on_searchtask -->

**Returns:** `Object`, the task object matching the requested type. The spec keeps the declared type
as plain `Object` because the full list of possibilities is long; the possible objects are
[`CsvImportTask`](#csvimporttask-object), [`EntityDeduplicationTask`](#entitydeduplicationtask-object),
[`MapReduceScriptTask`](#mapreducescripttask-object), [`QueryTask`](#querytask-object),
[`RecordActionTask`](#recordactiontask-object), [`ScheduledScriptTask`](#scheduledscripttask-object),
[`SearchTask`](#searchtask-object), [`SuiteQLTask`](#suiteqltask-object) and
[`WorkflowTriggerTask`](#workflowtriggertask-object), plus
[`DocumentCaptureTask`](#documentcapturetask-object), which the module's member table lists as a
possible result.

> [!SOURCE-ERROR] The return-type row of `task.create` in the source leaves out
> `DocumentCaptureTask`, although the module table lists it, `TaskType` has a `DOCUMENT_CAPTURE`
> value, and `DocumentCaptureTask` says it is created by `task.create`. Expect a
> `DocumentCaptureTask` for that task type, but verify it in your account. The source also gives no
> document-capture-specific options for `create`; set those properties on the returned object.
<!-- source-error: N/task.create | - | create_return_row_omits_documentcapturetask -->

**Errors:** the source does not list any for this function.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### task.checkStatus(options)
<!-- id: N/task.checkStatus -->

Looks up a task you submitted earlier and returns a status object whose shape matches the task's
type.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskId` | string | Yes | |

- `options.taskId`: the ID returned by the task's `submit()`.

**Returns:** `Object` (the spec keeps the declared type plain because the list is long), one of
[`CsvImportTaskStatus`](#csvimporttaskstatus-object),
[`EntityDeduplicationTaskStatus`](#entitydeduplicationtaskstatus-object),
[`MapReduceScriptTaskStatus`](#mapreducescripttaskstatus-object),
[`QueryTaskStatus`](#querytaskstatus-object), [`RecordActionTaskStatus`](#recordactiontaskstatus-object),
[`ScheduledScriptTaskStatus`](#scheduledscripttaskstatus-object),
[`SearchTaskStatus`](#searchtaskstatus-object), [`SuiteQLTaskStatus`](#suiteqltaskstatus-object) or
[`WorkflowTriggerTaskStatus`](#workflowtriggertaskstatus-object).

> [!SOURCE-ERROR] The source's list of status objects that `checkStatus` can return leaves out
> `DocumentCaptureTaskStatus`, even though that object says it is returned by `checkStatus` and
> document capture tasks point here for status. Expect a `DocumentCaptureTaskStatus` for document
> capture task IDs, but verify it in your account.
<!-- source-error: N/task.checkStatus | - | document_capture_status_not_listed -->

**Errors:** the source does not list any.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Objects

### CsvImportTask (object)
<!-- id: N/task.CsvImportTask -->

Queues an asynchronous import of record data from CSV. Create it with
`task.create({ taskType: task.TaskType.CSV_IMPORT, ... })`, `submit()` it, and follow it with
[`CsvImportTaskStatus`](#csvimporttaskstatus-object). Server scripts, since 2015.2.

Good fits: loading data while a SuiteApp installs, seeding demo or test accounts, imports that run
on a schedule, and imports pushed in through a RESTlet.

What it needs and how it behaves:

- **An import map saved in the UI first.** Build and save the map with the Import Assistant
  (sample files are fine for setting it up). The script needs the map's script ID and any linked
  files the map requires. Only the map's saved field mappings are used, together with the advanced
  options saved with the map.
- **Single thread, single queue.** The script-driven import ignores multi-threading and multiple
  queues. Queue and multi-thread settings saved on the map are not used; to pick a queue, use
  [`queueId`](#csvimporttaskqueueid).
- **Size limit:** at most 25,000 records per import.
- **Not supported:** import types that use the two-step assistant: budgets, single journal entries,
  single inventory worksheets, project tasks and website redirects.
- **Side effects:** if the map or the import preferences say to run server SuiteScript, user event
  scripts and workflows fire for the imported records.
- **Sublists.** The sublists a CSV import can carry are listed, per record type, in
  [csv_import_sublists](../gen/catalogs/csv_import_sublists.md), and many entries come with
  conditions you must meet before the import works. The kinds of conditions recorded there:
  - *A feature that must be on.* The item `Locations` sublist needs `Multi-Location Inventory`;
    `Accounting Book Detail` sublists on transactions need `Multi-Book Accounting` and, on top of
    that, more than one active accounting book. Some sublists accept any one of several features:
    item `Bin Numbers` works with either `Bin Management` or
    `Advanced Bin/Numbered Inventory Management`.
  - *A feature that must be off.* The project (job) `Address` sublist is importable only while
    `Project Management` is disabled.
  - *A preference that must be on.* Purchase order `Expenses` needs
    `Allow Expenses on Purchase Orders`.
  - *Key fields for selective updates*, which decide which existing lines a row updates. Item lines
    of invoices, estimates, sales orders and purchase orders match on `Line/Order Line` and `Item`;
    item `Bin Numbers` matches on `Bin Number`.
  - *Add-only sublists* that accept new records but cannot update existing lines (the `Credit Cards`
    sublist of customers, leads and prospects).
  - *Separate-file sublists* whose data must arrive in its own CSV file, which from a script means a
    [`linkedFiles`](#csvimporttasklinkedfiles) entry (vendor bill `Expenses`, purchase order
    `Expenses`, vendor credit `Apply`).
  - *Which import types offer a sublist at all* (item `Bin Numbers` and `Locations` only in
    `Assembly/BOM Item` and `Inventory Item` imports), and ordering rules (import bins before the
    items that reference them).
  - *Minimum line counts.* A journal entry needs at least two lines whose debits and credits
    balance; an intercompany journal entry needs at least four, with a debit and a credit for both
    the originating and the receiving subsidiary; item lines of new invoices, purchase orders and
    similar transactions need at least one line.

  Check the catalog row for every sublist your map includes.
- **Subrecords.** Importable subrecords are listed in
  [csv_import_subrecords](../gen/catalogs/csv_import_subrecords.md). See also the
  [CSV import topic](../topics/csv_import.md).

#### CsvImportTask.submit()
<!-- id: N/task.CsvImportTask#submit -->

Queues the import and returns its task ID. Problems found while validating the CSV before the
import starts are thrown straight back to the script; problems with individual rows during the
import are written to the CSV response file instead. Use `CsvImportTaskStatus.status` (through
`task.checkStatus`) to follow it.

**Returns:** `string`, a unique ID for the queued task.

**Errors:**

- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.
- Inline CSV validation errors: thrown when validation before the import fails. The source does not
  state which error codes these carry.

**Governance:** 100 units.

**Availability:** server; only in [bundle installation scripts](../script-types/BundleInstallationScript.md),
[scheduled scripts](../script-types/ScheduledScript.md) and [RESTlets](../script-types/Restlet.md).

**Since:** 2015.2

#### CsvImportTask.id
<!-- id: N/task.CsvImportTask#id -->

`string`. The task's ID. The source does not state a version for this property.

#### CsvImportTask.queueId
<!-- id: N/task.CsvImportTask#queueId -->

`number`, one of 1, 2, 3, 4 or 5. Picks the import queue, which you can use to spread imports for
better throughput; it overrides the queue number set in the Import Assistant. Using it requires a
`SuiteCloud Plus` license. Since 2015.2.

#### CsvImportTask.importFile
<!-- id: N/task.CsvImportTask#importFile -->

`file.File | string`. The data to import: either an [`N/file`](N_file.md) `File` object or the CSV
text itself as a string. Since 2015.2.

#### CsvImportTask.name
<!-- id: N/task.CsvImportTask#name -->

`string`, optional. A label for the job, shown on the CSV import job status page and in the list
of import jobs. Since 2015.2.

#### CsvImportTask.mappingId
<!-- id: N/task.CsvImportTask#mappingId -->

`number | string`. The saved import map created with the Import Assistant, given either as its
internal ID or its script ID. Saved import script IDs start with `CUSTIMPORT`. Since 2015.2.

#### CsvImportTask.linkedFiles
<!-- id: N/task.CsvImportTask#linkedFiles -->

`Object`. For multi-file imports: maps each sublist's internal ID to its data, which can be a
`File` object or CSV text; you can mix the two in one object. Linked files hold only sublist or
subrecord data, never the main record rows. Which sublists must arrive as a separate file is noted
per sublist in [csv_import_sublists](../gen/catalogs/csv_import_sublists.md). Since 2015.2.

### CsvImportTaskStatus (object)
<!-- id: N/task.CsvImportTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a CSV import task ID. Server
scripts, since 2015.2.

#### CsvImportTaskStatus.taskId
<!-- id: N/task.CsvImportTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`. The
source does not state a version for this property.

#### CsvImportTaskStatus.status
<!-- id: N/task.CsvImportTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

### DocumentCaptureTask (object)
<!-- id: N/task.DocumentCaptureTask -->

Runs document capture (text and table extraction) on a file as a background job instead of
inside your script; compare [`documentCapture.documentToStructure`](N_documentCapture.md). The
input file must already be in the File Cabinet, and the result is written as a JSON file to the
File Cabinet path given in `outputFilePath`. Any dependent tasks you attach run once the capture
finishes. If you do not set a language, `ENG` (from `documentCapture.Language`) is used.

Usual order of work: set `inputFile`, `documentType`, `features`, `language` and `outputFilePath`;
add dependent tasks; `submit()`; then check status. Created through `task.create`. Server scripts,
since 2025.2.

#### DocumentCaptureTask.submit()
<!-- id: N/task.DocumentCaptureTask#submit -->

Queues the capture job and returns its task ID. When the task is submitted, the IDs of any
dependent scripts are written into
[`inboundDependencies`](#documentcapturetaskinbounddependencies). Follow progress with
[`task.checkStatus`](#taskcheckstatusoptions), which gives a
[`DocumentCaptureTaskStatus`](#documentcapturetaskstatus-object).

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `CANNOT_RESUBMIT_SUBMITTED_DOCUMENT_CAPTURE_TASK`: this task was already submitted and has
  completed.
- `MUST_IDENTIFY_A_FILE`: the output file path points to a folder rather than a file.
- `THAT_RECORD_DOES_NOT_EXIST`: the input file cannot be found.
- `YOU_DO_NOT_HAVE_ACCESS_TO_THE_MEDIA_ITEM_YOU_SELECTED`: the script lacks permission to the input
  file.
- `DOCUMENT_CAPTURE_DEPENDENCY_SS_ALREADY_SUBMITTED`: a dependent task was already submitted and
  has not finished.
- `DOCUMENT_CAPTURE_DEPENDENCY_SS_INCORRECT_STATUS`: a dependent script's deployment is not in the
  Not Scheduled status.
- `DOCUMENT_CAPTURE_DEPLOYMENT_FOR_DEPENDENCY`: no deployment was given for a dependent script and
  none is available.
- `DOCUMENT_CAPTURE_MULTIPLE_DEPENDENCIES`: the same dependent task was added twice.
- `DOCUMENT_CAPTURE_SCRIPT_ID_NOT_FOUND`: a dependent task's script cannot be found.
- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the job could not be queued.
- `SSS_MISSING_REQD_ARGUMENT`: a required value is missing.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.2

#### DocumentCaptureTask.addInboundDependency(options)
<!-- id: N/task.DocumentCaptureTask#addInboundDependency -->

Attaches a scheduled script that should run after the capture finishes, for example to read the
JSON result file. Only scheduled scripts can be dependents here. Pass either a
`task.ScheduledScriptTask` or a plain object with `taskType`, `scriptId`, `deploymentId` and
`params`. Each call adds one task. Dependent tasks are run by SuiteCloud processors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskType` | string | Yes | |
| `options.deploymentId` | string | No | |
| `options.params` | Object | No | |
| `options.scriptId` | string | Yes | |

- `options.taskType`: must be `SCHEDULED_SCRIPT`.
- `options.deploymentId`: the script must be deployed; leave it out and an available deployment is
  chosen.
- `options.params`: script parameter values for the dependent run, typically a reference to the
  result file (for example a parameter named like `custscript_doccapture_res_result_file_param`).
  The parameters must be defined on the script.

**Returns:** `void`

**Errors:**

- `WRONG_PARAMETER_TYPE`: the dependent is not a scheduled script, or an argument has the wrong
  type.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

#### DocumentCaptureTask.id
<!-- id: N/task.DocumentCaptureTask#id -->

`string`. The task's ID. Since 2025.2.

#### DocumentCaptureTask.features
<!-- id: N/task.DocumentCaptureTask#features -->

`string[]`. Which extraction features to run, using values of
[`documentCapture.Feature`](N_documentCapture.md). Defaults to text extraction plus table
extraction (`TEXT_EXTRACTION`, `TABLE_EXTRACTION`). A value outside that enum raises
`WRONG_PARAMETER_TYPE`. Since 2025.2.

#### DocumentCaptureTask.documentType
<!-- id: N/task.DocumentCaptureTask#documentType -->

`string`. The kind of document, from [`documentCapture.DocumentType`](N_documentCapture.md). A
value outside that enum raises `INVALID_DOCUMENT_TYPE`. Since 2025.2.

#### DocumentCaptureTask.language
<!-- id: N/task.DocumentCaptureTask#language -->

`string`. The document's language, from [`documentCapture.Language`](N_documentCapture.md);
`ENG` when not set. A value outside that enum raises `INVALID_LANGUAGE`. Since 2025.2.

#### DocumentCaptureTask.outputFilePath
<!-- id: N/task.DocumentCaptureTask#outputFilePath -->

`string`. File Cabinet path of the JSON file the result is written to. It must name a file, not a
folder (see `MUST_IDENTIFY_A_FILE` under `submit`). Since 2025.2.

#### DocumentCaptureTask.inboundDependencies
<!-- id: N/task.DocumentCaptureTask#inboundDependencies -->

`Object[]`, read-only. Lets you check which dependent tasks you have added: one nested object per
dependent, keyed by a zero-based index in the order they were added, each holding `type`,
`scriptId`, `deploymentId` and `params`. After `submit()`, the dependent tasks' IDs are added to
these entries. Setting the property once the task exists throws `READ_ONLY_PROPERTY`. Since 2025.2.

#### DocumentCaptureTask.inputFile
<!-- id: N/task.DocumentCaptureTask#inputFile -->

`file.File`. The File Cabinet file to process, as an [`N/file`](N_file.md) `File` object. Only
PDF, TIFF, JPG and PNG files are accepted; anything else raises `UNSUPPORTED_FILE_TYPE`.
Since 2025.2.

#### DocumentCaptureTask.ociConfig
<!-- id: N/task.DocumentCaptureTask#ociConfig -->

`Object`. No longer supported: anything you assign is ignored. Listed with since 2025.2.

### DocumentCaptureTaskStatus (object)
<!-- id: N/task.DocumentCaptureTaskStatus -->

Status of a document capture job, returned by [`task.checkStatus`](#taskcheckstatusoptions) when you
pass the ID from `DocumentCaptureTask.submit()`; you need that ID to get it. Server scripts, since
2025.2.

#### DocumentCaptureTaskStatus.taskId
<!-- id: N/task.DocumentCaptureTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`.
Since 2025.2.

#### DocumentCaptureTaskStatus.status
<!-- id: N/task.DocumentCaptureTaskStatus#status -->

`string`, read-only. One of `COMPLETE`, `FAILED`, `PENDING` or `PROCESSING`
([`task.TaskStatus`](#tasktaskstatus)). Assigning it throws `READ_ONLY_PROPERTY`. Since 2025.2.

### EntityDeduplicationTask (object)
<!-- id: N/task.EntityDeduplicationTask -->

Merges or deletes duplicate entity records in the background; the scripted counterpart of
resolving entity duplicates in the UI, on the entity duplicate resolution page under the mass
update lists. Records that get merged drop off that duplicate resolution list. You can rely on
predefined duplicate rules or your own.

- At most 200 records per task.
- Entity records only; other record types are not supported.
- Needs the Duplicate Record Management permission at the Full level.
- Usual order: `task.create`, set `entityType`, `dedupeMode` and `masterSelectionMode` (plus
  `masterRecordId` when the mode is `SELECT_BY_ID`), find the duplicates with
  [`search.duplicates`](N_search.md) and put their IDs in `recordIds`, `submit()`, then read the
  status object.

Server scripts, since 2015.2.

#### EntityDeduplicationTask.submit()
<!-- id: N/task.EntityDeduplicationTask#submit -->

Queues the deduplication job. Follow it with `EntityDeduplicationTaskStatus.status`.

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the job could not be queued.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2015.2

#### EntityDeduplicationTask.id
<!-- id: N/task.EntityDeduplicationTask#id -->

`string`. The task's ID. The source does not state a version for this property.

#### EntityDeduplicationTask.masterRecordId
<!-- id: N/task.EntityDeduplicationTask#masterRecordId -->

`number`. The ID of the record that survives: depending on the mode, the other duplicates are
deleted or merged into it. Ignored unless `masterSelectionMode` is `SELECT_BY_ID`. Since 2015.2.

#### EntityDeduplicationTask.recordIds
<!-- id: N/task.EntityDeduplicationTask#recordIds -->

`number[]`. Internal IDs of the records to merge or delete, normally gathered with
[`search.duplicates`](N_search.md). Since 2015.2.

#### EntityDeduplicationTask.dedupeMode
<!-- id: N/task.EntityDeduplicationTask#dedupeMode -->

`string`. What to do with the duplicates, a [`task.DedupeMode`](#taskdedupemode) value. Since 2015.2.

#### EntityDeduplicationTask.entityType
<!-- id: N/task.EntityDeduplicationTask#entityType -->

`string`. The entity type, a [`task.DedupeEntityType`](#taskdedupeentitytype) value. Choosing the
customer type also covers prospects and leads. Since 2015.2.

#### EntityDeduplicationTask.masterSelectionMode
<!-- id: N/task.EntityDeduplicationTask#masterSelectionMode -->

`string`. Decides which duplicate becomes the master, a
[`task.MasterSelectionMode`](#taskmasterselectionmode) value. Since 2015.2.

### EntityDeduplicationTaskStatus (object)
<!-- id: N/task.EntityDeduplicationTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a deduplication task ID. Server
scripts, since 2015.2.

#### EntityDeduplicationTaskStatus.taskId
<!-- id: N/task.EntityDeduplicationTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`. The
source does not state a version for this property.

#### EntityDeduplicationTaskStatus.status
<!-- id: N/task.EntityDeduplicationTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

### MapReduceScriptTask (object)
<!-- id: N/task.MapReduceScriptTask -->

Submits a map/reduce script deployment from code. The script record and a deployment record must
already exist. Usual order: create the task, set `scriptId` and `deploymentId`, `submit()`, then
check status. Background on how submissions are processed is in
[Map/reduce submission](../topics/map_reduce.submission.md). Server scripts, since 2015.2.

#### MapReduceScriptTask.submit()
<!-- id: N/task.MapReduceScriptTask#submit -->

Queues the deployment. The run uses the user and role of the script that submitted it.

- Only one unfinished task may exist for a given script and deployment pair; submit again only
  after the earlier run is done.
- If a map/reduce script submits its own deployment, the new run waits until the current one has
  finished, and in that case the method does not hand back a task ID. The source does not state
  what it returns instead.
- Needs the Administrator role, or both the SuiteScript and SuiteScript Scheduling permissions.
- Submitting many times in quick succession can leave some submissions short of the queued state;
  if a submission fails, submit it again.

> [!SOURCE-ERROR] The source's note about rapid submissions on this map/reduce method talks about
> scheduled scripts, apparently copied from the scheduled script page. The spec keeps the caveat
> for map/reduce, but the source does not state it cleanly for this task type; treat it as likely
> but verify.
<!-- source-error: N/task.MapReduceScriptTask#submit | - | rapid_submission_note_names_scheduled_scripts -->

**Returns:** `string`, the task ID (except for the self-submission case above).

**Errors:**

- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTask.id
<!-- id: N/task.MapReduceScriptTask#id -->

`string`. The task's ID. The source does not state a version for this property.

#### MapReduceScriptTask.params
<!-- id: N/task.MapReduceScriptTask#params -->

`Object`. Maps script parameter IDs (such as one named like `custscript_my_script_param`) to
values. These replace the values stored on the deployment for this run. Since 2015.2.

#### MapReduceScriptTask.scriptId
<!-- id: N/task.MapReduceScriptTask#scriptId -->

`number | string`. The map/reduce script, as its internal ID (number) or script ID (string).
Since 2015.2.

#### MapReduceScriptTask.deploymentId
<!-- id: N/task.MapReduceScriptTask#deploymentId -->

`string`. The script ID of the deployment to run. Since 2015.2.

### MapReduceScriptTaskStatus (object)
<!-- id: N/task.MapReduceScriptTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a map/reduce task ID. Besides the
overall status it tells you which stage is running and offers counters for each stage. The counter
methods cost governance (10 or 25 units each), so call only the ones you need. See also the
[map/reduce status page](../topics/map_reduce.status_page.md). Server scripts, since 2015.2.

#### MapReduceScriptTaskStatus.getTotalMapCount()
<!-- id: N/task.MapReduceScriptTaskStatus#getTotalMapCount -->

Number of rows given to the map stage.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPendingMapCount()
<!-- id: N/task.MapReduceScriptTaskStatus#getPendingMapCount -->

Number of rows the map stage has not processed yet.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPendingMapSize()
<!-- id: N/task.MapReduceScriptTaskStatus#getPendingMapSize -->

Size in bytes of the data the map stage has not processed yet.

**Returns:** `number`

**Governance:** 25 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getTotalReduceCount()
<!-- id: N/task.MapReduceScriptTaskStatus#getTotalReduceCount -->

Number of rows given to the reduce stage.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPendingReduceCount()
<!-- id: N/task.MapReduceScriptTaskStatus#getPendingReduceCount -->

Number of rows the reduce stage has not processed yet.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPendingReduceSize()
<!-- id: N/task.MapReduceScriptTaskStatus#getPendingReduceSize -->

Size in bytes of the data the reduce stage has not processed yet.

**Returns:** `number`

**Governance:** 25 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getTotalOutputCount()
<!-- id: N/task.MapReduceScriptTaskStatus#getTotalOutputCount -->

Number of key/value pairs passed into the summarize stage.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPendingOutputCount()
<!-- id: N/task.MapReduceScriptTaskStatus#getPendingOutputCount -->

Number of output rows not yet processed. The source does not narrow this to a stage.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPendingOutputSize()
<!-- id: N/task.MapReduceScriptTaskStatus#getPendingOutputSize -->

Size in bytes of the key/value pairs written as output.

> [!SOURCE-ERROR] The method's name says "pending", but the source describes it as the size of all
> output written, unlike its siblings `getPendingMapSize` and `getPendingReduceSize`, which measure
> unprocessed data. The spec does not say which reading is right; check the figure against a known
> run before relying on it.
<!-- source-error: N/task.MapReduceScriptTaskStatus#getPendingOutputSize | - | pendingoutputsize_described_as_all_output_bytes -->

**Returns:** `number`

**Governance:** 25 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getCurrentTotalSize()
<!-- id: N/task.MapReduceScriptTaskStatus#getCurrentTotalSize -->

Size in bytes of all work-in-progress data currently stored for the run.

**Returns:** `number`

**Governance:** 25 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.getPercentageCompleted()
<!-- id: N/task.MapReduceScriptTaskStatus#getPercentageCompleted -->

How far the current stage has progressed, as a percentage. For the input and summarize stages the
value is only ever 0 or 100.

**Returns:** `number`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### MapReduceScriptTaskStatus.taskId
<!-- id: N/task.MapReduceScriptTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`. The
source does not state a version for this property.

#### MapReduceScriptTaskStatus.deploymentId
<!-- id: N/task.MapReduceScriptTaskStatus#deploymentId -->

`string`, read-only. Script ID of the deployment that is running. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

#### MapReduceScriptTaskStatus.scriptId
<!-- id: N/task.MapReduceScriptTaskStatus#scriptId -->

`number`, read-only. Internal ID of the map/reduce script. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

#### MapReduceScriptTaskStatus.status
<!-- id: N/task.MapReduceScriptTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

#### MapReduceScriptTaskStatus.stage
<!-- id: N/task.MapReduceScriptTaskStatus#stage -->

`string`, read-only. The stage currently running, a [`task.MapReduceStage`](#taskmapreducestage)
value. Since 2015.2.

### QueryTask (object)
<!-- id: N/task.QueryTask -->

Runs an [`N/query`](N_query.md) query asynchronously and writes the results to a CSV file in the
File Cabinet. Usual order: set the query, set the CSV file (by ID or path), optionally add
dependent tasks, `submit()`, then check status. Dependent tasks start once the query task has
finished. Created through `task.create`. Server scripts, since 2020.2.

#### QueryTask.addInboundDependency(options)
<!-- id: N/task.QueryTask#addInboundDependency -->

Attaches a script to run after the query task finishes, typically to process the CSV. Dependents
must be scheduled scripts or map/reduce scripts. Pass a `task.ScheduledScriptTask`, a
`task.MapReduceScriptTask`, or a plain object with `taskType`, `scriptId`, `deploymentId` and
`params`. One task per call; dependent tasks are run by SuiteCloud processors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskType` | string | Yes | |
| `options.deploymentId` | string | No | |
| `options.params` | Object | No | |
| `options.scriptId` | string | Yes | |

- `options.taskType`: `SCHEDULED_SCRIPT` or `MAP_REDUCE`.
- `options.deploymentId`: the script must be deployed; leave it out and an available deployment is
  chosen.
- `options.params`: parameter values for the dependent script, which must define those parameters
  (for example one named like `custscript_query_res_result_csv` to receive the result file).

**Returns:** `void`

**Errors:** the source does not list any for this method.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

#### QueryTask.submit()
<!-- id: N/task.QueryTask#submit -->

Queues the query export and returns its ID. On submit, the dependent tasks' IDs are added to
[`inboundDependencies`](#querytaskinbounddependencies). A query task that has completed cannot be
submitted again.

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `ASYNC_QUERY_DEPENDENCY_MR_ALREADY_SUBMITTED`, `ASYNC_QUERY_DEPENDENCY_SS_ALREADY_SUBMITTED`: a
  dependent map/reduce or scheduled task was already submitted and has not finished.
- `ASYNC_QUERY_DEPENDENCY_MR_INCORRECT_STATUS`, `ASYNC_QUERY_DEPENDENCY_SS_INCORRECT_STATUS`: a
  dependent script's deployment is in the wrong state (for scheduled scripts: not Not Scheduled).
- `ASYNC_QUERY_DEPLOYMENT_FOR_DEPENDENCY`: no deployment was given for a dependent and none is
  available.
- `ASYNC_QUERY_MULTIPLE_DEPENDENCIES`: the same dependent task was added twice.
- `ASYNC_QUERY_QUERY_ID_NOT_FOUND`: the query cannot be found.
- `ASYNC_QUERY_SCRIPT_ID_NOT_FOUND`: a dependent's script cannot be found.
- `CANNOT_RESUBMIT_SUBMITTED_ASYNC_QUERY_TASK`: the task was already submitted.
- `MUST_IDENTIFY_A_FILE`: the file path points to a folder.
- `THAT_RECORD_DOES_NOT_EXIST`: the CSV file cannot be found.
- `YOU_DO_NOT_HAVE_ACCESS_TO_THE_MEDIA_ITEM_YOU_SELECTED`: no permission to the CSV file.
- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.
- `SSS_MISSING_REQD_ARGUMENT`: a required value is missing.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2020.2

#### QueryTask.id
<!-- id: N/task.QueryTask#id -->

`string`. The task's ID.

> [!SOURCE-ERROR] The source leaves the "since" version for this property blank, so the spec has
> no version for it. The object itself dates from 2020.2; do not assume the property's version
> without checking.
<!-- source-error: N/task.QueryTask#id | - | since_row_blank -->

#### QueryTask.fileId
<!-- id: N/task.QueryTask#fileId -->

`number`. Internal ID of the CSV file in the File Cabinet that receives the results. Use either
this or [`filePath`](#querytaskfilepath); setting both raises `PROPERTY_VALUE_CONFLICT`.
Since 2020.2.

#### QueryTask.filePath
<!-- id: N/task.QueryTask#filePath -->

`string`. File Cabinet path of the CSV file that receives the results. Use either this or
[`fileId`](#querytaskfileid); setting both raises `PROPERTY_VALUE_CONFLICT`. Since 2020.2.

#### QueryTask.inboundDependencies
<!-- id: N/task.QueryTask#inboundDependencies -->

`Object[]`, read-only. Shows the dependent tasks added with
[`addInboundDependency`](#querytaskaddinbounddependencyoptions), keyed by a zero-based index in the
order they were added; each entry has `type`, `scriptId`, `deploymentId` and `params`. After
`submit()`, each entry also gets the dependent task's `id`. Setting the property once the task
exists throws `READ_ONLY_PROPERTY`. Since 2020.2.

#### QueryTask.query
<!-- id: N/task.QueryTask#query -->

`query.Query`. The query to run, built with [`query.create`](N_query.md) or loaded with
[`query.load`](N_query.md). Since 2020.2.

### QueryTaskStatus (object)
<!-- id: N/task.QueryTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a query task ID. If you poll for it,
remember that the polling script is still bound by the normal server script time limits. Server
scripts, since 2020.2.

#### QueryTaskStatus.fileId
<!-- id: N/task.QueryTaskStatus#fileId -->

`number`, read-only. The CSV file of the query task. Since 2020.2.

#### QueryTaskStatus.query
<!-- id: N/task.QueryTaskStatus#query -->

`query.Query`, read-only. The query the task ran. Since 2020.2.

#### QueryTaskStatus.status
<!-- id: N/task.QueryTaskStatus#status -->

`string`, read-only. One of `COMPLETE`, `FAILED`, `PENDING` or `PROCESSING`
([`task.TaskStatus`](#tasktaskstatus)). Since 2020.2.

#### QueryTaskStatus.taskId
<!-- id: N/task.QueryTaskStatus#taskId -->

`string`, read-only. The same ID that `QueryTask.submit()` returned. Since 2020.2.

### RecordActionTask (object)
<!-- id: N/task.RecordActionTask -->

Runs a record action (see [N/action](N_action.md)) asynchronously against a set of records. Server
scripts, since 2019.1.

#### RecordActionTask.submit()
<!-- id: N/task.RecordActionTask#submit -->

Queues the job. A background process then runs the action once for each record ID. Use
[`task.checkStatus`](#taskcheckstatusoptions) to see both the overall outcome and each action's
result ([`RecordActionTaskStatus`](#recordactiontaskstatus-object)).

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.

**Governance:** 50 units.

**Availability:** server scripts.

**Since:** 2019.1

#### RecordActionTask.recordType
<!-- id: N/task.RecordActionTask#recordType -->

`string`. The record type, a [`record.Type`](N_record.md) value. Since 2019.1.

#### RecordActionTask.action
<!-- id: N/task.RecordActionTask#action -->

`string`. The ID of the action to run. Since 2019.1.

#### RecordActionTask.condition
<!-- id: N/task.RecordActionTask#condition -->

`Object`. Selects records by condition instead of listing them; the only allowed value is
[`task.ActionCondition`](#taskactioncondition)`.ALL_QUALIFIED_INSTANCES`. Used together with
[`paramCallback`](#recordactiontaskparamcallback); if no callback is set, a default one is used
that returns an object holding just the `recordId`. Since 2019.1.

#### RecordActionTask.paramCallback
<!-- id: N/task.RecordActionTask#paramCallback -->

`function`. Receives a record ID and returns the parameter object for that record's action. Used
with [`condition`](#recordactiontaskcondition). Do not combine it with
[`params`](#recordactiontaskparams). Since 2019.1.

#### RecordActionTask.params
<!-- id: N/task.RecordActionTask#params -->

`Object[]`. One parameter object per record. Each object must carry `recordId`, plus whatever
parameters the action takes. Cannot be combined with `paramCallback`. Since 2019.1.

#### RecordActionTask.id
<!-- id: N/task.RecordActionTask#id -->

`string`. The task's ID. The source does not state a version for this property.

### RecordActionTaskStatus (object)
<!-- id: N/task.RecordActionTaskStatus -->

Status of a record action task, returned by [`task.checkStatus`](#taskcheckstatusoptions). Server
scripts, since 2019.1.

#### RecordActionTaskStatus.status
<!-- id: N/task.RecordActionTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Since 2019.1.

#### RecordActionTaskStatus.results
<!-- id: N/task.RecordActionTaskStatus#results -->

`Object`, read-only. Maps record IDs to the result of each action that succeeded. Since 2019.1.

#### RecordActionTaskStatus.errors
<!-- id: N/task.RecordActionTaskStatus#errors -->

`Object`, read-only. Maps record IDs to the error for each action that failed; each error carries
a code and a message. Since 2019.1.

#### RecordActionTaskStatus.complete
<!-- id: N/task.RecordActionTaskStatus#complete -->

`number`, read-only. How many actions have run so far, whether they failed or succeeded.
Since 2019.1.

#### RecordActionTaskStatus.succeeded
<!-- id: N/task.RecordActionTaskStatus#succeeded -->

`number`, read-only. How many actions succeeded. Since 2019.1.

#### RecordActionTaskStatus.failed
<!-- id: N/task.RecordActionTaskStatus#failed -->

`number`, read-only. How many actions failed. Since 2019.1.

#### RecordActionTaskStatus.pending
<!-- id: N/task.RecordActionTaskStatus#pending -->

`number`, read-only. How many actions have not run yet. Since 2019.1.

#### RecordActionTaskStatus.taskId
<!-- id: N/task.RecordActionTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`. The
source does not state a version for this property.

### ScheduledScriptTask (object)
<!-- id: N/task.ScheduledScriptTask -->

Puts a scheduled script deployment in the queue on demand. Usual order: create the script and its
deployment in the account, create the task, set its properties, `submit()`, then check status.
Server scripts, since 2015.2.

#### ScheduledScriptTask.submit()
<!-- id: N/task.ScheduledScriptTask#submit -->

Queues the deployment.

- The deployment is expected to have the status Not Scheduled. A deployment in Testing status is
  not queued; one in Scheduled status is queued at its scheduled times.
- Only one unfinished task may exist for a given script and deployment pair.
- If the script submits its own deployment, the new run waits until the current execution ends, and
  the method does not return a task ID in that case. The source does not state what it returns
  instead.
- Needs the SuiteScript Scheduling permission or the Administrator role. When the call happens in a
  user event script, the role of the user who triggered the event must have that permission.
- Submissions made in quick succession may not all reach the queued state.

> [!SOURCE-ERROR] The source requires the deployment to be Not Scheduled and, in the same place,
> describes what happens when it is Scheduled, so the two statements do not fit together. The spec
> does not resolve this; use a Not Scheduled deployment for on-demand submission and verify the
> behaviour for other statuses in your account.
<!-- source-error: N/task.ScheduledScriptTask#submit | - | submit_requires_not_scheduled_but_describes_scheduled_effect -->

**Returns:** `string`, the task ID (except for the self-submission case above).

**Errors:**

- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ScheduledScriptTask.id
<!-- id: N/task.ScheduledScriptTask#id -->

`string`. The task's ID. Since 2015.2.

#### ScheduledScriptTask.params
<!-- id: N/task.ScheduledScriptTask#params -->

`Object`. Script parameter values that replace the deployment's stored values for this run.
Since 2015.2.

#### ScheduledScriptTask.scriptId
<!-- id: N/task.ScheduledScriptTask#scriptId -->

`number | string`. The scheduled script, as its internal ID (number) or script ID (string).
Since 2015.2.

#### ScheduledScriptTask.deploymentId
<!-- id: N/task.ScheduledScriptTask#deploymentId -->

`string`. The script ID of the deployment. Since 2015.2.

### ScheduledScriptTaskStatus (object)
<!-- id: N/task.ScheduledScriptTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a scheduled script task ID. Server
scripts, since 2015.2.

#### ScheduledScriptTaskStatus.taskId
<!-- id: N/task.ScheduledScriptTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`. The
source does not state a version for this property.

#### ScheduledScriptTaskStatus.deploymentId
<!-- id: N/task.ScheduledScriptTaskStatus#deploymentId -->

`string`, read-only. Script ID of the deployment. Assigning it throws `READ_ONLY_PROPERTY`.
Since 2015.2.

#### ScheduledScriptTaskStatus.status
<!-- id: N/task.ScheduledScriptTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

#### ScheduledScriptTaskStatus.scriptId
<!-- id: N/task.ScheduledScriptTaskStatus#scriptId -->

`number`, read-only. Internal ID of the script. Assigning it throws `READ_ONLY_PROPERTY`.
Since 2015.2.

### SearchTask (object)
<!-- id: N/task.SearchTask -->

Runs a saved search asynchronously and writes the results to a CSV file in the File Cabinet,
comparable to a SuiteAnalytics persisted search. Reach for it when result volumes are large.

- Set exactly one of [`fileId`](#searchtaskfileid) or [`filePath`](#searchtaskfilepath).
- Dependent scripts added with [`addInboundDependency`](#searchtaskaddinbounddependencydependentscript)
  run when the search task has finished.
- How many asynchronous searches can run at once follows the same limit as CSV imports.
- The output file's maximum size is whatever the File Cabinet allows.

Created through `task.create`. Server scripts, since 2017.1.

#### SearchTask.addInboundDependency(dependentScript)
<!-- id: N/task.SearchTask#addInboundDependency -->

Attaches a scheduled script or map/reduce task that runs once the search has finished. Only those
two kinds of dependent are allowed, one per call; dependent tasks are run by SuiteCloud processors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `dependentScript` | task.ScheduledScriptTask \| task.MapReduceScriptTask | Yes | |

- `dependentScript`: since 2018.2.

> [!SOURCE-ERROR] The sources disagree on what this method accepts. The method's own page (whose
> version the spec keeps, shown in the table) takes only a task object as `dependentScript`;
> elsewhere the method is described like its siblings, also accepting a plain object with
> `taskType`, `scriptId`, `deploymentId` and `params`. The spec does not say which is right, so
> passing a task object is the safe choice.
<!-- source-error: N/task.SearchTask#addInboundDependency | - | page_takes_dependentscript_task_object_only -->

**Returns:** `void`

**Errors:** the source does not list any for this method.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2018.2

#### SearchTask.submit()
<!-- id: N/task.SearchTask#submit -->

Queues the search export and returns its ID. On submit, the dependent tasks' IDs are added to
[`inboundDependencies`](#searchtaskinbounddependencies). A search task that has completed cannot be
submitted again.

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `ASYNC_SEARCH_DEPENDENCY_MR_ALREADY_SUBMITTED`, `ASYNC_SEARCH_DEPENDENCY_SS_ALREADY_SUBMITTED`: a
  dependent task was already submitted and has not finished.
- `ASYNC_SEARCH_DEPENDENCY_MR_INCORRECT_STATUS`, `ASYNC_SEARCH_DEPENDENCY_SS_INCORRECT_STATUS`: a
  dependent script's deployment is in the wrong state (for scheduled scripts: not Not Scheduled).
- `ASYNC_SEARCH_DEPLOYMENT_FOR_DEPENDENCY`: no deployment was given for a dependent and none is
  available.
- `ASYNC_SEARCH_MULTIPLE_DEPENDENCIES`: the same dependent task was added twice.
- `ASYNC_SEARCH_SCRIPT_ID_NOT_FOUND`: a dependent's script cannot be found.
- `ASYNC_SEARCH_SEARCH_ID_NOT_FOUND`: the saved search cannot be found.
- `CANNOT_RESUBMIT_SUBMITTED_ASYNC_SEARCH_TASK`: the task was already submitted.
- `MUST_IDENTIFY_A_FILE`: the file path points to a folder.
- `THAT_RECORD_DOES_NOT_EXIST`: something referenced cannot be found.
- `YOU_DO_NOT_HAVE_ACCESS_TO_THE_MEDIA_ITEM_YOU_SELECTED`: permission to the file is missing.
- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.
- `SSS_MISSING_REQD_ARGUMENT`: a required value is missing.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2017.1

#### SearchTask.inboundDependencies
<!-- id: N/task.SearchTask#inboundDependencies -->

`Object[]`, read-only. Lets you verify the dependent scripts you added: one nested object per
dependent, keyed by a zero-based index in the order added, each with `type`, `scriptId`,
`deploymentId` and `params`. After `submit()`, the dependent task IDs are added. Assigning the
property throws `READ_ONLY_PROPERTY`. Since 2018.2.

#### SearchTask.id
<!-- id: N/task.SearchTask#id -->

`string`. The task's ID. The source does not state a version for this property.

#### SearchTask.savedSearchId
<!-- id: N/task.SearchTask#savedSearchId -->

`number`. ID of the saved search to run. Since 2017.1. (`task.create` types the matching option as
`string`; see the note there.)

#### SearchTask.fileId
<!-- id: N/task.SearchTask#fileId -->

`number`. Internal ID of the CSV file in the File Cabinet that receives the results. Setting both
this and [`filePath`](#searchtaskfilepath) raises `UNSUPPORTED_COMBINATION_OF_PARAMETERS`.
Since 2017.1.

#### SearchTask.filePath
<!-- id: N/task.SearchTask#filePath -->

`string`. File Cabinet path of the CSV file that receives the results. Setting both this and
[`fileId`](#searchtaskfileid) raises `UNSUPPORTED_COMBINATION_OF_PARAMETERS`. Since 2017.1.

### SearchTaskStatus (object)
<!-- id: N/task.SearchTaskStatus -->

Status of a search task, from [`task.checkStatus`](#taskcheckstatusoptions). A script that polls for
it is still subject to the normal server script time limits. Server scripts, since 2017.1.

#### SearchTaskStatus.status
<!-- id: N/task.SearchTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2017.1.

#### SearchTaskStatus.taskId
<!-- id: N/task.SearchTaskStatus#taskId -->

`string`, read-only. The same ID that `SearchTask.submit()` returned. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2017.1.

#### SearchTaskStatus.savedSearchId
<!-- id: N/task.SearchTaskStatus#savedSearchId -->

`number`, read-only. ID of the saved search that ran. Assigning it throws `READ_ONLY_PROPERTY`.
Since 2017.1.

#### SearchTaskStatus.fileId
<!-- id: N/task.SearchTaskStatus#fileId -->

`number`, read-only. ID of the CSV file holding the results. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2017.1.

### SuiteQLTask (object)
<!-- id: N/task.SuiteQLTask -->

Runs a SuiteQL statement asynchronously and writes the results to a CSV file in the File Cabinet.
Dependent tasks start once the SuiteQL task has finished. Created through `task.create`. See the
[SuiteQL topic](../topics/suiteql.md) for the language itself. Server scripts, since 2020.2.

#### SuiteQLTask.addInboundDependency(options)
<!-- id: N/task.SuiteQLTask#addInboundDependency -->

Attaches a script to run after the SuiteQL task finishes. Dependents must be scheduled scripts or
map/reduce scripts. Pass a `task.ScheduledScriptTask`, a `task.MapReduceScriptTask`, or a plain
object with `taskType`, `scriptId`, `deploymentId` and `params`. One task per call; dependent tasks
are run by SuiteCloud processors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskType` | string | Yes | |
| `options.deploymentId` | string | No | |
| `options.params` | Object | No | |
| `options.scriptId` | string | Yes | |

- `options.taskType`: `SCHEDULED_SCRIPT` or `MAP_REDUCE`.
- `options.deploymentId`: the script must be deployed; leave it out and an available deployment is
  chosen.
- `options.params`: parameter values for the dependent, which must define those parameters (for
  example one named like `custscript_query_res_result_csv`).

**Returns:** `void`

**Errors:** the source does not list any for this method.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

#### SuiteQLTask.submit()
<!-- id: N/task.SuiteQLTask#submit -->

Queues the SuiteQL export and returns its ID. On submit, the dependent tasks' IDs are added to
[`inboundDependencies`](#suiteqltaskinbounddependencies). A SuiteQL task that has completed cannot
be submitted again.

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `ASYNC_SUITEQL_DEPENDENCY_MR_ALREADY_SUBMITTED`, `ASYNC_SUITEQL_DEPENDENCY_SS_ALREADY_SUBMITTED`:
  a dependent task was already submitted and has not finished.
- `ASYNC_SUITEQL_DEPENDENCY_MR_INCORRECT_STATUS`, `ASYNC_SUITEQL_DEPENDENCY_SS_INCORRECT_STATUS`: a
  dependent script's deployment is in the wrong state (for scheduled scripts: not Not Scheduled).
- `ASYNC_SUITEQL_DEPLOYMENT_FOR_DEPENDENCY`: no deployment was given for a dependent and none is
  available.
- `ASYNC_SUITEQL_MULTIPLE_DEPENDENCIES`: the same dependent task was added twice.
- `ASYNC_SUITEQL_SCRIPT_ID_NOT_FOUND`: a dependent's script cannot be found.
- `ASYNC_SUITEQL_SUITEQL_ID_NOT_FOUND`: the SuiteQL task cannot be found.
- `CANNOT_RESUBMIT_SUBMITTED_ASYNC_SUITEQL_TASK`: the task was already submitted.
- `MUST_IDENTIFY_A_FILE`: the file path points to a folder.
- `THAT_RECORD_DOES_NOT_EXIST`: the CSV file cannot be found.
- `YOU_DO_NOT_HAVE_ACCESS_TO_THE_MEDIA_ITEM_YOU_SELECTED`: no permission to the CSV file.
- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.
- `SSS_MISSING_REQD_ARGUMENT`: a required value is missing.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2020.2

#### SuiteQLTask.id
<!-- id: N/task.SuiteQLTask#id -->

`string`. The task's ID.

> [!SOURCE-ERROR] The source leaves this property's "since" version blank, and a neighbouring row
> on the same page names `QueryTask` instead of `SuiteQLTask`, so the page looks copied from the
> query task page. The spec gives no version; verify before relying on one.
<!-- source-error: N/task.SuiteQLTask#id | - | since_blank_and_siblings_row_says_querytask -->

#### SuiteQLTask.fileId
<!-- id: N/task.SuiteQLTask#fileId -->

`number`. Internal ID of the CSV file in the File Cabinet that receives the results. Use either
this or [`filePath`](#suiteqltaskfilepath); setting both raises `PROPERTY_VALUE_CONFLICT`.
Since 2020.2.

#### SuiteQLTask.filePath
<!-- id: N/task.SuiteQLTask#filePath -->

`string`. File Cabinet path of the CSV file that receives the results. Use either this or
[`fileId`](#suiteqltaskfileid); setting both raises `PROPERTY_VALUE_CONFLICT`. Since 2020.2.

#### SuiteQLTask.inboundDependencies
<!-- id: N/task.SuiteQLTask#inboundDependencies -->

`Object[]`, read-only. One nested object per dependent task, indexed from 0 in the order they were
added, each with `type`, `scriptId`, `deploymentId` and `params`. After the task is submitted, the
dependent task IDs are added. Setting the property once the task exists throws
`READ_ONLY_PROPERTY`. Since 2020.2.

> [!SOURCE-ERROR] The source's description of this property says the IDs appear after
> `QueryTask.submit()` is called, although this is the SuiteQL task. Read it as
> `SuiteQLTask.submit()`; the spec does not confirm the wording beyond flagging it.
<!-- source-error: N/task.SuiteQLTask#inboundDependencies | - | page_says_querytask_submit_for_suiteql_task -->

#### SuiteQLTask.params
<!-- id: N/task.SuiteQLTask#params -->

`Array<string | boolean | number>`. Values substituted into the SuiteQL statement when the task is
submitted. Since 2020.2.

#### SuiteQLTask.query
<!-- id: N/task.SuiteQLTask#query -->

`string`. The SuiteQL statement to run. Since 2020.2.

### SuiteQLTaskStatus (object)
<!-- id: N/task.SuiteQLTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a SuiteQL task ID. Server scripts,
since 2020.2.

#### SuiteQLTaskStatus.fileId
<!-- id: N/task.SuiteQLTaskStatus#fileId -->

`number`, read-only. The CSV file of the SuiteQL task. Since 2020.2.

#### SuiteQLTaskStatus.query
<!-- id: N/task.SuiteQLTaskStatus#query -->

`string`, read-only. The SuiteQL statement of the task. Since 2020.2.

#### SuiteQLTaskStatus.params
<!-- id: N/task.SuiteQLTaskStatus#params -->

`Array<string | boolean | number>`, read-only. The parameter values of the task. Since 2020.2.

#### SuiteQLTaskStatus.status
<!-- id: N/task.SuiteQLTaskStatus#status -->

`string`, read-only. One of `COMPLETE`, `FAILED`, `PENDING` or `PROCESSING`
([`task.TaskStatus`](#tasktaskstatus)). Since 2020.2.

#### SuiteQLTaskStatus.taskId
<!-- id: N/task.SuiteQLTaskStatus#taskId -->

`string`, read-only. The same ID that `SuiteQLTask.submit()` returned. Since 2020.2.

### WorkflowTriggerTask (object)
<!-- id: N/task.WorkflowTriggerTask -->

Starts a workflow on a record without waiting for it: the workflow begins when the task reaches
the front of the queue. If an identical instance (same record type, record ID and workflow) is
already running or queued, the new one is not queued. Server scripts, since 2015.2.

#### WorkflowTriggerTask.submit()
<!-- id: N/task.WorkflowTriggerTask#submit -->

Queues the workflow start. Follow it with `WorkflowTriggerTaskStatus.status`.

**Returns:** `string`, a unique ID for the task.

**Errors:**

- `FAILED_TO_SUBMIT_JOB_REQUEST_1`: the task could not be queued.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

#### WorkflowTriggerTask.id
<!-- id: N/task.WorkflowTriggerTask#id -->

`string`. The task's ID. The source does not state a version for this property.

#### WorkflowTriggerTask.params
<!-- id: N/task.WorkflowTriggerTask#params -->

`Object`. Default values for workflow fields: both fields on the workflow definition and custom
fields at workflow or state level. Since 2015.2.

#### WorkflowTriggerTask.recordType
<!-- id: N/task.WorkflowTriggerTask#recordType -->

`string`. The workflow's base record type, as offered by the record type field in the workflow
manager (values such as `customer`, `salesorder` or `lead`). Since 2015.2.

#### WorkflowTriggerTask.recordId
<!-- id: N/task.WorkflowTriggerTask#recordId -->

`number`. Internal ID of the base record to run the workflow on (an ID such as 55 or 124).
Since 2015.2.

#### WorkflowTriggerTask.workflowId
<!-- id: N/task.WorkflowTriggerTask#workflowId -->

`number | string`. The workflow, as its internal ID (number) or script ID (string); valid values
are shown in the ID field of the workflow definition page. Since 2015.2.

### WorkflowTriggerTaskStatus (object)
<!-- id: N/task.WorkflowTriggerTaskStatus -->

Returned by [`task.checkStatus`](#taskcheckstatusoptions) for a workflow trigger task ID. Server
scripts, since 2015.2.

#### WorkflowTriggerTaskStatus.taskId
<!-- id: N/task.WorkflowTriggerTaskStatus#taskId -->

`string`, read-only. The ID of the submitted task. Assigning it throws `READ_ONLY_PROPERTY`. The
source does not state a version for this property.

#### WorkflowTriggerTaskStatus.status
<!-- id: N/task.WorkflowTriggerTaskStatus#status -->

`string`, read-only. A [`task.TaskStatus`](#tasktaskstatus) value. Assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

## Enums

### task.TaskType
<!-- id: N/task.TaskType -->

The kind of task [`task.create`](#taskcreateoptions) builds. Since 2015.2.

| Value | Builds |
|---|---|
| `CSV_IMPORT` | [`CsvImportTask`](#csvimporttask-object) |
| `DOCUMENT_CAPTURE` | [`DocumentCaptureTask`](#documentcapturetask-object) |
| `ENTITY_DEDUPLICATION` | [`EntityDeduplicationTask`](#entitydeduplicationtask-object) |
| `MAP_REDUCE` | [`MapReduceScriptTask`](#mapreducescripttask-object) |
| `QUERY` | [`QueryTask`](#querytask-object) |
| `RECORD_ACTION` | [`RecordActionTask`](#recordactiontask-object) |
| `SCHEDULED_SCRIPT` | [`ScheduledScriptTask`](#scheduledscripttask-object) |
| `SEARCH` | [`SearchTask`](#searchtask-object) |
| `SUITE_QL` | [`SuiteQLTask`](#suiteqltask-object) |
| `WORKFLOW_TRIGGER` | [`WorkflowTriggerTask`](#workflowtriggertask-object) |

### task.TaskStatus
<!-- id: N/task.TaskStatus -->

Values of the `status` property on every task status object. Since 2015.2. The source does not
describe what each value means beyond its name.

| Value |
|---|
| `COMPLETE` |
| `FAILED` |
| `PENDING` |
| `PROCESSING` |

### task.MapReduceStage
<!-- id: N/task.MapReduceStage -->

Values of [`MapReduceScriptTaskStatus.stage`](#mapreducescripttaskstatusstage). Since 2015.2. The
source does not describe the values individually beyond their names.

| Value |
|---|
| `GET_INPUT` |
| `MAP` |
| `SHUFFLE` |
| `REDUCE` |
| `SUMMARIZE` |

### task.ActionCondition
<!-- id: N/task.ActionCondition -->

Values of [`RecordActionTask.condition`](#recordactiontaskcondition). Since 2019.1.

| Value |
|---|
| `ALL_QUALIFIED_INSTANCES` |

### task.DedupeMode
<!-- id: N/task.DedupeMode -->

Values of [`EntityDeduplicationTask.dedupeMode`](#entitydeduplicationtaskdedupemode). Since 2015.2.
The source does not describe what each mode does beyond its name.

| Value |
|---|
| `MERGE` |
| `DELETE` |
| `MAKE_MASTER_PARENT` |
| `MARK_AS_NOT_DUPES` |

### task.DedupeEntityType
<!-- id: N/task.DedupeEntityType -->

Values of [`EntityDeduplicationTask.entityType`](#entitydeduplicationtaskentitytype). Since 2015.2.

| Value | Note |
|---|---|
| `CUSTOMER` | Also covers prospects and leads |
| `CONTACT` | |
| `VENDOR` | |
| `PARTNER` | |
| `LEAD` | |
| `PROSPECT` | |

### task.MasterSelectionMode
<!-- id: N/task.MasterSelectionMode -->

Values of [`EntityDeduplicationTask.masterSelectionMode`](#entitydeduplicationtaskmasterselectionmode).
Since 2015.2. The source does not describe the modes individually; the only behaviour it states is
that `masterRecordId` is honoured only with `SELECT_BY_ID`.

| Value |
|---|
| `CREATED_EARLIEST` |
| `MOST_RECENT_ACTIVITY` |
| `MOST_POPULATED_FIELDS` |
| `SELECT_BY_ID` |

## Examples

A RESTlet that accepts vendor price rows as CSV text, queues them through a saved import map, and
lets the caller poll for the result. CSV import submission is allowed in RESTlets. The map ID is a
placeholder for one saved in your account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/task', 'N/log'], (task, log) => {
  const post = (body) => {
    const importJob = task.create({
      taskType: task.TaskType.CSV_IMPORT,
      mappingId: 'CUSTIMPORT_vendor_price_refresh',
      importFile: body.csvText,
      name: `Vendor price refresh ${new Date().toISOString().slice(0, 10)}`
    });
    const taskId = importJob.submit();
    log.audit({ title: 'Price import queued', details: taskId });
    return { taskId };
  };

  const get = (requestParams) => {
    const state = task.checkStatus({ taskId: requestParams.taskId });
    const done = state.status === task.TaskStatus.COMPLETE ||
      state.status === task.TaskStatus.FAILED;
    return { taskId: state.taskId, status: state.status, done };
  };

  return { get, post };
});
```

A scheduled script that imports vendor bills whose expense lines live in a second file. Vendor bill
`Expenses` is one of the sublists that must come as a separate CSV file, so it goes into
`linkedFiles`, keyed by the sublist's internal ID. The map ID, folder path and sublist key are
placeholders for values from your own saved map.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/task', 'N/file', 'N/log'], (task, file, log) => {
  const execute = () => {
    const headerRows = file.load({ id: 'Imports/vendor_bills_header.csv' });
    const expenseRows = file.load({ id: 'Imports/vendor_bills_expenses.csv' });

    const billImport = task.create({ taskType: task.TaskType.CSV_IMPORT });
    billImport.mappingId = 'CUSTIMPORT_vendor_bill_with_expenses';
    billImport.importFile = headerRows;
    billImport.linkedFiles = { expense: expenseRows };
    billImport.name = 'Nightly vendor bills';

    const taskId = billImport.submit();
    log.audit({ title: 'Vendor bill import queued', details: taskId });
  };

  return { execute };
});
```

A nightly scheduled script that hands a cutoff date to a map/reduce deployment that closes stale
purchase order lines. Because only one unfinished run per deployment is allowed, the submit is
wrapped so an overlapping night is logged rather than crashing the script.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/task', 'N/runtime', 'N/log'], (task, runtime, log) => {
  const execute = () => {
    const cutoff = runtime.getCurrentScript().getParameter({ name: 'custscript_po_close_cutoff' });

    const closer = task.create({
      taskType: task.TaskType.MAP_REDUCE,
      scriptId: 'customscript_close_stale_po_lines',
      deploymentId: 'customdeploy_close_stale_po_lines',
      params: { custscript_mr_po_cutoff: cutoff }
    });

    let taskId;
    try {
      taskId = closer.submit();
    } catch (e) {
      log.error({ title: 'Map/reduce not queued', details: `${e.name}: ${e.message}` });
      return;
    }

    const state = task.checkStatus({ taskId });
    log.audit({ title: 'PO close run', details: `${taskId} is ${state.status}` });
  };

  return { execute };
});
```

Reporting progress of that map/reduce run to a dashboard. The counter methods cost 10 units each,
so they are only called while the run is actually processing.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/task'], (task) => {
  const get = (requestParams) => {
    const run = task.checkStatus({ taskId: requestParams.taskId });
    const report = { status: run.status, stage: run.stage };

    if (run.status === task.TaskStatus.PROCESSING) {
      report.stagePercent = run.getPercentageCompleted();
      if (run.stage === task.MapReduceStage.MAP) {
        report.mapRowsLeft = run.getPendingMapCount();
      }
    }
    return report;
  };

  return { get };
});
```

Chaining work: export open vendor bills with SuiteQL to a CSV file, then let a map/reduce script
pick the file up when the export is done. Table and column names in SuiteQL come from the
account's Records Catalog; the ones here are illustrative.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/task'], (task) => {
  const queueBillExport = (subsidiaryId) => {
    const exportJob = task.create({
      taskType: task.TaskType.SUITE_QL,
      query: 'SELECT id, tranid, foreigntotal FROM transaction WHERE recordtype = ? AND subsidiary = ?',
      params: ['vendorbill', subsidiaryId],
      filePath: 'Exports/open_vendor_bills.csv'
    });

    exportJob.addInboundDependency({
      taskType: task.TaskType.MAP_REDUCE,
      scriptId: 'customscript_vendor_bill_followup',
      deploymentId: 'customdeploy_vendor_bill_followup'
    });

    return exportJob.submit();
  };

  return { queueBillExport };
});
```
