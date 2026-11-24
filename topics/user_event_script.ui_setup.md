# User event script setup in the UI
<!-- id: topic:user_event_script.ui_setup -->

See also: [UserEventScript](../script-types/UserEventScript.md),
[Script deployment record (`scriptdeployment`)](../gen/records/scriptdeployment.md),
[Script deployment](script_deployment.md).

This page walks through getting a [user event script](../script-types/UserEventScript.md) running
using only the NetSuite UI, without SDF. The deployment you end up with is a
[`scriptdeployment`](../gen/records/scriptdeployment.md) record.

## Steps

1. **Upload the script file** to the File Cabinet, into the SuiteScripts folder. You can also
   upload it later from the script record: the plus icon next to the script file list uploads a
   file.
2. **Create the script record** from the file, on the New Script page under Customization >
   Scripting. When you click the button that creates the script record, NetSuite first validates
   the file.
3. **Fill in the script's name and ID.**
4. **Add a deployment line** and choose the record type the script applies to.
5. **Save** the script record together with its deployment.
6. **Test** by working with that record type in the UI.

Once uploaded, the file can still be edited in the File Cabinet.

## Deployment status

A new deployment starts with status **Testing**. In that status the script does not run for other
users, only for you as its owner. Switch the status to **Released** when it should run for
everyone.

## Checking the results

Errors the script logs appear in the execution log of the script deployment.

If you are testing with a record type that is assigned to users (tasks, for instance), the list
view may need to be set to show all users before the assigned records show up.

## Example

A small script you could deploy this way on the task record: it tags new tasks that are due
within two days by prefixing their title. Deploy it with status Testing first and create a task to
see the effect.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log'], (log) => {
  const SOON_MS = 2 * 24 * 60 * 60 * 1000;

  const beforeSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const taskRec = context.newRecord;
    const due = taskRec.getValue({ fieldId: 'duedate' });
    if (!(due instanceof Date) || due.getTime() - Date.now() > SOON_MS) {
      return;
    }
    const title = taskRec.getValue({ fieldId: 'title' }) || '';
    if (!title.startsWith('[Due soon] ')) {
      taskRec.setValue({ fieldId: 'title', value: `[Due soon] ${title}` });
      log.debug({ title: 'Task flagged', details: title });
    }
  };

  return { beforeSubmit };
});
```
