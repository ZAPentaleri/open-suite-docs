# SuiteScript FAQ
<!-- id: topic:suitescript_faq -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [N/task](../modules/N_task.md), [N/record](../modules/N_record.md),
[N/log](../modules/N_log.md), [N/ui/message](../modules/N_ui_message.md),
[N/ui/dialog](../modules/N_ui_dialog.md).

Short answers to recurring questions. The source sorts them into six sections (general, SDF and
SuiteApps, code, tools, errors, and logs); this page regroups them by theme.

## SuiteScript 1.0 versus 2.x

- **1.0** is a set of global functions, and its calls in client scripts are synchronous.
- **2.x** is organized into modules and can also work asynchronously.
- New capabilities are only added to 2.x.

> [!SOURCE-ERROR] The FAQ says SuiteScript 2.1 supports ECMAScript 6, while the source's overview of
> 2.1 names ECMAScript 2023. The spec does not settle which is current; check the language level your
> account's 2.1 runtime supports before relying on newer syntax.
<!-- source-error: topic:suitescript_faq | - | faq_ss21_ecmascript_6_vs_overview_ecmascript_2023 -->

## Script design questions

- **In what order do library files load?** In the order they are listed on the script record's
  Libraries subtab. If an earlier library uses something defined in a later one, you get a reference
  error, so order them so that each library depends only on those loaded before it.
- **Standard or dynamic mode?** Standard mode for reading. Dynamic mode when you write values that
  depend on one another (for example fields whose values drive sourcing of other fields).
- **Can one user event script trigger another?** No, that is not supported.
- **How do I set the execution order of several scripts on a record?** On the Scripted Records page,
  drag the scripts into the order you want. Keep in mind that many scripts on one record make it slow.
- **How do I run server-side logic from a button?** Add the button in `beforeLoad` and give it a
  client-side function that redirects to a Suitelet doing the work.
- **Can a script set a script parameter's value?** No. The only way to supply values is through the
  parameters you pass to [`task.create`](../modules/N_task.md) when you queue a task.
- **Is there a mass delete?** No. Delete the records one at a time with
  [`record.delete`](../modules/N_record.md).
- **What does an outbound HTTPS call need?** The endpoint's certificate authority must be on Mozilla's
  list of included CA certificates, and the server must send the full certificate chain.
- **Where are [N/ui/message](../modules/N_ui_message.md) and [N/ui/dialog](../modules/N_ui_dialog.md)
  available?** Only in client scripts.
- **Record-level or form-level client script?** A record-level script runs for everyone who works
  with the record; a form-level script runs only on the form it is attached to.
- **Which context does a system note record?** The scope that is second most deeply nested. For
  example, with a chain of UI, then client script, then Suitelet, then RESTlet, then user event, the
  system note shows RESTlet.
- **How do I save values that are not on the form?** In `beforeSubmit`, change the new record; values
  the UI does not show are saved with it.

> [!SOURCE-ERROR] The FAQ's sample for passing parameters to a task omits the `taskType` key in its
> call to `task.create`, although the task type is required. Always pass `taskType`, as in the
> example at the end of this page.
<!-- source-error: topic:suitescript_faq | - | task_create_sample_omits_tasktype_key -->

## Errors

| Error | Usual cause |
|---|---|
| Record has been changed | Another user or a script changed the record at the same time |
| You do not have privileges to view this page | Select All is not checked on the deployment's audience, so the user is outside it |
| You are not allowed to navigate directly to this page | The Suitelet's deployment status is not Released |
| `SSS_USAGE_LIMIT_EXCEEDED` | The script used more usage units than it is allowed |
| `SSS_REQUEST_LIMIT_EXCEEDED` | The script ran too long |
| `SSS_INVALID_SUBLIST_OPERATION` | The line does not exist, or the sublist cannot be edited |

The first three rows are the messages users see; the source identifies them by their meaning rather
than by an error code.

> [!SOURCE-ERROR] The source's answer about the usage limit is garbled: one phrase in it is repeated,
> so its wording cannot be relied on. Go by the meaning given in the table (usage units exceeded) and
> see [SuiteScript Governance and Limits](governance.md) for the actual limits.
<!-- source-error: topic:suitescript_faq | - | usage_limit_answer_garbled_ii_happens_twice -->

## Map/Reduce

A Map/Reduce script must have a `getInputData` stage plus at least one of `map` or `reduce`.

## Logs

- Deployment logs share database storage and are purged once that storage limit is reached. How long
  they last therefore varies: it may be about a week or more than 30 days.
- If you need log data later, also store it in custom records.
- The Script Execution Logs page keeps 30 days of entries and shows at most 10,000 results. You can
  filter it by date, script and log level, but you cannot search it fully or customize it.
- Client scripts can write log entries too, through [N/log](../modules/N_log.md).

## Example

Because script parameter values cannot be set from code, they are passed when the task is queued.
Here a user event on sales orders hands the new order's ID to a scheduled credit review.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/task', 'N/log'], (task, log) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const followUp = task.create({
      taskType: task.TaskType.SCHEDULED_SCRIPT,
      scriptId: 'customscript_so_credit_review',
      deploymentId: 'customdeploy_so_credit_review',
      params: { custscript_review_order_id: context.newRecord.id }
    });
    const taskId = followUp.submit();
    log.audit({ title: 'Credit review queued', details: `Order ${context.newRecord.id}, task ${taskId}` });
  };

  return { afterSubmit };
});
```
