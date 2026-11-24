# SuiteScript Debugger
<!-- id: topic:debugger -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

Sub-topics:

- [Legacy debugger for 1.0 and 2.0](debugger.legacy.md)
- [Debugging SuiteScript 2.1 Scripts](debugger.ss2_1.md)

NetSuite has two server-side debuggers; which one you use depends on the script's API version:

| Debugger | Scripts it handles | Where log output appears |
|---|---|---|
| Script Debugger | SuiteScript 1.0 and 2.0 | The Execution Log subtab of the debugger |
| 2.1 Script Debugger | SuiteScript 2.1 | The console of Chrome DevTools |

Both are opened from Customization > Scripting > Script Debugger, and both need the SuiteScript
permission at the Full level.

## Ground rules

- One script per debug session.
- You can debug only your own scripts, and only within your current login session. Opening the
  debugger in a second window shows the same debug session.
- A script running under the debugger is limited to 1,000 usage units. For example, a scheduled
  script that normally has 2,000 units fails when it reaches 1,000 under the debugger.
- No error notification emails are sent for a script while it is being debugged.
- The debugger is not an API test console, an IDE, a deployment interface or a script runner.
  Scheduled scripts, for instance, still have to be queued.
- Scripts hidden in a bundle cannot be debugged.

## Timeouts

| | SuiteScript 1.0 / 2.0 | SuiteScript 2.1 |
|---|---|---|
| Idle timeout | 2 minutes | 5 minutes |
| Overall session limit | 10 minutes | Up to 20 minutes, set through a preference |

Errors and messages:

- `DEBUGGER_IDLE_TIMEOUT` is raised when nothing happens for 5 minutes. That matches the 2.1 idle
  timeout; the spec does not say how it relates to the 2-minute idle timeout of the 1.0/2.0
  debugger, so the two figures look inconsistent.
- `DEBUGGER_SESSION_TIMEOUT` is raised when the session reaches the limit set in the preference.
- On an idle timeout, both debuggers show a message saying the maximum idle time for debugging was
  exceeded. This is message text, not an error code.

The session limit is the preference `IDLE_SESSION_TIMEOUT_IN_MINUTES`, set under Setup > General
Preferences. Changing it also changes the user's ordinary session timeout, not only the
debugger's.

Recovering after a timeout:

- **1.0/2.0, deployed script:** go back and start again with **Debug Existing**.
- **1.0/2.0, on-demand code:** go back and enter the code again.
- **2.1:** reconnecting DevTools does not resume the session. Close the session with the **X**
  button, switch to the editor and choose **Debug Script** again. A DevTools tab that is still open
  is reused for the next session.

## On-demand and deployed debugging

| Mode | What it needs | Use it for |
|---|---|---|
| On-demand | No deployment; you enter the code | Scripts that do not interact with a form or record |
| Deployed | A deployment that you own, with status Testing | Script deployments and core plug-in implementations |

To debug a deployment that is Released, change its status to Testing first.

### The debugger domain

Debugging runs on a separate debugger domain:

- It changes production data: records the script modifies are really modified.
- It may be slower than the normal domain.
- It is available only in production and Release Preview accounts.
- You do not need to log in a second time, and a debugger logo on the page tells you where you are.

## What can be debugged

- **Supported:** server scripts, core plug-in implementations, and on-demand code.
- **Not supported:** client scripts and map/reduce scripts. Client scripts are debugged with the
  browser's developer tools instead (below).
- **2.1 debugger:** [scheduled scripts](../script-types/ScheduledScript.md),
  [Suitelets](../script-types/Suitelet.md) and [user event scripts](../script-types/UserEventScript.md).
  Core plug-ins cannot be debugged with the 2.1 debugger.
- RESTlets and Suitelets can also be debugged with the IDE's debug client.
- [SDF installation scripts](../script-types/SDFInstallationScript.md) are recorded as running in
  the client browser.

> [!SOURCE-ERROR] The source says SDF installation scripts run in the client browser, which looks
> doubtful for an installation script. Check the SDF installation script page before relying on
> this when choosing how to debug one.
<!-- source-error: topic:debugger | - | sdf_installation_said_to_run_on_client_browser -->

## Debugging client scripts

Client scripts run in the browser, so you debug them with the browser's developer tools.

Supported browsers are Chrome, Firefox, Edge and Safari; Chrome and Firefox are preferred.

The script must contain a `debugger` statement. Place it near the top of the client script, and test
on a form or record the script is deployed to. The steps:

1. Deploy the script with the `debugger` statement in it.
2. Get the page ready for the action that triggers the script, but do not save.
3. Open the browser's developer tools (for example Ctrl+Shift+I in Chrome or Firefox,
   Ctrl+Option+I in Safari).
4. Perform the triggering action.
5. Debug.
6. Close the developer tools.

If you open the developer tools too early, execution pauses on every action. For minified code, use
the developer tools' pretty-print button.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/log'], (log) => {
  const fieldChanged = (context) => {
    // Remove before release: pauses here only while the developer tools are open.
    debugger;
    if (context.fieldId !== 'otherrefnum') {
      return;
    }
    const poNumber = context.currentRecord.getValue({ fieldId: 'otherrefnum' });
    log.debug({ title: 'PO number entered', details: `${poNumber}` });
  };

  return { fieldChanged };
});
```

## Debugging a RESTlet

A RESTlet is debugged as a deployed script, so its deployment must have status Testing. An
`Authorization` header on the request can prevent the RESTlet from being debugged.

1. Remove the `Authorization` header from the request you will send.
2. Set the RESTlet deployment status to Testing.
3. Click **Debug Existing** (the button appears only when deployments with status Testing exist)
   and choose the RESTlet. A banner shows the cookies for the selected RESTlet, and execution waits
   at the RESTlet wrapper.
4. Copy the cookies.
5. Send the request with those cookies.

Test the HTTP request on its own first, so you know it is correct before the debugger is involved.
