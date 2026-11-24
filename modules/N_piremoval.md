# N/piremoval
<!-- id: N/piremoval -->

`N/piremoval` scrubs personal information from records, which is how a script can carry out a
"right to be forgotten" request. A removal task can clear three kinds of data: the values of
chosen fields, the system notes that recorded them, and workflow history. It works on entity
records, transactions and custom records.

The life of a task:

1. Build one with [`createTask`](#piremovalcreatetaskoptions), or reopen a saved one with
   [`loadTask`](#piremovalloadtaskoptions).
2. [`save`](#piremovaltasksave) it. Validation happens at this point, and the task gets its ID.
3. [`run`](#piremovaltaskrun) it.
4. Check the outcome with [`getTaskStatus`](#piremovalgettaskstatusoptions).

A task covers a single record type; use separate tasks for different types.

- **Permissions:** creating a removal task requires the permission
  `Remove Personal Information Create`; running one requires `Remove Personal Information Run`. A
  script that both creates and runs tasks needs both. In the generated
  [permissions](../gen/catalogs/permissions.md) catalog these names appear as the entries
  `ADMI_PI_REMOVAL_CREATE` and `ADMI_PI_REMOVAL_RUN`, with their levels.
- **Availability:** server scripts only.
- **Load it as:** `define(['N/piremoval'], (piremoval) => { ... })`
- **Since:** 2019.2

## Members

| Member | Kind | Summary |
|---|---|---|
| [`piremoval.createTask(options)`](#piremovalcreatetaskoptions) | function | Build a new removal task |
| [`piremoval.loadTask(options)`](#piremovalloadtaskoptions) | function | Open a saved task by ID |
| [`piremoval.deleteTask(options)`](#piremovaldeletetaskoptions) | function | Delete a saved task by ID |
| [`piremoval.getTaskStatus(options)`](#piremovalgettaskstatusoptions) | function | Status and log of a task |
| [`PiRemovalTask`](#piremovaltask-object) | object | A removal task |
| [`PiRemovalTaskStatus`](#piremovaltaskstatus-object) | object | Status of a task |
| [`PiRemovalTaskLogItem`](#piremovaltasklogitem-object) | object | One log entry of a task |

## Functions

### piremoval.createTask(options)
<!-- id: N/piremoval.createTask -->

Creates an unsaved removal task that describes which records of one type to clean and what to
remove. No data changes until the task is saved and run. Requires the permission
`Remove Personal Information Create`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.historyOnly` | boolean | No | `false` |
| `options.fieldIds` | number[] | No | |
| `options.historyReplacement` | string | No | |
| `options.recordIds` | number[] | No | |
| `options.recordType` | string | No | |
| `options.workflowIds` | number[] | No | |

- `options.historyOnly`: when `true`, only system notes are cleaned.
- `options.fieldIds`: the fields whose values are removed. It is typed `number[]` here, while the
  matching task property [`fieldIds`](#piremovaltaskfieldids) is typed `string[]`.

> [!SOURCE-ERROR] The source types `options.fieldIds` as a number array, but the task's `fieldIds`
> property as a string array. The spec does not say which is right, so test which form your
> account accepts before relying on either.
<!-- source-error: N/piremoval.createTask | options.fieldIds | fieldIds_number_array_vs_property_string_array -->

- `options.historyReplacement`: text written into system notes in place of the removed values.
- `options.recordIds`: internal IDs of the records to clean, all of `options.recordType`.
- `options.recordType`: the record type the task works on.
- `options.workflowIds`: workflows whose history is cleaned.
- Every option is marked optional, yet `save` fails when no record type is set, so in practice
  supply `recordType`.

**Returns:** `piremoval.PiRemovalTask`, not yet saved.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.2

### piremoval.loadTask(options)
<!-- id: N/piremoval.loadTask -->

Returns an existing, saved task so you can inspect it, run it or delete it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |

- `options.id`: the task's ID, as assigned when it was saved.

**Returns:** `piremoval.PiRemovalTask`, the stored task.

**Errors:**

- `_1_WAS_NOT_FOUND`: no task has that ID. (The code is written as the source has it; the `_1_`
  looks like a slot for a name.)

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.2

### piremoval.deleteTask(options)
<!-- id: N/piremoval.deleteTask -->

Deletes a saved task by its ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |

- `options.id`: the ID of the task to delete.

**Returns:** `void`

**Errors:**

- `UNEXPECTED_ERROR`: the task is in a state that does not allow this, notably when it was never
  saved.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2019.2

### piremoval.getTaskStatus(options)
<!-- id: N/piremoval.getTaskStatus -->

Reports a task's current status together with its log.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |

- `options.id`: the ID of the task to check.

**Returns:** `piremoval.PiRemovalTaskStatus`, the task's status and log.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.2

## Objects

### PiRemovalTask (object)
<!-- id: N/piremoval.PiRemovalTask -->

A personal-information removal job: the record type and record IDs it targets, the fields and
workflows to clean, and the replacement text for system notes. Returned by `createTask` and
`loadTask`. Server scripts, since 2019.2.

#### PiRemovalTask.save()
<!-- id: N/piremoval.PiRemovalTask#save -->

Stores the task and assigns its ID. This is where the task's contents are validated, so problems
with the record type, records, fields or workflows show up here rather than at `run`.

**Returns:** `void`

**Errors:**

- `_1_CANNOT_BE_EMPTY`: no record type has been set.
- `_1_JOB_WAS_NOT_FOUND`: the record type, one of the records, a field or a workflow could not be
  found.

(Codes as the source writes them, with `_1_` where a name seems to belong.)

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2019.2

#### PiRemovalTask.run()
<!-- id: N/piremoval.PiRemovalTask#run -->

Starts the removal. The task must have been saved first; validation already took place at
[`save`](#piremovaltasksave). Requires the permission `Remove Personal Information Run`.

**Returns:** `void`

**Errors:**

- `UNEXPECTED_ERROR`: the task is in the wrong state, for example it has not been saved.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2019.2

#### PiRemovalTask.deleteTask()
<!-- id: N/piremoval.PiRemovalTask#deleteTask -->

Deletes this task.

**Returns:** `void`

**Errors:**

- `UNEXPECTED_ERROR`: the task is in the wrong state, for example it has not been saved.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2019.2

#### PiRemovalTask.fieldIds
<!-- id: N/piremoval.PiRemovalTask#fieldIds -->

`string[]`, read-only. The fields whose values are removed. An empty list means no field values
change; a null or otherwise invalid value raises `WRONG_PARAMETER_TYPE`. Its type differs from the
`number[]` of `createTask`'s `options.fieldIds` (see the note there). Since 2019.2.

#### PiRemovalTask.historyOnly
<!-- id: N/piremoval.PiRemovalTask#historyOnly -->

`boolean`, read-only, default `false`. `true` limits the task to system notes; `false` cleans
system notes, workflow history and field values. Since 2019.2.

#### PiRemovalTask.historyReplacement
<!-- id: N/piremoval.PiRemovalTask#historyReplacement -->

`string`, read-only. The text that replaces removed values inside system notes. Since 2019.2.

#### PiRemovalTask.id
<!-- id: N/piremoval.PiRemovalTask#id -->

`number`, read-only. Assigned when the task is saved; you cannot choose a task ID yourself. Since
2019.2.

#### PiRemovalTask.recordIds
<!-- id: N/piremoval.PiRemovalTask#recordIds -->

`number[]`, read-only. Internal IDs of the records to clean. An empty list changes nothing; a null
or invalid value raises `WRONG_PARAMETER_TYPE`. Since 2019.2.

#### PiRemovalTask.recordType
<!-- id: N/piremoval.PiRemovalTask#recordType -->

`string`, read-only. The record type; every record in the task must be of this type. Since 2019.2.

#### PiRemovalTask.status
<!-- id: N/piremoval.PiRemovalTask#status -->

`piremoval.PiRemovalTaskStatus`. The task's status. The source does not state whether this
property is read-only. Since 2019.2.

#### PiRemovalTask.workflowIds
<!-- id: N/piremoval.PiRemovalTask#workflowIds -->

`number[]`, read-only. Workflows whose history is cleaned. An empty list changes nothing; a null
or invalid value raises `WRONG_PARAMETER_TYPE`. Since 2019.2.

### PiRemovalTaskStatus (object)
<!-- id: N/piremoval.PiRemovalTaskStatus -->

The state of a task as returned by `getTaskStatus`: an overall status plus the task's log. The
status values are `PENDING`, `PROCESSING`, `COMPLETE` and `FAILED`, the same set as
[`task.TaskStatus`](N_task.md). Server scripts, since 2019.2.

#### PiRemovalTaskStatus.logList
<!-- id: N/piremoval.PiRemovalTaskStatus#logList -->

`piremoval.PiRemovalTaskLogItem[]`. The task's [log entries](#piremovaltasklogitem-object). Read
them with an ordinary index loop from 0 up to `logList.length`. The source does not state whether
the property is read-only. Since 2019.2.

#### PiRemovalTaskStatus.status
<!-- id: N/piremoval.PiRemovalTaskStatus#status -->

`string`, read-only. One of `PENDING`, `PROCESSING`, `COMPLETE` or `FAILED` (see
[`task.TaskStatus`](N_task.md)). Since 2019.2.

### PiRemovalTaskLogItem (object)
<!-- id: N/piremoval.PiRemovalTaskLogItem -->

One entry in a task's log. Entries are written when the task is created, when it starts and when
it completes, and they are ordered by date. Found in `PiRemovalTaskStatus.logList`. Server
scripts, since 2019.2.

#### PiRemovalTaskLogItem.exception
<!-- id: N/piremoval.PiRemovalTaskLogItem#exception -->

`string`, read-only. The message of an unexpected error, when one occurred. Since 2019.2.

#### PiRemovalTaskLogItem.message
<!-- id: N/piremoval.PiRemovalTaskLogItem#message -->

`string`, read-only. Reports problems such as a record type that was not set or IDs that could not
be found. Since 2019.2.

#### PiRemovalTaskLogItem.status
<!-- id: N/piremoval.PiRemovalTaskLogItem#status -->

`string`, read-only. One of `PENDING`, `PROCESSING`, `COMPLETE` or `FAILED` (see
[`task.TaskStatus`](N_task.md)). Since 2019.2.

#### PiRemovalTaskLogItem.type
<!-- id: N/piremoval.PiRemovalTaskLogItem#type -->

`string`, read-only. What the entry concerns: `FieldValue`, `SystemNote` or `Workflow`. Since
2019.2.

## Examples

A Suitelet back end for a privacy team: given a contact ID, it clears the contact's email and
phone numbers, replaces them in system notes with a fixed marker, and runs the task. The script's
role needs both `Remove Personal Information Create` and `Remove Personal Information Run`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/piremoval'], (piremoval) => {
  const onRequest = (context) => {
    const contactId = Number(context.request.parameters.contact);
    if (!contactId) {
      context.response.write({ output: 'Missing contact parameter.' });
      return;
    }

    const removal = piremoval.createTask({
      recordType: 'contact',
      recordIds: [contactId],
      // options.fieldIds is typed number[] but the task property string[]; verify the form
      fieldIds: ['email', 'phone', 'mobilephone'],
      historyReplacement: '[removed on request]'
    });
    removal.save();
    removal.run();

    context.response.write({ output: `Removal task ${removal.id} started.` });
  };

  return { onRequest };
});
```

A scheduled follow-up that checks a task whose ID arrives as a script parameter and logs any
failed steps from its log:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/piremoval', 'N/runtime', 'N/log'], (piremoval, runtime, log) => {
  const execute = () => {
    const taskId = Number(runtime.getCurrentScript().getParameter({ name: 'custscript_pi_task_id' }));
    const state = piremoval.getTaskStatus({ id: taskId });
    if (state.status !== 'COMPLETE') {
      log.audit({ title: `PI task ${taskId}`, details: `Still ${state.status}` });
    }
    const entries = state.logList;
    for (let i = 0; i < entries.length; i += 1) {
      const entry = entries[i];
      if (entry.status === 'FAILED') {
        log.error({ title: `${entry.type} step failed`, details: entry.exception || entry.message });
      }
    }
  };

  return { execute };
});
```
