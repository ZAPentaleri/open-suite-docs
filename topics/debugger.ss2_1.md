# Debugging SuiteScript 2.1 Scripts
<!-- id: topic:debugger.ss2_1 -->

Part of: [SuiteScript Debugger](debugger.md)

The 2.1 Script Debugger is for SuiteScript 2.1 only; you cannot debug 2.0 and 2.1 code together. It is
built on Chrome DevTools, and Chrome is the only browser with full support; Firefox and Safari have
limited support or none. You need the **SuiteScript** permission at **Full** level.

Start it from the Script Debugger page with the API version set to **2.1**. The first time, allow
pop-ups for the site, because the debugger opens a DevTools tab.

## What it can debug

| Supported | Not supported |
|---|---|
| [Scheduled scripts](../script-types/ScheduledScript.md), [Suitelets](../script-types/Suitelet.md), [user event scripts](../script-types/UserEventScript.md) | Plug-in implementations |
| Scripts without a deployment, and code snippets | Files hidden in a SuiteBundle |

You still debug one script per session.

## DevTools in the 2.1 debugger

Only the **Console** and **Sources** tabs are supported; **Memory** and **Profiler** are not.

| You can | You cannot |
|---|---|
| Set breakpoints, and disable or re-enable them all at once (they must be set first) | Use source maps |
| Add watches and watch expressions; a watch on a SuiteScript object lists its members | Switch thread context |
| Inspect the call stack and restart from a frame | Edit code |
| Pause, resume and step | Use the Memory or Profiler tabs |
| View local, closure and global scope | Use workspaces |
| Use the console, search, copy a stack trace, see values on hover | Step into a SuiteScript module (Step Into just moves to the next line) |
| Black-box scripts, check code coverage, pretty-print (de-minify) source | |

Other behaviour to expect:

- Log messages, execution metrics and errors appear in the DevTools Console.
- Breakpoints may survive from one session to the next, and so does your DevTools layout.
- Clicking the Console tab on the left closes the source window.
- Closing the DevTools tab does **not** end the session; cancel it with the red **X** button in
  NetSuite. An open DevTools tab is reused for the next session.
- A "WebSocket disconnected" message at the end of a run is normal.

## Debugging code that has no deployment

1. Enter the code and click **Debug Script**. A DevTools tab opens and waits to attach.
2. In NetSuite, the legacy debugger buttons are greyed out except the red **X** that cancels.
3. The `@NApiVersion` tag is ignored in this mode, and you may need a `debugger` statement to stop
   where you want.
4. If DevTools does not show the code, resume, or click the program entry in the call stack.
5. A message reports that the new script execution completed.

## Debugging a deployed script

Requirements: the script file is in the File Cabinet and a script deployment record exists. This
applies to scheduled scripts and Suitelets as well as user event scripts.

1. Click **Debug Existing** and select the script.
2. NetSuite shows that it is waiting for a user action. Perform the triggering action, and a DevTools
   tab opens.
3. The debugger stops at the first line: the `define` or `require` call.
4. When the script ends, a message reports that the deployed script execution completed. The
   WebSocket disconnected message that follows is normal.

As with the legacy debugger, sublists of child records are available only when the parent allows
child record editing.

### Debugging a user event script

The source's example debugs a user event script deployed to customers. For user events, trigger the
record save before **Debug Existing** will list the script.

- **beforeLoad:** create or edit a record (for customers, **Lists > Relationships > Customers > New**).
  The DevTools tab opens as the record loads. To debug beforeLoad again, save the record and start
  over.
- **beforeSubmit and afterSubmit:** these fire when the record is saved.
  1. Open or edit the record, but do not save it.
  2. Click **Debug Existing** and select the script.
  3. Save the record; this triggers beforeSubmit.
  4. Debug beforeSubmit.
  5. Reconnect DevTools for afterSubmit. The reconnect step appears only when the script implements
     both beforeSubmit and afterSubmit; reconnecting reloads the script at afterSubmit.
  6. Debug afterSubmit.

DevTools has to be re-attached for each entry point. To run an entry point again, reload the record
and repeat the steps. A `debugger` statement at the start of an entry point makes execution stop
there.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log'], (log) => {
  const beforeSubmit = (context) => {
    debugger; // first stop after you save the record
    const terms = context.newRecord.getValue({ fieldId: 'terms' });
    log.debug({ title: 'Terms on save', details: `${terms}` });
  };

  const afterSubmit = (context) => {
    debugger; // reached after you reconnect DevTools
    log.debug({ title: 'Saved customer', details: `${context.newRecord.id}` });
  };

  return { beforeSubmit, afterSubmit };
});
```
