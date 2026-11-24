# N/task/accounting/recognition
<!-- id: N/task/accounting/recognition -->

`N/task/accounting/recognition` merges revenue records in revenue management. It has two jobs:

- combine several revenue arrangements into one new arrangement;
- combine individual revenue elements into a new arrangement.

Merge tasks are processed asynchronously. The steps are:

1. Create a task object with [`recognition.create`](#recognitioncreateoptions).
2. Set its properties.
3. Call `submit()` to queue the task. It returns a task ID.
4. Check on it with [`recognition.checkStatus`](#recognitioncheckstatusoptions), passing that ID.

The status object reports one of four states: pending, processing, complete or failed. When the
task completes it carries the ID of the merged arrangement; when it fails it carries an error
message.

- **Availability:** server scripts only.
- **Required feature:** `Advanced Revenue Management` must be enabled.
- **Permission:** the spec lists the Revenue Arrangement permission for the module, at the Create
  level or higher.
- **Limit:** one merge can include at most 10,000 revenue elements. Above that an error is thrown;
  the source does not state the error code.
- **Defaults across both task types:** `revenueArrangementDate` and `contractCostAccrualDate`
  default to today. `mergeResidualRevenueAmounts` and `recalculateResidualFairValue` default to
  `false`. Recalculating residual fair value is allowed only together with a prospective merge.
- **Contract cost properties** (`contractAcquisitionExpenseAccount`,
  `contractAcquisitionDeferredExpenseAccount`, `contractCostAccrualDate`) need the
  `Enable Advanced Cost Amortization` preference turned on. When the two account properties are
  left unset, their values come from the `Contract Acquisition Expense Account` and
  `Contract Acquisition Deferred Expense Account` preferences respectively.
- **Since:** the source does not state a version for the module itself; its members date from
  2019.2.
- **Load it as:** `define(['N/task/accounting/recognition'], (recognition) => { ... })`
- Related: [N/task](N_task.md), the [revenue arrangement record](../gen/records/revenuearrangement.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`recognition.create(options)`](#recognitioncreateoptions) | function | Create an unsubmitted merge task |
| [`recognition.checkStatus(options)`](#recognitioncheckstatusoptions) | function | Get the status of a submitted merge task |
| [`MergeArrangementsTask`](#mergearrangementstask-object) | object | Merge whole revenue arrangements |
| [`MergeElementsTask`](#mergeelementstask-object) | object | Merge individual revenue elements |
| [`MergeArrangementsTaskStatus`](#mergearrangementstaskstatus-object) | object | Status of either kind of merge task |
| [`recognition.TaskType`](#recognitiontasktype) | enum | Which kind of merge task to create |
| [`recognition.TaskStatus`](#recognitiontaskstatus) | enum | States a merge task can be in |

## Functions

### recognition.create(options)
<!-- id: N/task/accounting/recognition.create -->

Creates a merge task of the requested type. Nothing runs until you set the task's properties and
call its `submit()` method.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskType` | string | Yes | |

- `options.taskType`: a [`recognition.TaskType`](#recognitiontasktype) value. Anything else raises
  `INVALID_TASK_TYPE`.

**Returns:** `recognition.MergeArrangementsTask | recognition.MergeElementsTask`, depending on
the task type: a [`MergeArrangementsTask`](#mergearrangementstask-object) or a
[`MergeElementsTask`](#mergeelementstask-object).

**Errors:**

- `INVALID_TASK_TYPE`: `options.taskType` is not a valid task type.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.2

### recognition.checkStatus(options)
<!-- id: N/task/accounting/recognition.checkStatus -->

Looks up the current state of a merge task that has already been submitted.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskId` | number \| string | Yes | |

- `options.taskId`: the ID returned by the task's `submit()` method.

> [!SOURCE-ERROR] The source says this task ID is assigned by `create`. Everywhere else the spec
> says the ID comes from `submit()`, and `create` only returns an unsubmitted task object. Pass the
> value that `submit()` returned.
<!-- source-error: N/task/accounting/recognition.checkStatus | options.taskId | says_task_id_assigned_by_create -->

**Returns:** [`recognition.MergeArrangementsTaskStatus`](#mergearrangementstaskstatus-object). It is
used for both arrangement merges and element merges.

**Errors:** the source does not state any errors for this function (for example, for an unknown
task ID).

**Governance:** 50 units.

**Availability:** server scripts.

**Since:** 2019.2

## Objects

### MergeArrangementsTask (object)
<!-- id: N/task/accounting/recognition.MergeArrangementsTask -->

A task that merges existing revenue arrangements into a new one. Get it from
[`recognition.create`](#recognitioncreateoptions) with `TaskType.MERGE_ARRANGEMENTS_TASK`. Only
`arrangements` must be set. Server scripts; since 2019.2.

#### MergeArrangementsTask.arrangements
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#arrangements -->

`Array<number | string>`. **Required.** Internal IDs of the revenue arrangements to merge. IDs that
are not valid are ignored without an error. If no IDs are present when you submit,
`NO_REVENUE_ARRANGEMENT_IDS_ARE_INCLUDED_IN_YOUR_INPUT` is raised. Submitting also fails if no
revenue elements are found for the IDs you gave. Since 2019.2.

#### MergeArrangementsTask.contractAcquisitionExpenseAccount
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#contractAcquisitionExpenseAccount -->

`number | string`. The contract acquisition expense account for the merged arrangement. Leave it
unset and the account in the `Contract Acquisition Expense Account` preference is used. It needs
the `Enable Advanced Cost Amortization` preference; when advanced cost amortization is off, the
value is ignored. Since 2019.2.

#### MergeArrangementsTask.contractAcquisitionDeferredExpenseAccount
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#contractAcquisitionDeferredExpenseAccount -->

`number | string`. The contract acquisition deferred expense account. Leave it unset and the
account in the `Contract Acquisition Deferred Expense Account` preference is used. It needs the
`Enable Advanced Cost Amortization` preference; when advanced cost amortization is off, the value
is ignored. Since 2019.2.

#### MergeArrangementsTask.contractCostAccrualDate
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#contractCostAccrualDate -->

`Date`. The contract cost accrual date. Defaults to today. It needs the
`Enable Advanced Cost Amortization` preference and is ignored when advanced cost amortization is
off. A value in an invalid date format makes `submit()` raise `WRONG_PARAMETER_TYPE`. Since 2019.2.

#### MergeArrangementsTask.mergeResidualRevenueAmounts
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#mergeResidualRevenueAmounts -->

`boolean`. Set it to `true` for a prospective merge. Defaults to `false`. Since 2019.2.

#### MergeArrangementsTask.recalculateResidualFairValue
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#recalculateResidualFairValue -->

`boolean`. Set it to `true` to recalculate the residual fair value during the merge. Defaults to
`false`. It can only be used when
[`mergeResidualRevenueAmounts`](#mergearrangementstaskmergeresidualrevenueamounts) is also `true`.
Since 2019.2.

#### MergeArrangementsTask.revenueArrangementDate
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#revenueArrangementDate -->

`Date`. The date of the new, merged arrangement. Defaults to today. A value in an invalid date
format makes `submit()` raise `WRONG_PARAMETER_TYPE`. Since 2019.2.

#### MergeArrangementsTask.submit()
<!-- id: N/task/accounting/recognition.MergeArrangementsTask#submit -->

Queues the merge for asynchronous processing. Set every property you need before calling it. The
returned task ID is the same as the submission ID of the bulk process. Pass it to
[`recognition.checkStatus`](#recognitioncheckstatusoptions).

**Returns:** `number`, the task ID.

**Errors:**

- `NO_REVENUE_ARRANGEMENT_IDS_ARE_INCLUDED_IN_YOUR_INPUT`: `arrangements` is empty or was never
  set.
- `NO_REVENUE_ELEMENTS_WERE_FOUND_FOR_THE_REVENUE_ARRANGEMENT_IDS_YOU_INPUT`: the arrangement IDs
  given do not lead to any revenue elements.
- `WRONG_PARAMETER_TYPE`: a date property (`revenueArrangementDate`, `contractCostAccrualDate`)
  is in an invalid format.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2019.2

### MergeElementsTask (object)
<!-- id: N/task/accounting/recognition.MergeElementsTask -->

A task that merges individual revenue elements into a new arrangement. Get it from
[`recognition.create`](#recognitioncreateoptions) with `TaskType.MERGE_ELEMENTS_TASK`. Only
`elements` must be set. Server scripts; since 2019.2.

#### MergeElementsTask.elements
<!-- id: N/task/accounting/recognition.MergeElementsTask#elements -->

`Array<number | string>`. **Required.** Internal IDs of the revenue elements to merge. The source
does not state what happens when this list is empty or holds invalid IDs. Since 2019.2.

#### MergeElementsTask.contractAcquisitionExpenseAccount
<!-- id: N/task/accounting/recognition.MergeElementsTask#contractAcquisitionExpenseAccount -->

`number | string`. The contract acquisition expense account. Leave it unset and the account in the
`Contract Acquisition Expense Account` preference is used. It needs the
`Enable Advanced Cost Amortization` preference; when advanced cost amortization is off, the value
is ignored. Since 2019.2.

#### MergeElementsTask.contractAcquisitionDeferredExpenseAccount
<!-- id: N/task/accounting/recognition.MergeElementsTask#contractAcquisitionDeferredExpenseAccount -->

`number | string`. The contract acquisition deferred expense account. Leave it unset and the
account in the `Contract Acquisition Deferred Expense Account` preference is used. It needs the
`Enable Advanced Cost Amortization` preference; when advanced cost amortization is off, the value
is ignored. Since 2019.2.

#### MergeElementsTask.contractCostAccrualDate
<!-- id: N/task/accounting/recognition.MergeElementsTask#contractCostAccrualDate -->

`Date`. The contract cost accrual date. Defaults to today. It needs the
`Enable Advanced Cost Amortization` preference. Unlike the same property on
`MergeArrangementsTask`, the source does not say here whether the value is ignored when advanced
cost amortization is off. An invalid date format makes `submit()` raise `WRONG_PARAMETER_TYPE`.
Since 2019.2.

#### MergeElementsTask.revenueArrangementDate
<!-- id: N/task/accounting/recognition.MergeElementsTask#revenueArrangementDate -->

`Date`. The date of the new arrangement. Defaults to today. An invalid date format makes
`submit()` raise `WRONG_PARAMETER_TYPE`. Since 2019.2.

#### MergeElementsTask.submit()
<!-- id: N/task/accounting/recognition.MergeElementsTask#submit -->

Queues the element merge for asynchronous processing. The returned task ID is the same as the bulk
process submission ID. Use it with [`recognition.checkStatus`](#recognitioncheckstatusoptions).

**Returns:** `number`, the task ID.

**Errors:**

- `WRONG_PARAMETER_TYPE`: a date property is in an invalid format, or the input is otherwise
  malformed.
- The source does not state an error for an empty `elements` list, unlike the arrangement merge.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2019.2

### MergeArrangementsTaskStatus (object)
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus -->

What [`recognition.checkStatus`](#recognitioncheckstatusoptions) returns. Despite its name it
describes both arrangement merges and element merges. Its `status` takes a
[`recognition.TaskStatus`](#recognitiontaskstatus) value. All properties are read-only. Server
scripts; since 2019.2.

#### MergeArrangementsTaskStatus.status
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#status -->

`string`, read-only. The current state, as a [`recognition.TaskStatus`](#recognitiontaskstatus)
value. Since 2019.2.

#### MergeArrangementsTaskStatus.resultingArrangement
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#resultingArrangement -->

`number`, read-only. The internal ID of the new revenue arrangement the merge produced. It applies
once the status is complete. Since 2019.2.

#### MergeArrangementsTaskStatus.errorMessage
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#errorMessage -->

`string`, read-only. Why the merge failed. It applies when the status is failed. Since 2019.2.

#### MergeArrangementsTaskStatus.inputArrangements
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#inputArrangements -->

`number[]`, read-only. The arrangement IDs that went into the task. It applies to arrangement
merges. The source notes that reading it may cost 10 usage units; it does not say when that cost
applies, so budget for it. Since 2019.2.

#### MergeArrangementsTaskStatus.inputElements
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#inputElements -->

`number[]`, read-only. The element IDs that went into the task. It applies to element merges.
Since 2019.2.

#### MergeArrangementsTaskStatus.submissionId
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#submissionId -->

`number`, read-only. The same value as the task ID that `submit()` returned (for either task
type). Since 2019.2.

#### MergeArrangementsTaskStatus.taskId
<!-- id: N/task/accounting/recognition.MergeArrangementsTaskStatus#taskId -->

`number | string`, read-only. The task's ID, assigned when the task was submitted with `submit()`
on a `MergeArrangementsTask` or `MergeElementsTask`. Since 2019.2.

## Enums

### recognition.TaskType
<!-- id: N/task/accounting/recognition.TaskType -->

Selects the kind of task for `options.taskType` of [`recognition.create`](#recognitioncreateoptions).
The source lists only the enum names, not the underlying string values, so always use the enum
rather than a literal. Since 2019.2.

| Value | Creates |
|---|---|
| `MERGE_ARRANGEMENTS_TASK` | a [`MergeArrangementsTask`](#mergearrangementstask-object) |
| `MERGE_ELEMENTS_TASK` | a [`MergeElementsTask`](#mergeelementstask-object) |

### recognition.TaskStatus
<!-- id: N/task/accounting/recognition.TaskStatus -->

States reported in [`MergeArrangementsTaskStatus.status`](#mergearrangementstaskstatusstatus).
The source lists only the enum names, not their string values. Since 2019.2.

| Value | What the source ties to it |
|---|---|
| `PENDING` | Not described beyond its name |
| `PROCESSING` | Not described beyond its name |
| `COMPLETE` | `resultingArrangement` holds the new arrangement ID |
| `FAILED` | `errorMessage` explains why |

## Examples

A scheduled script that consolidates the arrangements of a renewed contract into one, merging
residual amounts prospectively and dating the new arrangement on the first of the month. The
arrangement IDs come from a script parameter (a comma-separated list). The parameter ID is an
invented example.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/task/accounting/recognition', 'N/runtime', 'N/log'], (recognition, runtime, log) => {
  const execute = () => {
    const raw = runtime.getCurrentScript().getParameter({ name: 'custscript_rr_merge_ids' }) || '';
    const ids = String(raw).split(',').map((s) => s.trim()).filter((s) => s.length > 0);
    if (ids.length === 0) {
      log.audit({ title: 'Nothing to merge', details: 'Parameter was empty' });
      return;
    }

    const now = new Date();
    const mergeTask = recognition.create({
      taskType: recognition.TaskType.MERGE_ARRANGEMENTS_TASK
    });
    mergeTask.arrangements = ids;
    mergeTask.revenueArrangementDate = new Date(now.getFullYear(), now.getMonth(), 1);
    mergeTask.mergeResidualRevenueAmounts = true;
    mergeTask.recalculateResidualFairValue = true; // only valid with a prospective merge

    const taskId = mergeTask.submit();
    log.audit({ title: 'Arrangement merge queued', details: `Task ${taskId} for ${ids.length} arrangements` });
  };

  return { execute };
});
```

A RESTlet with two verbs. POST queues an element merge, and GET reports progress for a task ID so a
caller can poll until the merge finishes.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/task/accounting/recognition'], (recognition) => {
  const post = (body) => {
    const elementTask = recognition.create({
      taskType: recognition.TaskType.MERGE_ELEMENTS_TASK
    });
    elementTask.elements = body.elementIds;
    return { taskId: elementTask.submit() };
  };

  const get = (params) => {
    const st = recognition.checkStatus({ taskId: params.taskId });
    switch (st.status) {
      case recognition.TaskStatus.COMPLETE:
        return { done: true, newArrangement: st.resultingArrangement };
      case recognition.TaskStatus.FAILED:
        return { done: true, error: st.errorMessage };
      default:
        return { done: false, status: st.status };
    }
  };

  return { post, get };
});
```
