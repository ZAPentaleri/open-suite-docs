# SuiteScript platform overview
<!-- id: topic:suitescript_overview -->

Sub-topics:

- [SuiteScript 2.1 transition](ss21_transition.md)

SuiteScript is NetSuite's JavaScript platform for customizing and extending an account. This page
summarizes what the platform offers, where scripts run, which script type fits which job, and how a
script gets deployed. The material it introduces spans an overview, guidance on moving to 2.1, the
developer guide, the records guide, the 2.1 guide, the API reference and the legacy 1.0
documentation.

## What the platform offers

- Modular code with managed dependencies (AMD `define` / `require`).
- Standard JavaScript, plus the ability to use third-party libraries.
- HTTP calls with various content types, and SFTP transfers.
- Caching.
- Paged search results and streaming of flat files.
- Cryptography.
- Promises for asynchronous work.
- Map/Reduce processing for large data volumes.

## Versions

- Update scripts written in SuiteScript 1.x or 2.x to 2.1 by release 2028.2.
- SuiteScript 1.0 is no longer updated. Its documentation is available only as a PDF, and you
  must log in to get it.

See [SuiteScript 2.1 transition](ss21_transition.md).

## Where scripts run

- **Client side:** in the user's browser.
- **Server side:** on NetSuite's servers.

Some script types mix the two. A portlet script runs on the server, but what it produces is
rendered in the browser.

## Choosing a script type

| Job | Script type |
|---|---|
| Processing records as they are created, changed or deleted | [User event](../script-types/UserEventScript.md) |
| Validating input in the browser | [Client](../script-types/ClientScript.md) |
| Custom UI | [Suitelet](../script-types/Suitelet.md) or [user event](../script-types/UserEventScript.md) |
| Batch processing | [Scheduled](../script-types/ScheduledScript.md) |
| Searches | [User event](../script-types/UserEventScript.md) or [scheduled](../script-types/ScheduledScript.md) |
| Utility work | [Suitelet](../script-types/Suitelet.md) or [user event](../script-types/UserEventScript.md) |
| Dashboard content | [Portlet](../script-types/Portlet.md) |
| Work done when a SuiteApp is installed | [Bundle installation](../script-types/BundleInstallationScript.md) or [SDF installation](../script-types/SDFInstallationScript.md) |

Notes on particular types:

- A **user event** script can control what data is returned to clients and to imports.
- A **mass update** script runs only on demand, when a user starts it with Perform Update.
- A **Suitelet** is triggered by an HTTP request to its URL.

> [!SOURCE-ERROR] The source says a Suitelet is triggered by a GET or a PUT request; PUT looks like
> an error in that sentence. Check the [Suitelet](../script-types/Suitelet.md) page for the request
> methods the spec supports instead of relying on that pairing.
<!-- source-error: topic:suitescript_overview | - | suitelet_said_triggered_by_get_or_put -->

## Getting a script to run

There are two routes:

- **SDF** (SuiteCloud Development Framework), recommended for solutions you package and ship.
- **Script and deployment records** created in the UI. The steps are: write the JavaScript file,
  upload it, create a script record for it, then create a deployment record.

## How a record looks to a script

A NetSuite record page is made of body fields, buttons and actions, subtabs, and sublists. A sales
order is the spec's example. Different modules handle different parts:

| Part | Module |
|---|---|
| The record as a whole | [N/record](../modules/N_record.md) |
| Body fields of the record open in the UI | [N/currentRecord](../modules/N_currentRecord.md) |
| Subtabs | [N/ui/serverWidget](../modules/N_ui_serverWidget.md) |
| Sublists | [N/ui/serverWidget](../modules/N_ui_serverWidget.md) |

Built-in buttons cannot be scripted. You can add your own buttons with
[N/ui/serverWidget](../modules/N_ui_serverWidget.md), and custom actions through
[N/action](../modules/N_action.md).

## Example

A user event script that adds a custom button to a vendor bill in view mode. The button runs a
function from a client script module that the form loads; that client module is not shown here.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define([], () => {
  const beforeLoad = (context) => {
    if (context.type !== context.UserEventType.VIEW) {
      return;
    }
    context.form.clientScriptModulePath = './vb_duplicate_check_cs.js';
    context.form.addButton({
      id: 'custpage_flag_duplicate',
      label: 'Flag Possible Duplicate',
      functionName: 'flagDuplicateBill'
    });
  };

  return { beforeLoad };
});
```
