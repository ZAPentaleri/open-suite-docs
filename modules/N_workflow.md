# N/workflow
<!-- id: N/workflow -->

`N/workflow` lets a server script drive SuiteFlow workflows: start a new workflow instance on a
record (`initiate`), or make a running instance re-evaluate its current state, optionally as if a
workflow button had been clicked (`trigger`).

- **Availability:** server scripts only.
- **Permission:** the spec lists the Workflow permission as a requirement for using the module.
- **Since:** the source does not state a version for the module itself; both functions date from
  2015.2.
- **Load it as:** `define(['N/workflow'], (workflow) => { ... })`
- Related: [SuiteFlow](../topics/suiteflow.md), [N/task](N_task.md) (`task.WorkflowTriggerTask` for
  starting workflows asynchronously), [N/record](N_record.md) (`record.Type`).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`workflow.initiate(options)`](#workflowinitiateoptions) | function | Start a workflow instance on a record |
| [`workflow.trigger(options)`](#workflowtriggeroptions) | function | Re-evaluate a running instance, or "click" one of its buttons |

## Functions

### workflow.initiate(options)
<!-- id: N/workflow.initiate -->

Starts a new instance of a workflow on one record and returns the instance ID. It has the same
effect as the Initiate Workflow action inside a workflow. The workflow must already be defined in
the account. If you would rather start workflows in the background, queue a
[`task.WorkflowTriggerTask`](N_task.md) instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.defaultValues` | Object | No | |
| `options.recordType` | string | Yes | |
| `options.workflowId` | string \| number | Yes | |
| `options.recordId` | string \| number | Yes | |

- `options.defaultValues`: starting values for fields on the workflow definition and for custom
  fields defined on the workflow or its states. Since 2015.2.
- `options.recordType`: the record's type, a [`record.Type`](N_record.md) value. Since 2015.2.
- `options.workflowId`: the workflow's numeric internal ID or its custom script ID. Since 2015.2.
- `options.recordId`: internal ID of the record to run the workflow on. Since 2015.2.

**Returns:** `number`, the ID of the new workflow instance.

**Errors:** the source does not state which errors this function raises.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

### workflow.trigger(options)
<!-- id: N/workflow.trigger -->

Makes a workflow instance evaluate the actions and transitions of its current state now. If you pass
an `actionId`, the call acts like a click on that workflow button. The workflow ID and record type
you need are shown in the ID and Record Type fields of the workflow definition page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.actionId` | string \| number | No | |
| `options.recordType` | string | Yes | |
| `options.stateId` | string \| number | No | |
| `options.workflowId` | string \| number | Yes | |
| `options.recordId` | string | Yes | |
| `options.workflowInstanceId` | string \| number | No | |

- `options.actionId`: the button action to "press"; passing it behaves like a button click.
  Since 2015.2.
- `options.recordType`: a [`record.Type`](N_record.md) value. Since 2015.2.
- `options.stateId`: the state, by internal ID or script ID. Since 2015.2.
- `options.workflowId`: the workflow, by internal ID or script ID. Since 2015.2.
- `options.recordId`: internal ID of the record, typed `string` here. Since 2015.2.

> [!SOURCE-ERROR] The source types `options.recordId` as a string for `trigger`, while
> `workflow.initiate` accepts a string or a number for the same value. The spec does not say
> whether `trigger` really rejects numbers; passing the ID as a string is the safe choice.
<!-- source-error: N/workflow.trigger | options.recordId | recordid_typed_string_here_string_or_number_in_initiate -->

- `options.workflowInstanceId`: a specific running instance. Since 2015.2.

**Returns:** `number`, the workflow instance ID.

**Errors:** the source does not state which errors this function raises.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

## Examples

A user event script that starts a credit-review workflow on a new customer only when the requested
credit limit is high, passing a starting value for a workflow field. The workflow script ID and its
`custworkflow_review_reason` field are invented for the example.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/workflow', 'N/record'], (workflow, record) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const limit = Number(context.newRecord.getValue({ fieldId: 'creditlimit' })) || 0;
    if (limit < 50000) {
      return;
    }
    workflow.initiate({
      recordType: record.Type.CUSTOMER,
      recordId: context.newRecord.id,
      workflowId: 'customworkflow_credit_review',
      defaultValues: { custworkflow_review_reason: `Requested limit ${limit}` }
    });
  };

  return { afterSubmit };
});
```

A RESTlet that lets an external approval tool press a workflow button on an expense report. The
workflow and action IDs are invented for the example.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/workflow', 'N/record'], (workflow, record) => {
  const post = (body) => {
    const instanceId = workflow.trigger({
      recordType: record.Type.EXPENSE_REPORT,
      recordId: String(body.reportId),
      workflowId: 'customworkflow_expense_approval',
      actionId: body.decision === 'approve' ? 'workflowaction_approve' : 'workflowaction_reject'
    });
    return { instanceId };
  };

  return { post };
});
```
