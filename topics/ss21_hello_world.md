# SuiteScript 2.1 Hello World tutorial
<!-- id: topic:ss21_hello_world -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

The getting-started tutorial walks through a first 2.1 script end to end: a
[client script](../script-types/ClientScript.md) that, using [N/ui/dialog](../modules/N_ui_dialog.md),
pops up a dialog when a task record loads. This page explains the same steps and concepts, then
shows a different minimal script you can use for the same exercise.

## The four steps

1. Turn on client SuiteScript in the account.
2. Write the script file and upload it to the File Cabinet.
3. Create the script record and its deployment record.
4. Test it.

## Step 1: enable SuiteScript

- The switches are under **Setup > Company > Enable Features**, on the **SuiteCloud** subtab. You need
  the Enable Features permission.
- Quick check: if **Customization > Scripting > Scripts** is in your menu, SuiteScript is already on.
  If the menu item is missing, that may mean you lack the permission or that your role's menu was
  customized, not necessarily that the feature is off.
- Ticking the client or server SuiteScript box opens a terms of service window, and the terms must
  be accepted before the feature turns on.
- Turn on server SuiteScript too; you will need it for later work.

## Step 2: write and upload the file

Uploaded scripts belong in the **SuiteScripts** folder under **Documents > Files**. Select that folder,
choose to add a file, and upload your `.js` file. A file in the File Cabinet can also be edited in
place there.

The concepts this step relies on:

- **Versions:** the file declares its API version with `@NApiVersion`.
- **Reserved words:** do not name variables or functions after ECMAScript reserved words.
- **Script types and entry points:** `@NScriptType` says what kind of script it is, and the file
  returns entry point functions of that type. A file can return several entry points, as long as
  they belong to its declared type.
- **Modules:** NetSuite APIs are loaded as modules through `define`.
- **Entry point scripts versus custom modules:** the two are formatted differently; see
  [SuiteScript 2.1 custom modules](custom_modules.md).

Details of the file itself:

- `@NApiVersion` and `@NScriptType` are both required, and each tag takes exactly one value.
- The callback's parameters must match the `define` dependencies in order. Naming each parameter
  after its module (`dialog` for `N/ui/dialog`) keeps this readable.
- The entry point function must be referenced in the object the callback returns.
- The `log` object is global, so it can be used without loading a module.
- Wrapping the entry point body in `try`/`catch` means an error in your script does not stop other
  scripts on the same page from running.

For comparison, a scheduled script differs in having a single `execute` entry point.

## Step 3: script and deployment records

- Create the script record from **Customization > Scripting > Scripts > New**, picking the uploaded
  file. The plus icon to the right of the script file list opens an upload window if the file is
  not in the File Cabinet yet.
- The deployment ties the script to a record type (a task record, in the tutorial), and the entry
  point decides which event on that record runs the code.
- A new deployment's status defaults to **Testing**. While it is in Testing, the script does not
  run for other users; set it to **Released** to run it for everyone.
- Script and deployment records can also be created programmatically.

## Step 4: test

Open a record of the deployed type. The script worked if:

- what it is meant to show appears (in the tutorial, a dialog when the task record loads), and
- its log entry appears in the execution log. Find it on the **Execution Log** subtab when viewing
  the script deployment; the same entries are also on the script record's execution log.

## A minimal script for the exercise

Instead of a dialog on a task, this client script shows a short banner with the case's priority
when a support case is opened for editing, and writes one log line. It uses one module, one entry
point, a `try`/`catch`, and the global `log`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/message'], (message) => {
  const pageInit = (context) => {
    try {
      if (context.mode !== 'edit') {
        return;
      }
      const priority = context.currentRecord.getText({ fieldId: 'priority' }) || 'not set';
      message.create({
        title: 'Editing case',
        message: `Current priority: ${priority}`,
        type: message.Type.INFORMATION
      }).show({ duration: 4000 });
      log.debug({ title: 'pageInit', details: `priority=${priority}` });
    } catch (e) {
      log.error({ title: 'pageInit failed', details: e.message });
    }
  };

  return { pageInit };
});
```

Deploy it to the support case record, open a case in edit mode, and check for the banner and the
log line as described in step 4.

## Where to go next

The tutorial suggests continuing with the user event script tutorial, a user event script sample,
and the custom module tutorial (see [UserEventScript](../script-types/UserEventScript.md) and
[SuiteScript 2.1 custom modules](custom_modules.md)).
