# SuiteScript 1.0 workflow action scripts
<!-- id: topic:ss1_api.workflow_action -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is
> [WorkflowActionScript](../script-types/WorkflowActionScript.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

A workflow action script provides a custom action that a workflow can call.

## Deployment

- A workflow action script can be deployed to all records, which makes it a generic custom action
  usable by workflows on any record type.
- Deploying it to all records **and** to a specific record type at the same time is an error.

## Entry function arguments

| Position | Argument | What it holds |
|---|---|---|
| 1 | `id` | An integer ID (in 2.1 this maps to the workflow ID) |
| 2 | `form` | The [`nlobjForm`](../ss1/objects-forms.md), only when the action runs in `beforeLoad` |
| 3 | `type` | The event type |

## Running

- While the script runs, the execution context reports `workflow`.
- Read the record the workflow is processing with [`nlapiGetNewRecord`](../ss1/records.md).
- The function may return a value, which the workflow can store in one of its fields.

## Converting

In 2.1 the function becomes the `onAction` entry point of
[WorkflowActionScript](../script-types/WorkflowActionScript.md): `id` becomes `workflowId`, and
`form` and `type` come from the context object, which also carries the new and old records. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

An action deployed to opportunities that returns the number of days left until the expected close
date, so the workflow can store it in a field and branch on it.

```js
function daysToExpectedClose(id, form, type) {
  var rec = nlapiGetNewRecord();
  var closeText = rec.getFieldValue('expectedclosedate');
  if (!closeText) {
    return -1;
  }
  var closeDate = nlapiStringToDate(closeText);
  var msPerDay = 24 * 60 * 60 * 1000;
  return Math.ceil((closeDate.getTime() - new Date().getTime()) / msPerDay);
}
```
