# WorkflowActionScript
<!-- id: script:WorkflowActionScript -->

A workflow action script is server-side code that a SuiteFlow workflow runs as a custom action. You
reach for it when the built-in workflow actions are not enough: most often when the logic has to
work with sublists (the Workflow Manager cannot get at sublist fields), or when the rule needs more
logic than the standard actions offer. The script can hand a value back, which the workflow stores
in one of its fields and can use in later conditions.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `WorkflowActionScript` |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server |
| Execution context reported by [`runtime.executionContext`](../modules/N_runtime.md) | workflow |
| SuiteScript 1.0 counterpart | Workflow Action Scripts ([1.0 concepts](../topics/ss1_api.workflow_action.md)) |
| Entry points | [`onAction`](#onactioncontext) |

Related pages: [SuiteFlow](../topics/suiteflow.md), [workflow actions](../topics/suiteflow.actions.md),
[SDF object](../topics/sdf.workflowactionscript.md),
[SDF object elements](../gen/catalogs/sdf_workflowactionscript_object.md),
[script parameters](../topics/script_parameters.md), [N/workflow](../modules/N_workflow.md),
[N/runtime](../modules/N_runtime.md), [N/record](../modules/N_record.md).

## How it runs

**Creating it.** Create the script record from the new-script page (Customization > Scripting >
Scripts > New).

**Generic actions for every workflow.** Deploy the script with Applies To set to `All Records` to get
a generic custom action that is offered in all workflows, whatever their record type. `All Records`
must then be the only choice: selecting it together with another record type produces a deployment
error. A typical generic action is one driven by parameters, for example a parameterized "set sales
rep" action that each workflow configures for its own case.

**Parameters.** The parameters the action accepts are defined as script parameters, and the
workflow supplies their values in the custom action's details in the Workflow Manager. Inside the
script, read them through [`N/runtime`](../modules/N_runtime.md) as deployment script parameters
(`runtime.getCurrentScript().getParameter(...)`).

**The current record.** The context already gives you the workflow's record, so you normally do not
need anything else to find it. The record type and record ID are still sent as parameters as well.

**Returning a value.** A workflow action script can return a value that is saved in a workflow field
or state field of the same type. To set this up:

1. On the script record's Parameters subtab, set the return type.
2. In the workflow, create a field with a matching type.
3. Add the custom action to a state.
4. In the action, choose that field under Store Result In.

Examples of what that enables: return 1 if some item is present on the transaction and 0 if not, or
report whether every line has a quantity of at least 1, then branch on the stored value.

**Managing scripts.** Workflow action scripts can be managed with the SuiteCloud Development
Framework, copied between accounts with Copy to Account, and reviewed with SuiteScript Analysis.

## Entry points

### onAction(context)
<!-- id: script:WorkflowActionScript.onAction -->

The point where the workflow hands control to your script when it reaches the custom action.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.newRecord` | record.Record | - | |
| `context.oldRecord` | record.Record | - | |
| `context.form` | serverWidget.Form | - | |
| `context.type` | string | - | |
| `context.workflowId` | integer | - | |

`newRecord` and `oldRecord` exist since 2016.1; `form`, `type` and `workflowId` since 2016.2.

- `context.newRecord`: the record in its new state. You cannot call
  [`Record.save`](../modules/N_record.md) on it.
- `context.oldRecord`: the record as it was before. It cannot be saved either.
- `context.form`: the form being displayed. It is only available when the action runs on before
  load, so check for it before use.
- `context.type`: the event type that triggered the workflow, such as create, edit, view or delete.
- `context.workflowId`: the internal ID of the workflow that called the script.

**Returns:** `void` according to the entry point's documented signature. As described above, the
function can still return a value that the workflow stores, provided a return type is set on the
script record.

> [!SOURCE-ERROR] The source declares the return type of `onAction` as `void` while also describing a
> return value that the workflow stores in a field. Rely on the documented return-value setup (return
> type on the Parameters subtab plus Store Result In) when you need a result, and verify it in your
> account.
<!-- source-error: script:WorkflowActionScript.onAction | - | onaction_returns_void_but_return_value_stored_in_field -->

**Errors:** the source does not state any errors for this entry point. The only documented error in
this area is the deployment error raised when `All Records` is combined with another record type.

**Governance:** the execution is limited to 1,000 usage units.

## Examples

Count the backordered lines on a sales order and return the number, so the workflow can store it in an
integer workflow field and route the order to a fulfilment review state when it is above zero:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType WorkflowActionScript
 */
define(['N/log'], (log) => {
  const onAction = (context) => {
    const order = context.newRecord;
    const lines = order.getLineCount({ sublistId: 'item' });
    let backordered = 0;
    for (let line = 0; line < lines; line += 1) {
      const short = Number(order.getSublistValue({ sublistId: 'item', fieldId: 'quantitybackordered', line })) || 0;
      if (short > 0) {
        backordered += 1;
      }
    }
    log.debug({ title: `Workflow ${context.workflowId}`, details: `${backordered} of ${lines} lines backordered` });
    return backordered;
  };

  return { onAction };
});
```

A generic action (deployed to All Records) that hides a field named by a script parameter, for use in
a workflow's before-load state. The form only exists on before load, so the script checks for it. The
parameter ID is invented:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType WorkflowActionScript
 */
define(['N/runtime', 'N/ui/serverWidget'], (runtime, serverWidget) => {
  const onAction = (context) => {
    if (!context.form) {
      return;
    }
    const fieldId = runtime.getCurrentScript().getParameter({ name: 'custscript_wf_field_to_hide' });
    const target = fieldId ? context.form.getField({ id: fieldId }) : null;
    if (target) {
      target.updateDisplayType({ displayType: serverWidget.FieldDisplayType.HIDDEN });
    }
  };

  return { onAction };
});
```
