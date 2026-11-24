# Converting SuiteScript 1.0 scripts
<!-- id: topic:ss21_transition.from_ss1 -->

Part of: [Transitioning to SuiteScript 2.1](ss21_transition.md)

Sub-topics:

- [SuiteScript 1.0 API conventions](ss1_api.md)

See also: [Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md) (full mapping table),
[SuiteScript 1.0 to 2.1 API map](ss21_transition.api_map.md),
[SuiteScript 1.0 conversion samples](ss21_transition.ss1_samples.md).

## Why convert

SuiteScript 1.0 now only gets fixes for critical issues. Use 2.1 both for new customizations and
for the ones you already have.

## What changes

The differences fall into two areas: how a script is structured, and the API and language it
uses.

**Structure:**

- Entry points replace the event functions you named on the script record.
- One context object replaces the positional arguments.
- JSDoc tags in the file declare the API version and the script type.
- Modules are loaded through `define` instead of being global.

**API and language:**

- Global `nlapi*` functions are replaced by methods of modules.
- Constants become enums, which are read-only objects. Some enums are static (the same values in
  every account); others are dynamic, with values that depend on the account and its features.
- Methods take an options object instead of positional arguments.
- Subrecords must be handled with the subrecord methods.
- Parameter names can differ between versions. Values can too: [`runtime.Permission`](../modules/N_runtime.md),
  for example, uses `FULL`, `EDIT`, `CREATE`, `VIEW` and `NONE`.
- Some 1.0 functions turn into properties rather than methods; the notes in the
  [mapping table](../conversion/ss1-to-ss21.md) say which.

## Things to watch

- A 1.0 script that named its own variables `log` or `util` collides with the 2.1 global objects of
  those names.
- Using an ECMAScript reserved word as a name raises a syntax error.
- Suitelets have reserved URL parameter names (see [SuiteScript 1.0 Suitelets](ss1_api.suitelets.md)).
- Client scripts run whatever JavaScript the user's browser supports.

## 1.0 APIs with no direct 2.x call

| 1.0 | Use in 2.x |
|---|---|
| `nlapiAddDays`, `nlapiAddMonths` | JavaScript `Date` methods; third-party date libraries are also allowed |
| `nlapiEncrypt` | [N/crypto](../modules/N_crypto.md) and [N/encode](../modules/N_encode.md) |
| Date-time value functions | [N/format](../modules/N_format.md), with time zones |
| `nlapiSetRecoveryPoint`, `nlapiYieldScript` | No 2.x equivalent |
| `nlobjPortlet.setRefreshInterval` | No 2.x equivalent |
| `nlobjCredentialBuilder.replace` | No 2.x equivalent; replace values in credential strings yourself |
| `setContentType`, `setEncoding` | Set the corresponding HTTP headers |
| `setLineItemValues` | [`Sublist.setSublistValue`](../modules/N_ui_serverWidget.md) in N/ui/serverWidget |

## How to convert a script

1. Add the 2.1 entry point structure (JSDoc tags, `define`, entry point functions, `return`).
2. Convert each call using the API map.
3. Upload the new file.
4. Point the existing script record at the new file.

- Do the structure first, then the calls.
- The converted file must be the same script type as the original.
- A custom module (library file) has no entry points, so only its calls need converting.
- When you save the script record, its API version updates to match the file.
- Copy to Account copies one object at a time, together with its dependencies.

## Script types new in 2.x

| Type | Notes |
|---|---|
| [Map/reduce](../script-types/MapReduceScript.md) | Entry points `getInputData`, `map`, `reduce`, `summarize` |
| [SDF installation](../script-types/SDFInstallationScript.md) | |
| [Custom tool](../script-types/CustomTool.md) | Its operations come from a JSON-RPC schema; deployed with SDF |

## Script type notes

### Bundle installation

Entry points `beforeInstall`, `afterInstall`, `beforeUpdate`, `afterUpdate` and `beforeUninstall`
([BundleInstallationScript](../script-types/BundleInstallationScript.md)). The context carries
`toVersion` for install and update and `fromVersion` for update. SuiteBundler is still supported
but gets no new features.

### Mass update

The record type and ID that 1.0 passed as `rec_type` and `rec_id` arrive as `params.type` and
`params.id` in the `each` entry point ([MassUpdateScript](../script-types/MassUpdateScript.md)).

### Scheduled

- `nlapiScheduleScript` is replaced by creating a scheduled script task with
  [`task.create`](../modules/N_task.md) and submitting it.
- There is no equivalent of recovery points or yielding; a scheduled script that restarts begins
  again from the start.
- Work that needs to yield should become a map/reduce script.

### Client

| 1.0 | 2.1 ([ClientScript](../script-types/ClientScript.md)) |
|---|---|
| `pageInit(type)` | `context.mode` |
| `fieldChanged(type, name, linenum)` | `context.sublistId`, `context.fieldId`, `context.line` |
| `recalc` | `sublistChanged` |

- Line indexes start at 0 in 2.1.
- New in 2.1: the `currentRecord` property on the context, a `column` property, a `lineCount` on
  `validateDelete`, and the `localizationContextEnter` and `localizationContextExit` entry points.

### Portlet

- The entry function becomes `render`, with `params.portlet` and `params.column`
  ([Portlet](../script-types/Portlet.md)).
- `setRefreshInterval` is not supported.
- Argument mapping: `name` becomes `options.id`; both `just` and `indent` become `options.align`.
  For [`Portlet.addLine`](../script-types/Portlet.md), `options.align` is a number of spaces to
  indent the line by, and its allowed range is 0 to 5.
- `addEditColumn` takes the column ID, not a column object.
- `clientScriptFileId` identifies a file in the File Cabinet, not a script record.

### Suitelet

The request and response arrive on the context object of `onRequest`
([Suitelet](../script-types/Suitelet.md)).

### User event

- `context.request` exists in `beforeLoad` only for browser requests.
- The `beforeLoad` context has no old record.

### Workflow action

| 1.0 argument | 2.1 |
|---|---|
| `id` | `workflowId` |
| `form` | `scriptContext.form` (only in `beforeLoad`) |
| `type` | `scriptContext.type` |

The `onAction` context also carries `newRecord` and `oldRecord`
([WorkflowActionScript](../script-types/WorkflowActionScript.md)).

### RESTlet

Entry points `get`, `post`, `put` and `delete` ([Restlet](../script-types/Restlet.md)). The script
must expose the entry point for every HTTP method that callers use, and calls must be
authenticated (see [RESTlet authentication](restlet_auth.md)).

## Example: before and after

A 1.0 client field-change function that copies a support case's category into its title prefix:

```js
function prefixTitleWithCategory(type, name, linenum) {
  if (type !== null || name !== 'category') {
    return;
  }
  var category = nlapiGetFieldText('category');
  var title = nlapiGetFieldValue('title') || '';
  if (category && title.indexOf('[' + category + ']') !== 0) {
    nlapiSetFieldValue('title', '[' + category + '] ' + title, false);
  }
}
```

The same logic as a 2.1 client script: an entry point, a context object, and options objects.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define([], () => {
  const fieldChanged = (context) => {
    if (context.sublistId || context.fieldId !== 'category') {
      return;
    }
    const rec = context.currentRecord;
    const category = rec.getText({ fieldId: 'category' });
    const title = rec.getValue({ fieldId: 'title' }) || '';
    if (category && !title.startsWith(`[${category}]`)) {
      rec.setValue({ fieldId: 'title', value: `[${category}] ${title}`, ignoreFieldChange: true });
    }
  };

  return { fieldChanged };
});
```
