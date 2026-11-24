# Converting SuiteScript 1.0 to 2.1
<!-- id: map:ss1_to_ss2 -->

This page is for reading, converting and minimally repairing legacy SuiteScript 1.0 code. It pairs
every 1.0 function, `nlobj*` object and object method that the spec's mapping covers with its 2.x
counterpart, then walks through the recurring rewrite patterns with before-and-after code. It is not
a guide to writing new 1.0 code.

The 1.0 reference itself lives in the `../ss1/` pages: [records](../ss1/records.md),
[fields and sublists](../ss1/fields-sublists.md),
[search, scheduling and context](../ss1/search-scheduling-context.md), [UI and HTTP](../ss1/ui-http.md),
[utilities](../ss1/utilities.md), [record and search objects](../ss1/objects-record-search.md),
[service objects](../ss1/objects-services.md), [form objects](../ss1/objects-forms.md) and
[other UI objects](../ss1/objects-ui-other.md). Background is in
[SuiteScript 1.0 concepts](../topics/ss1_api.md),
[Converting SuiteScript 1.0 scripts](../topics/ss21_transition.from_ss1.md) and
[the 1.0 to 2.1 API map topic](../topics/ss21_transition.api_map.md).

Ground rules before you start:

- A 2.1 script cannot call 1.0 APIs. Converting a file means replacing every `nlapi*` call and every
  `nlobj*` method, not mixing the two.
- The mapping is not always one-to-one. Some 1.0 functions fan out to several 2.x members, some
  collapse into a property, and a few have nothing to map to.
- The mapping leaves out 1.0 APIs that were already deprecated, and anything that is new in 2.1.
  For example, `nlapiCreateReportDefinition` and `nlapiCreateReportForm` have no mapping entry.
- The mapping covers objects and their methods, but not every 1.0 object constructor. In particular
  it has no entry for `new nlobjSearchFilter(...)` (see the search pattern below).
- Parameter names may differ between versions even when the meaning is the same. Always check the
  2.x parameter table before you fill in an options object.
- SuiteScript 1.0 now only gets fixes for critical issues; the transition material recommends 2.1
  for both new and existing customizations.

## How to read the mapping table

Each row of the [full table](#full-mapping-table) is one entry of the spec's mapping
(`map:ss1_to_ss2`). The columns are:

| Column | What it holds |
|---|---|
| 1.0 symbol | The legacy function, object or `object#method` (the section heading links to its 1.0 page) |
| 2.x symbol(s) | The spec ID of each 2.x target. `N/x.y` lives on `../modules/N_x.md` (slashes in deeper paths become underscores, so `N/ui/serverWidget` is `N_ui_serverWidget.md`); `script:Name...` lives on `../script-types/Name.md`. A dash means the mapping names no target |
| Relation | How the two sides correspond (see below) |
| Arguments (1.0) | The 1.0 positional arguments the mapping lists, as `position name`, counting from 0. A dash means the mapping lists none |
| Tags | The mapping's shape tags (see below) |
| Notes | Caveats the spec states for that pair, in plain words |

About the arguments column: the mapping records only the position and the 1.0 name of each
argument. The source does not state which 2.x option receives each argument, and it records no value
transform for any of them. Read `0 type, 1 id` as "the first argument was `type`, the second `id`",
then find the matching option in the 2.x member's parameter table by meaning. Where another part of
the spec does pair them (the portlet methods, for example), the Notes say so.

The source also does not record how return values change shape between the versions (for example
a string becoming a typed value, or an ID becoming a whole object). Read the **Returns** line of the
2.x member before relying on what the converted call gives back.

### Relations

| Relation | Meaning for the converter |
|---|---|
| `equivalent` | Same job, same shape. Swap the name and the calling convention. |
| `renamed` | Same job under a different 2.x name. Any arguments usually move into an options object as well. |
| `options_object` | The 2.x member takes one object of named options instead of positional arguments. The most common case. |
| `split` | One 1.0 member becomes several 2.x members. You pick one per call site, usually from the value of one 1.0 argument. |
| `approximate` | There is a 2.x way to reach the same outcome, but not as a drop-in call. Expect to restructure the code. |
| `pattern_change` | The 2.x side works differently, most often a getter or setter call that becomes a property read or assignment. |
| `no_equivalent` | Nothing in 2.x replaces it. The Notes give the closest approach only when the spec offers one. |

### Mapping tags

| Tag | What it tells you |
|---|---|
| `alternative_targets` | The listed targets are alternatives. Use one of them, not all. |
| `multi_module` | The targets live in different modules, so the `define` dependency list depends on which one you use. For record-level calls the pair is usually `N/record.Record` and `N/currentRecord.CurrentRecord`. |
| `module_level_only` | The mapping points at a whole module rather than a member. You build the replacement from that module's members yourself. |
| `getter_to_property` | A `getX()` call becomes a read of a property on the 2.x object. |
| `setter_to_property` | A `setX(value)` call becomes an assignment to a property. |
| `target_is_module_property` | The replacement is a property of a module itself (for example `N/runtime.accountId`), read directly from the module binding rather than from an object. |
| `target_is_script_object` | The replacement is an object a 2.x entry point hands you (such as the portlet object a Portlet script receives), not something you import. |
| `feature_retired` | The feature itself is gone; there is nothing to convert to. |
| `use_native_date_setdate` / `use_native_date_setmonth` | No SuiteScript API replaces the call; use the JavaScript `Date` methods named in the Notes. |

## Conversion workflow

The transition material lays out a workflow for a 1.0 file: add the 2.x structure first (JSDoc
annotations, the `define` wrapper and the returned entry-point object), then convert the calls one by
one using the mapping, upload the new file, and point the existing script record at it. Saving the
script record updates its API version. The converted file must keep the same script type. A custom
module (a library file with no entry points) only needs the call conversion.

Structural changes that apply to every file:

- Entry points replace the free-standing event functions, and a single context object replaces the
  positional arguments those functions received.
- JSDoc tags declare the API version and the script type.
- Modules are loaded through `define`, and their enums are read-only objects. Some enum values exist
  in every account; others depend on the account and its enabled features.
- Arguments are passed as options objects, and subrecords must be handled through the subrecord
  methods.

Two runtime hazards are worth checking while you are in the file. A 1.0 script that declares its own
variable named `log` or `util` collides with the 2.1 globals of the same name (see
[Globals](../globals.md)). And because 2.1 is a newer ECMAScript dialect, using an ECMAScript reserved
word as an identifier raises a syntax error.

## Patterns

Each pattern shows a small 1.0 function and its 2.1 rewrite. The 1.0 snippets are plain functions (no
`define`), as they appeared in legacy files. Record and field IDs come from the spec's record
definitions (`purchaseorder`, `supportcase`, `inventoryitem`); `cust*` IDs are invented examples.

### Positional arguments become an options object

In 1.0 every argument is positional and order matters. In 2.x nearly every call takes one object
whose keys name the arguments, so order stops mattering but names start to. The mapping tells you
which positions exist; the 2.x parameter table tells you the option names.

**Before (1.0):**

```js
function flagLatePurchaseOrder(poId) {
  var po = nlapiLoadRecord('purchaseorder', poId);
  var due = po.getFieldValue('duedate');
  nlapiSubmitField('purchaseorder', poId, 'memo', 'Expedite: due ' + due);
  return due;
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/record'], (record) => {
  const flagLatePurchaseOrder = (poId) => {
    const po = record.load({ type: record.Type.PURCHASE_ORDER, id: poId });
    const due = po.getValue({ fieldId: 'duedate' });
    record.submitFields({
      type: record.Type.PURCHASE_ORDER,
      id: poId,
      values: { memo: `Expedite: due ${due}` }
    });
    return due;
  };
  return { flagLatePurchaseOrder };
});
```

What changed:

- `nlapiLoadRecord(type, id, initializeValues)` became `record.load` with named options. Record types
  come from the `record.Type` enum.
- `nlapiSubmitField(type, id, fields, values, doSourcing)` became `record.submitFields`. The two
  parallel 1.0 arguments for field names and values become one `values` object keyed by field ID.
- `nlobjRecord#getFieldValue(name)` became `Record.getValue`, which returns a typed value rather than
  a string (a checkbox comes back as a boolean, for instance). Check the getter's Returns line before
  doing string operations on the result.

### `nlobj*` objects become `N/*` objects; getters and setters become properties

Most `nlobj*` types have a 2.x counterpart object. Methods that only read or write one attribute
(tagged `getter_to_property` / `setter_to_property`) turn into plain properties.

**Before (1.0):**

```js
function archiveExport(csvText, folderId) {
  var exportFile = nlapiCreateFile('po_export.txt', 'PLAINTEXT', csvText);
  exportFile.setFolder(folderId);
  exportFile.setDescription('Nightly purchase order export');
  var fileId = nlapiSubmitFile(exportFile);
  return fileId;
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/file'], (file) => {
  const archiveExport = (csvText, folderId) => {
    const exportFile = file.create({
      name: 'po_export.txt',
      fileType: file.Type.PLAINTEXT,
      contents: csvText
    });
    exportFile.folder = folderId;
    exportFile.description = 'Nightly purchase order export';
    return exportFile.save();
  };
  return { archiveExport };
});
```

`nlobjFile#setFolder` and `#setDescription` became the `folder` and `description` properties of
`N/file.File`, and `nlapiSubmitFile(file)` became a method on the file object itself (`File.save`).

Watch for 2.x properties whose names read as the opposite of the 1.0 argument:
`nlobjButton#setVisible(visible)` maps to `Button.isHidden`, `nlobjFieldGroup#setShowBorder(show)` to
`FieldGroup.isBorderHidden`, `nlobjAssistant#setNumbered(hasStepNumber)` to
`Assistant.hideStepNumber`, `#setOrdered(ordered)` to `Assistant.isNotOrdered` and
`#setShortcut(show)` to `Assistant.hideAddToShortcutsLink`. The source does not state how the boolean
should be converted for these pairs, so check each property's meaning on its 2.x page before copying
the 1.0 value across; going by the names, the value usually has to be negated.

### 1.0 event functions become 2.x entry points

In 1.0 the function names were free; the script record said which function handled which event. In
2.x the script type is declared with `@NScriptType`, and the object returned from `define` must use
the fixed entry-point names. Each entry point receives one context object.

| 1.0 script type | 1.0 event functions and their arguments | 2.x script type | 2.x entry points and context |
|---|---|---|---|
| User event | `beforeLoad(type, form, request)`; submit functions take `type`; records read with `nlapiGetNewRecord` / `nlapiGetOldRecord` | [UserEventScript](../script-types/UserEventScript.md) | `beforeLoad`, `beforeSubmit`, `afterSubmit`; `context.newRecord`, `context.type`; `context.oldRecord` in the submit entry points only; `context.request` in beforeLoad is only there for browser requests |
| Client | `pageInit(type)`, `saveRecord()`, `validateField(type, name, linenum)`, `fieldChanged(type, name, linenum)`, `postSourcing(type, name)`, `lineInit(type)`, `validateLine(type)`, `recalc(type)`, `validateInsert(type)`, `validateDelete(type)` | [ClientScript](../script-types/ClientScript.md) | Same names except `recalc`, which becomes `sublistChanged`. `pageInit`'s `type` becomes `context.mode`; `type, name, linenum` become `context.sublistId, context.fieldId, context.line`. New in 2.x: `context.currentRecord`, a `column` property, a line count on `validateDelete`, and the `localizationContextEnter` / `localizationContextExit` entry points |
| Suitelet | `(request, response)` with `nlobjRequest` / `nlobjResponse` | [Suitelet](../script-types/Suitelet.md) | `onRequest`; `context.request`, `context.response` |
| RESTlet | One function per HTTP method, named on the script record | [Restlet](../script-types/Restlet.md) | `get`, `post`, `put`, `delete`; the script must expose an entry point for each method it is called with, and RESTlet authentication is required |
| Scheduled | One function receiving `type` | [ScheduledScript](../script-types/ScheduledScript.md) | `execute`; `context.type` |
| Portlet | `(portlet, column, entityid)`: column is 1 left, 2 middle, 3 right; `entityid` only on customer dashboards | [Portlet](../script-types/Portlet.md) | `render`; `params.portlet`, `params.column` |
| Mass update | `(rec_type, rec_id)` | [MassUpdateScript](../script-types/MassUpdateScript.md) | `each`; `params.type`, `params.id` |
| Workflow action | `(id, form, type)`, form in beforeLoad only; the record read with `nlapiGetNewRecord` | [WorkflowActionScript](../script-types/WorkflowActionScript.md) | `onAction`; `context.workflowId`, `context.form` (beforeLoad only), `context.type`, `context.newRecord`, `context.oldRecord` |
| Bundle installation | Install and update functions with `toversion` / `fromversion` | [BundleInstallationScript](../script-types/BundleInstallationScript.md) | `beforeInstall`, `afterInstall`, `beforeUpdate`, `afterUpdate`, `beforeUninstall`; `toVersion` on install and update, `fromVersion` on update |

2.x also adds script types with no 1.0 ancestor: [MapReduceScript](../script-types/MapReduceScript.md)
(`getInputData`, `map`, `reduce`, `summarize`), SDF installation scripts and custom tools.

**Before (1.0 user event, beforeSubmit):**

```js
function rememberPreviousAssignee(type) {
  if (type !== 'edit') {
    return;
  }
  var current = nlapiGetNewRecord();
  var previous = nlapiGetOldRecord();
  var before = previous.getFieldValue('assigned');
  if (current.getFieldValue('assigned') !== before) {
    current.setFieldValue('custevent_prev_assignee', before);
  }
}
```

**After (2.1):**

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define([], () => {
  const beforeSubmit = (context) => {
    if (context.type !== context.UserEventType.EDIT) {
      return;
    }
    const current = context.newRecord;
    const previous = context.oldRecord;
    const before = previous.getValue({ fieldId: 'assigned' });
    if (current.getValue({ fieldId: 'assigned' }) !== before) {
      current.setValue({ fieldId: 'custevent_prev_assignee', value: before });
    }
  };
  return { beforeSubmit };
});
```

The 1.0 `type` strings are lowercase (`edit`, `create` and so on); in 2.x compare `context.type`
against the `UserEventType` values. The beforeLoad context has no `oldRecord`.

**Before (1.0 client, fieldChanged):**

```js
function warnOnLargeQuantity(type, name, linenum) {
  if (type === 'item' && name === 'quantity') {
    var qty = parseFloat(nlapiGetCurrentLineItemValue('item', 'quantity')) || 0;
    if (qty > 500) {
      alert('Line ' + linenum + ' orders an unusually large quantity');
    }
  }
}
```

**After (2.1):**

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define([], () => {
  const fieldChanged = (context) => {
    if (context.sublistId === 'item' && context.fieldId === 'quantity') {
      const qty = Number(context.currentRecord.getCurrentSublistValue({
        sublistId: 'item',
        fieldId: 'quantity'
      })) || 0;
      if (qty > 500) {
        alert(`Line ${Number(context.line) + 1} orders an unusually large quantity`);
      }
    }
  };
  return { fieldChanged };
});
```

`linenum` was 1-based; `context.line` is a 0-based index, so add 1 only when you show it to a person.
Client scripts run in the browser, so use JavaScript the target browsers support.

**Before (1.0 Suitelet):**

```js
function showOrderStatus(request, response) {
  var poId = request.getParameter('custom_po');
  var status = nlapiLookupField('purchaseorder', poId, 'status', true);
  response.write('Status: ' + status);
}
```

**After (2.1):**

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/search'], (search) => {
  const onRequest = (context) => {
    const poId = context.request.parameters.custom_po;
    const found = search.lookupFields({
      type: search.Type.PURCHASE_ORDER,
      id: poId,
      columns: ['status']
    });
    const status = Array.isArray(found.status) && found.status.length > 0
      ? found.status[0].text
      : found.status;
    context.response.write({ output: `Status: ${status}` });
  };
  return { onRequest };
});
```

`nlobjRequest#getParameter(name)` became a read of the `parameters` property, and
`nlobjResponse#write(output)` became `ServerResponse.write`. `search.lookupFields` returns a scalar for
a single-value field and an array of value/text pairs for a select or multi-select field, instead of
taking a "return text" flag. Suitelets have reserved URL parameter names, which is why the example
gives its own parameter a distinctive prefix.

### Line and column indexes: 1-based in 1.0, 0-based in 2.x

Every 1.0 sublist call counts lines from 1; in 2.1 line indexes start at 0. The API map topic singles
out `nlapiGetLineItemField`, `nlapiGetLineItemMatrixField`, `nlapiGetLineItemMatrixValue`,
`nlapiGetLineItemText`, `nlapiGetLineItemValue`, `nlapiGetLineItemValues`, `nlapiSelectLineItem`,
`nlapiSetLineItemValue` and `nlapiViewLineItemSubrecord`, and the transition topic states the 0-based
rule for 2.1 in general; the table flags each row that passes a line argument. Loops are the usual
place this goes wrong.

**Before (1.0):**

```js
function countBackorderedLines(poId) {
  var po = nlapiLoadRecord('purchaseorder', poId);
  var count = po.getLineItemCount('item');
  var waiting = 0;
  for (var line = 1; line <= count; line++) {
    var ordered = parseFloat(po.getLineItemValue('item', 'quantity', line)) || 0;
    var received = parseFloat(po.getLineItemValue('item', 'quantityreceived', line)) || 0;
    if (received < ordered) {
      waiting++;
    }
  }
  return waiting;
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/record'], (record) => {
  const countBackorderedLines = (poId) => {
    const po = record.load({ type: record.Type.PURCHASE_ORDER, id: poId });
    const count = po.getLineCount({ sublistId: 'item' });
    let waiting = 0;
    for (let line = 0; line < count; line++) {
      const ordered = Number(po.getSublistValue({ sublistId: 'item', fieldId: 'quantity', line })) || 0;
      const received = Number(po.getSublistValue({ sublistId: 'item', fieldId: 'quantityreceived', line })) || 0;
      if (received < ordered) {
        waiting++;
      }
    }
    return waiting;
  };
  return { countBackorderedLines };
});
```

### Checkbox values and control flags

1.0 represents checkbox field values as the strings `'T'` and `'F'`, while its own control flags
such as `firefieldchanged` and `synchronous` are real booleans, not `'T'`/`'F'`. In 2.x the record
getters return a boolean for a checkbox field, and the setters expect one.

**Before (1.0):**

```js
function retireCases(caseIds) {
  for (var i = 0; i < caseIds.length; i++) {
    nlapiSubmitField('supportcase', caseIds[i], 'isinactive', 'T');
  }
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/record'], (record) => {
  const retireCases = (caseIds) => {
    caseIds.forEach((caseId) => {
      record.submitFields({
        type: record.Type.SUPPORT_CASE,
        id: caseId,
        values: { isinactive: true }
      });
    });
  };
  return { retireCases };
});
```

A comparison such as `value === 'T'` silently becomes always-false after conversion, so search
converted code for `'T'` and `'F'`. One flag also flips meaning: 1.0's `firefieldchanged` (true means
the field-changed script runs; default true) corresponds to `Record.setValue`'s `ignoreFieldChange`,
where true means the field change and its follow-on events are ignored (default false). Negate the
value when you move it across, and check each setter's own parameter table.

### "Null when nothing found" versus 2.x results

`nlapiSearchRecord` returns `null`, not an empty array, when a search finds nothing, and it stops at
1,000 rows. Legacy code is full of `if (results != null)` guards.

Filters in 1.0 are built with the constructor `new nlobjSearchFilter(name, join, operator, value1,
value2)` (documented on [record and search objects](../ss1/objects-record-search.md)). The mapping
pairs only the `nlobjSearchFilter` object with `N/search.Filter`; it has no entry for the
constructor. **The source does not state which 2.x call replaces `new nlobjSearchFilter(...)`.** The
rewrite below happens to build its filter with `search.createFilter`, a function that exists in
`N/search`, but that is this page's choice for the example, not a replacement the mapping names.
Check the `N/search` page and pick the way of supplying filters that suits your code.

**Before (1.0):**

```js
function caseTitlesFor(customerId) {
  var filters = [new nlobjSearchFilter('company', null, 'anyof', customerId)];
  var columns = [new nlobjSearchColumn('title')];
  var results = nlapiSearchRecord('supportcase', null, filters, columns);
  var titles = [];
  if (results === null) {
    return titles;
  }
  for (var i = 0; i < results.length; i++) {
    titles.push(results[i].getValue('title'));
  }
  return titles;
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/search'], (search) => {
  const caseTitlesFor = (customerId) => {
    // Example choice only: the mapping names no 2.x call for the 1.0 filter constructor.
    const companyFilter = search.createFilter({
      name: 'company',
      operator: search.Operator.ANYOF,
      values: [customerId]
    });
    const caseSearch = search.create({
      type: search.Type.SUPPORT_CASE,
      filters: [companyFilter],
      columns: ['title']
    });
    const titles = [];
    caseSearch.run().each((result) => {
      titles.push(result.getValue({ name: 'title' }));
      return true;
    });
    return titles;
  };
  return { caseTitlesFor };
});
```

`ResultSet.getRange` returns an empty array when the range holds no results (and at most 1,000 rows
per call), and `ResultSet.each` simply never calls the callback; returning `true` from the callback
continues, `false` stops. `ResultSet.each` handles up to 4,000 rows per call.

### Targets that are module properties or script objects

`nlapiGetContext()` returned one `nlobjContext`; 2.x splits it into the script, session and user
objects of `N/runtime` (`getCurrentScript`, `getCurrentSession`, `getCurrentUser`). Five context
getters are tagged `target_is_module_property`: their replacement is a property of the `N/runtime`
module itself. `getCompany` becomes `runtime.accountId`, `getEnvironment` becomes `runtime.envType`,
`getExecutionContext` becomes `runtime.executionContext`, `getQueueCount` becomes `runtime.queueCount`
and `getVersion` becomes `runtime.version`.

**Before (1.0):**

```js
function shouldSkipRun() {
  var ctx = nlapiGetContext();
  if (ctx.getExecutionContext() === 'webservices') {
    return true;
  }
  var floor = parseInt(ctx.getSetting('SCRIPT', 'custscript_min_usage'), 10) || 0;
  return ctx.getRemainingUsage() < floor;
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/runtime'], (runtime) => {
  const shouldSkipRun = () => {
    if (runtime.executionContext === runtime.ContextType.WEBSERVICES) {
      return true;
    }
    const script = runtime.getCurrentScript();
    const floor = Number(script.getParameter({ name: 'custscript_min_usage' })) || 0;
    return script.getRemainingUsage() < floor;
  };
  return { shouldSkipRun };
});
```

`getSetting(type, name)` is a split chosen by `type`: `SCRIPT` goes to `Script.getParameter`,
`SESSION` to `Session.get`, `FEATURE` to `runtime.isFeatureInEffect` and `PERMISSION` to
`User.getPermission`. The user getters (`nlapiGetRole`, `nlapiGetDepartment`, `nlapiGetLocation`,
`nlapiGetSubsidiary`) become properties of `N/runtime.User`. `nlapiGetUser` is mapped to
`runtime.getCurrentUser()`, which gives you the whole `User` object, while the 1.0 function returned
a number (the user's ID, or -4 when no user is authenticated). The user's ID is the `id` property
(the target of `nlobjContext#getUser`). The source is flagged for mapping `nlapiGetUser` to the user
object rather than the ID; the details are on the [1.0 page](../ss1/search-scheduling-context.md).

Rows tagged `target_is_script_object` point at objects a 2.x entry point hands you. `nlobjPortlet`
becomes the portlet object passed to the Portlet `render` entry point as `params.portlet`; its methods
are listed under [Portlet](../script-types/Portlet.md). `nlobjResponse#setEncoding` points at the
Suitelet's response: the transition material says to set HTTP headers instead, and an `N/https` server
response sends text as UTF-8 by default.

### Split targets and multi-module targets

A `split` row means you choose a 2.x member per call site. The deciding input is usually one 1.0
argument: the `type` of `nlapiLogExecution` picks `log.debug`, `log.audit`, `log.error` or
`log.emergency`; the `type` of `nlapiResolveURL` / `nlapiSetRedirectURL` picks the `url.resolve*` or
`redirect.*` function; the HTTP method of `nlapiRequestURL` picks `http.get`, `http.post`, `http.put`,
`http.delete` or the general `http.request`; the document type of `nlapiPrintRecord` picks the
`render` function.

**Before (1.0):**

```js
function notifyBuyer(senderId, buyerId, poNumber) {
  nlapiLogExecution('AUDIT', 'PO notice', 'Sending approval notice for ' + poNumber);
  nlapiSendEmail(senderId, buyerId, 'PO ' + poNumber + ' needs approval', 'Please review the order.');
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/log', 'N/email'], (log, email) => {
  const notifyBuyer = (senderId, buyerId, poNumber) => {
    log.audit({ title: 'PO notice', details: `Sending approval notice for ${poNumber}` });
    email.send({
      author: senderId,
      recipients: [buyerId],
      subject: `PO ${poNumber} needs approval`,
      body: 'Please review the order.'
    });
  };
  return { notifyBuyer };
});
```

Rows tagged `alternative_targets` plus `multi_module` (most record and sublist functions) list the
same method on `N/record.Record` and on `N/currentRecord.CurrentRecord`. In a client script working on
the record the user has open, call it on the current record from the context; elsewhere call it on a
`Record` you loaded, created, copied or transformed, or on `context.newRecord`. The 1.0 global record
functions (`nlapiGetFieldValue` and friends) had no record argument because they always acted on the
current record; in 2.x you always call through an object.

Subrecords follow the same pattern. 1.0 had separate create, edit and view calls
(`nlapiCreateSubrecord`, `nlapiEditSubrecord`, `nlapiViewSubrecord` and their line forms); the
mapping sends all three to the same 2.x getter (`getSubrecord`, `getCurrentSublistSubrecord`,
`getSublistSubrecord`).

### Scheduling, yielding and retired features

`nlapiScheduleScript(scriptId, deployId, params)` is replaced by creating a scheduled-script task with
`task.create` and submitting it.

**Before (1.0):**

```js
function queuePriceRebuild(batchId) {
  return nlapiScheduleScript('customscript_rebuild_pricing', 'customdeploy_rebuild_pricing', {
    custscript_batch: batchId
  });
}
```

**After (2.1):**

```js
/** @NApiVersion 2.1 */
define(['N/task'], (task) => {
  const queuePriceRebuild = (batchId) => {
    const job = task.create({
      taskType: task.TaskType.SCHEDULED_SCRIPT,
      scriptId: 'customscript_rebuild_pricing',
      deploymentId: 'customdeploy_rebuild_pricing',
      params: { custscript_batch: batchId }
    });
    return job.submit();
  };
  return { queuePriceRebuild };
});
```

`nlapiYieldScript` and `nlapiSetRecoveryPoint` have no call-for-call replacement in scheduled
scripts. The mapping points them at the Map/Reduce script type because a map/reduce script yields on
its own; the transition material recommends map/reduce for work that needs to yield, and warns that a
restarted 2.x scheduled script begins again from the start.

**Before (1.0 scheduled script):**

```js
function clearRepriceFlags(type) {
  var results = nlapiSearchRecord('inventoryitem', 'customsearch_reprice_queue');
  for (var i = 0; results !== null && i < results.length; i++) {
    nlapiSubmitField('inventoryitem', results[i].getId(), 'custitem_reprice_flag', 'F');
    if (nlapiGetContext().getRemainingUsage() < 100) {
      nlapiYieldScript();
    }
  }
}
```

**After (2.1 map/reduce):**

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/search', 'N/record'], (search, record) => {
  const getInputData = () => search.load({ id: 'customsearch_reprice_queue' });

  const map = (context) => {
    // Each search result arrives as a JSON string.
    const row = JSON.parse(context.value);
    record.submitFields({
      type: record.Type.INVENTORY_ITEM,
      id: row.id,
      values: { custitem_reprice_flag: false }
    });
  };

  return { getInputData, map };
});
```

Entries with nothing to convert to:

- `nlapiOutboundSSO` is tagged as a retired feature: there is no replacement. In 1.0 it needed the
  features "SuiteSignOn" and "web services" [sic]; the source writes the second name in lowercase
  rather than as its UI label (see the [1.0 page](../ss1/utilities.md)).
- `nlapiSendFax`, `nlapiRefreshLineItems`, `nlobjError#getUserEvent`, `nlobjResponse#getError` and
  `nlobjPortlet#setRefreshInterval` have no 2.x equivalent, and the source does not state any
  alternative approach for them.
- `nlobjSubList#setLineItemValues` has no equivalent in the mapping; the transition material says to
  set values one at a time with `N/ui/serverWidget.Sublist#setSublistValue` instead.
- `nlapiAddDays` and `nlapiAddMonths` have no 2.x API. Use the JavaScript `Date` methods
  (`setDate(getDate() + n)`, `setMonth(getMonth() + n)`) or a third-party date library.

Related near-misses: the six `*DateTimeValue` functions (and their `nlobjRecord` methods) map only to
the `N/format` module, for the time-zone conversion; `nlapiEncrypt` maps to `N/crypto` together with
`N/encode`; `nlapiSubmitCSVImport` maps only to the `N/task` module, while the `nlobjCSVImport` object
maps to `N/task.CsvImportTask`.

### Other script-type differences

- Portlets: on the portlet object, `addColumn`'s and `addField`'s `name` goes to `options.id`, and
  `addColumn`'s `just` and `addLine`'s `indent` go to `options.align`; `addLine`'s indent takes 0 to 5.
  `addEditColumn` takes a column ID rather than a column object. `clientScriptFileId` (the target of
  the `setScript` methods) names a file, not a script record.
- Workflow action scripts: the 1.0 `id` argument becomes `context.workflowId`, and the form is only
  available in beforeLoad.
- Suitelets: the request and response come in on the context object rather than as two arguments.
- RESTlets need RESTlet authentication, and must expose an entry point for every HTTP method they
  are called with.
- `nlobjField` maps to two objects: `N/ui/serverWidget.Field` for fields on Suitelet pages, and
  `N/record.Field` for field metadata on records.
- `nlobjConfiguration` maps to `N/record.Record`, because `config.load` returns a record.

## Where the source is silent

- The mapping never states which 2.x option receives a 1.0 argument, nor any value transform, so
  every `options_object` row needs a by-meaning match against the 2.x parameter table.
- The mapping does not record changes in return shape (a number becoming an object, a string becoming
  a typed value); read the 2.x member's Returns line.
- The source does not state which 2.x call replaces the 1.0 constructor `new nlobjSearchFilter(...)`.
  The mapping pairs only the `nlobjSearchFilter` object with `N/search.Filter` and has no entry for
  the constructor.
- For the inverted-sense setter pairs above, the source does not state how the boolean converts.
- For `approximate` rows aimed at a whole module (`module_level_only`), the spec gives no recipe
  beyond the module name and, for some rows, a one-line hint.
- For `nlapiSendFax`, `nlapiRefreshLineItems`, `nlobjError#getUserEvent`, `nlobjResponse#getError`
  and `nlobjPortlet#setRefreshInterval`, the source states no alternative.
- `nlapiCheckPasswordField` is mapped with six argument slots, but its 1.0 signature is not verified
  in the spec, so those names come from the mapping alone.

## Full mapping table

477 entries, grouped by the 1.0 reference page that documents the 1.0 side, in the mapping's own
order within each group. Where the source is flagged as unreliable for a row, the Notes say so and
the details (with the source-error callout) are on the linked 1.0 page; flagged 1.0 names carry
` [sic]`.

### Record functions

Documented on [records](../ss1/records.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlapiAttachRecord` | `N/record.attach` | options_object | 0 type, 1 id, 2 type2, 3 id2, 4 attributes | | |
| `nlapiCopyRecord` | `N/record.copy` | options_object | 0 type, 1 id, 2 initializeValues | | |
| `nlapiCreateCSVImport` | `N/task.create` | renamed | - | | The job object becomes `N/task.CsvImportTask` (see `nlobjCSVImport`). |
| `nlapiCreateCurrentLineItemSubrecord` | `N/record.Record#getCurrentSublistSubrecord`, `N/currentRecord.CurrentRecord#getCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | Create, edit and view all map to this getter. |
| `nlapiCreateEmailMerger` | `N/render.mergeEmail` | options_object | 0 templateId | | The merger's setters become `mergeEmail` parameters (see `nlobjEmailMerger`). |
| `nlapiCreateRecord` | `N/record.create` | options_object | 0 type, 1 initializeValues | | |
| `nlapiCreateSubrecord` | `N/record.Record#getSubrecord`, `N/currentRecord.CurrentRecord#getSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | Create, edit and view all map to this getter. |
| `nlapiDeleteRecord` | `N/record.delete` | options_object | 0 type, 1 id | | |
| `nlapiDetachRecord` | `N/record.detach` | options_object | 0 type, 1 id, 2 type2, 3 id2, 4 attributes | | |
| `nlapiEditCurrentLineItemSubrecord` | `N/record.Record#getCurrentSublistSubrecord`, `N/currentRecord.CurrentRecord#getCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | |
| `nlapiEditSubrecord` | `N/record.Record#getSubrecord`, `N/currentRecord.CurrentRecord#getSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlapiGetNewRecord` | `script:UserEventScript.beforeLoad`, `script:UserEventScript.beforeSubmit`, `script:UserEventScript.afterSubmit`, `script:UserEventScript` | approximate | - | | No function call: read `context.newRecord` inside the user event entry points. |
| `nlapiGetOldRecord` | `script:UserEventScript.beforeSubmit`, `script:UserEventScript.afterSubmit`, `script:UserEventScript` | approximate | - | | Read `context.oldRecord` in beforeSubmit or afterSubmit; the beforeLoad context has none. |
| `nlapiGetRecordId` | `N/record.Record#id`, `N/currentRecord.CurrentRecord#id` | pattern_change | - | `alternative_targets`, `multi_module` | Read the `id` property of the record object. |
| `nlapiGetRecordType` | `N/record.Record#type`, `N/currentRecord.CurrentRecord#type` | pattern_change | - | `alternative_targets`, `multi_module` | Read the `type` property of the current or loaded record. |
| `nlapiLoadRecord` | `N/record.load` | options_object | 0 type, 1 id, 2 initializeValues | | |
| `nlapiPrintRecord` | `N/render.bom`, `N/render.packingSlip`, `N/render.pickingTicket`, `N/render.statement`, `N/render.transaction` | split | 0 type, 1 id, 2 mode, 3 properties | | Pick the function matching the 1.0 `type` (document kind). |
| `nlapiRemoveCurrentLineItemSubrecord` | `N/record.Record#removeCurrentSublistSubrecord`, `N/currentRecord.CurrentRecord#removeCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | |
| `nlapiRemoveSubrecord` | `N/record.Record#removeSubrecord`, `N/currentRecord.CurrentRecord#removeSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlapiSubmitCSVImport` | `N/task` | approximate | 0 nlobjCSVImport | `module_level_only` | Build the import from `N/task`; the job object itself maps to `N/task.CsvImportTask`. |
| `nlapiSubmitRecord` | `N/record.Record#save` | options_object | 0 record, 1 doSourcing, 2 ignoreMandatoryFields | | Called on the record object that was the 1.0 `record` argument. |
| `nlapiTransformRecord` | `N/record.transform` | options_object | 0 type, 1 id, 2 transformType, 3 transformValues | | |
| `nlapiViewCurrentLineItemSubrecord` | `N/currentRecord.CurrentRecord#getCurrentSublistSubrecord`, `N/record.Record#getCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | |
| `nlapiViewLineItemSubrecord` | `N/record.Record#getSublistSubrecord` | options_object | 0 sublist, 1 fldname, 2 linenum | | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiViewSubrecord` | `N/record.Record#getSubrecord`, `N/currentRecord.CurrentRecord#getSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlapiVoidTransaction` | `N/transaction.void` | options_object | 0 transactionType, 1 recordId | | |

### Field and sublist functions

Documented on [fields and sublists](../ss1/fields-sublists.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlapiCancelLineItem` | `N/record.Record#cancelLine`, `N/currentRecord.CurrentRecord#cancelLine` | options_object | 0 type | `alternative_targets`, `multi_module` | |
| `nlapiCommitLineItem` | `N/record.Record#commitLine`, `N/currentRecord.CurrentRecord#commitLine` | options_object | 0 type | `alternative_targets`, `multi_module` | |
| `nlapiDisableField` | `N/currentRecord.Field#isDisabled` | pattern_change | 0 fldnam, 1 val | | `isDisabled` is a property: set it on the field object. |
| `nlapiDisableLineItemField` | `N/currentRecord.Field#isDisabled` | pattern_change | 0 type, 1 fldnam, 2 val | | Set `isDisabled` on the sublist field's object. |
| `nlapiFindLineItemMatrixValue` | `N/record.Record#findMatrixSublistLineWithValue`, `N/currentRecord.CurrentRecord#findMatrixSublistLineWithValue` | options_object | 0 type, 1 fldnam, 2 val, 3 column | `alternative_targets`, `multi_module` | |
| `nlapiFindLineItemValue` | `N/record.Record#findSublistLineWithValue`, `N/currentRecord.CurrentRecord#findSublistLineWithValue` | options_object | 0 type, 1 fldnam, 2 val | `alternative_targets`, `multi_module` | The line found is 0-based in 2.x. |
| `nlapiGetCurrentLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 timeZone | `module_level_only` | Read the value, then convert time zones with `N/format`. |
| `nlapiGetCurrentLineItemIndex` | `N/record.Record#getCurrentSublistIndex`, `N/currentRecord.CurrentRecord#getCurrentSublistIndex` | options_object | 0 type | `alternative_targets`, `multi_module` | The 2.x index is 0-based. |
| `nlapiGetCurrentLineItemMatrixValue` | `N/currentRecord.CurrentRecord#getCurrentMatrixSublistValue`, `N/record.Record#getCurrentMatrixSublistValue` | options_object | 0 type, 1 fldnam, 2 column | `alternative_targets`, `multi_module` | |
| `nlapiGetCurrentLineItemText` | `N/record.Record#getCurrentSublistText`, `N/currentRecord.CurrentRecord#getCurrentSublistText` | options_object | 0 type, 1 fldnam | `alternative_targets`, `multi_module` | |
| `nlapiGetCurrentLineItemValue` | `N/record.Record#getCurrentSublistValue`, `N/currentRecord.CurrentRecord#getCurrentSublistValue` | options_object | 0 type, 1 fldnam | `alternative_targets`, `multi_module` | |
| `nlapiGetCurrentLineItemValues` | `N/record.Record#getCurrentSublistValue`, `N/currentRecord.CurrentRecord#getCurrentSublistValue` | options_object | 0 type, 1 fldnam | `alternative_targets`, `multi_module` | Same getter as the single-value form. |
| `nlapiGetDateTimeValue` | `N/format` | approximate | 0 fieldId, 1 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlapiGetField` | `N/record.Record#getField`, `N/currentRecord.CurrentRecord#getField` | options_object | 0 fldnam | `alternative_targets`, `multi_module` | |
| `nlapiGetFieldText` | `N/record.Record#getText`, `N/currentRecord.CurrentRecord#getText` | options_object | 0 fldnam | `alternative_targets`, `multi_module` | |
| `nlapiGetFieldTexts` | `N/record.Record#getText`, `N/currentRecord.CurrentRecord#getText` | options_object | 0 fldnam | `alternative_targets`, `multi_module` | Same getter as the single-value form. |
| `nlapiGetFieldValue` | `N/record.Record#getValue`, `N/currentRecord.CurrentRecord#getValue` | options_object | 0 fldnam | `alternative_targets`, `multi_module` | |
| `nlapiGetFieldValues` | `N/record.Record#getValue`, `N/currentRecord.CurrentRecord#getValue` | options_object | 0 fldnam | `alternative_targets`, `multi_module` | Same getter as the single-value form. |
| `nlapiGetLineItemCount` | `N/record.Record#getLineCount`, `N/currentRecord.CurrentRecord#getLineCount` | options_object | 0 type | `alternative_targets`, `multi_module` | |
| `nlapiGetLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 lineNum, 3 timeZone | `module_level_only` | Line: 1-based in 1.0. The source's API map is flagged as naming the line argument differently; see the 1.0 page. |
| `nlapiGetLineItemField` | `N/record.Record#getSublistField`, `N/currentRecord.CurrentRecord#getSublistField` | options_object | 0 type, 1 fldnam, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. The source's API map is flagged as misspelling an argument; see the 1.0 page. |
| `nlapiGetLineItemMatrixField` | `N/record.Record#getMatrixSublistField`, `N/currentRecord.CurrentRecord#getMatrixSublistField` | options_object | 0 type, 1 fldnam, 2 linenum, 3 column | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiGetLineItemMatrixValue` | `N/record.Record#getMatrixSublistValue`, `N/currentRecord.CurrentRecord#getMatrixSublistValue` | options_object | 0 type, 1 fldnam, 2 linenum, 3 column | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiGetLineItemText` | `N/record.Record#getSublistText`, `N/currentRecord.CurrentRecord#getSublistText` | options_object | 0 type, 1 fldnam, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiGetLineItemValue` | `N/record.Record#getSublistValue`, `N/currentRecord.CurrentRecord#getSublistValue` | options_object | 0 type, 1 fldnam, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiGetLineItemValues` | `N/record.Record#getSublistValue`, `N/currentRecord.CurrentRecord#getSublistValue` | options_object | 0 type, 1 fldname [sic], 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. Returns an array for multi-select fields. The source spells this argument two ways (the 1.0 signature has `fldnam`); see the 1.0 page. |
| `nlapiGetMatrixCount` | `N/record.Record#getMatrixHeaderCount`, `N/currentRecord.CurrentRecord#getMatrixHeaderCount` | options_object | 0 type, 1 fldnam | `alternative_targets`, `multi_module` | |
| `nlapiGetMatrixField` | `N/record.Record#getMatrixHeaderField`, `N/currentRecord.CurrentRecord#getMatrixHeaderField` | options_object | 0 type, 1 fldnam, 2 column | `alternative_targets`, `multi_module` | |
| `nlapiGetMatrixValue` | `N/record.Record#getMatrixHeaderValue`, `N/currentRecord.CurrentRecord#getMatrixHeaderValue` | options_object | 0 type, 1 fldnam, 2 column | `alternative_targets`, `multi_module` | |
| `nlapiInsertLineItem` | `N/record.Record#insertLine`, `N/currentRecord.CurrentRecord#insertLine` | options_object | 0 type, 1 line | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiInsertLineItemOption` | `N/currentRecord.Field#insertSelectOption` | options_object | 0 type, 1 fldnam, 2 value, 3 text, 4 selected | | A method on the sublist field's object; the sublist and field IDs identify which field object to fetch. |
| `nlapiInsertSelectOption` | `N/currentRecord.Field#insertSelectOption` | options_object | 0 fldnam, 1 value, 2 text, 3 selected | | A method on the field object. |
| `nlapiIsLineItemChanged` | `N/record.Sublist#isChanged` | pattern_change | 0 type | | `isChanged` is a property of the sublist object. |
| `nlapiLookupField` | `N/search.lookupFields` | options_object | 0 type, 1 id, 2 fields, 3 text | | 2.x returns select fields as value/text arrays instead of taking a text flag. |
| `nlapiRefreshLineItems` | - | no_equivalent | 0 type | | No 2.x equivalent; the source states no alternative. |
| `nlapiRemoveLineItem` | `N/record.Record#removeLine`, `N/currentRecord.CurrentRecord#removeLine` | options_object | 0 type, 1 line | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiRemoveLineItemOption` | `N/currentRecord.Field#removeSelectOption` | options_object | 0 type, 1 fldnam, 2 value | | A method on the sublist field's object. |
| `nlapiRemoveSelectOption` | `N/currentRecord.Field#removeSelectOption` | options_object | 0 fldnam, 1 value | | A method on the field object. |
| `nlapiSelectLineItem` | `N/record.Record#selectLine`, `N/currentRecord.CurrentRecord#selectLine` | options_object | 0 type, 1 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiSelectNewLineItem` | `N/record.Record#selectNewLine`, `N/currentRecord.CurrentRecord#selectNewLine` | options_object | 0 type | `alternative_targets`, `multi_module` | |
| `nlapiSetCurrentLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 dateTime, 3 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlapiSetCurrentLineItemMatrixValue` | `N/record.Record#setCurrentMatrixSublistValue`, `N/currentRecord.CurrentRecord#setCurrentMatrixSublistValue` | options_object | 0 type, 1 fldnam, 2 column, 3 value, 4 firefieldchanged, 5 synchronous | `alternative_targets`, `multi_module` | `firefieldchanged` is the opposite sense of 2.x `ignoreFieldChange`. |
| `nlapiSetCurrentLineItemText` | `N/record.Record#setCurrentSublistText`, `N/currentRecord.CurrentRecord#setCurrentSublistText` | options_object | 0 type, 1 fldnam, 2 text, 3 firefieldchanged, 4 synchronous | `alternative_targets`, `multi_module` | `firefieldchanged` is the opposite sense of 2.x `ignoreFieldChange`. |
| `nlapiSetCurrentLineItemValue` | `N/record.Record#setCurrentSublistValue`, `N/currentRecord.CurrentRecord#setCurrentSublistValue` | options_object | 0 type, 1 fldnam, 2 value, 3 firefieldchanged, 4 synchronous | `alternative_targets`, `multi_module` | `firefieldchanged` is the opposite sense of 2.x `ignoreFieldChange`. |
| `nlapiSetCurrentLineItemValues` | `N/record.Record#setCurrentSublistValue`, `N/currentRecord.CurrentRecord#setCurrentSublistValue` | options_object | 0 type, 1 fldnam, 2 values, 3 firefieldchanged, 4 synchronous | `alternative_targets`, `multi_module` | Same setter as the single-value form. |
| `nlapiSetDateTimeValue` | `N/format` | approximate | 0 fieldId, 1 dateTime, 2 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlapiSetFieldText` | `N/record.Record#setText`, `N/currentRecord.CurrentRecord#setText` | options_object | 0 fldname, 1 txt, 2 firefieldchanged, 3 synchronous | `alternative_targets`, `multi_module` | |
| `nlapiSetFieldTexts` | `N/record.Record#setText`, `N/currentRecord.CurrentRecord#setText` | options_object | 0 fldname, 1 txts, 2 firefieldchanged, 3 synchronous | `alternative_targets`, `multi_module` | Same setter as the single-value form. |
| `nlapiSetFieldValue` | `N/record.Record#setValue`, `N/currentRecord.CurrentRecord#setValue` | options_object | 0 fldnam, 1 value, 2 firefieldchanged, 3 synchronous | `alternative_targets`, `multi_module` | `firefieldchanged` is the opposite sense of 2.x `ignoreFieldChange`. |
| `nlapiSetFieldValues` | `N/record.Record#setValue`, `N/currentRecord.CurrentRecord#setValue` | options_object | 0 fldnam, 1 value, 2 firefieldchanged, 3 synchronous | `alternative_targets`, `multi_module` | Same setter as the single-value form. |
| `nlapiSetLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 lineNum, 3 dateTime, 4 timeZone | `module_level_only` | Line: 1-based in 1.0. The source's API map is flagged as naming the line argument differently; see the 1.0 page. |
| `nlapiSetLineItemValue` | `N/record.Record#setSublistValue` | options_object | 0 type, 1 fldnam, 2 linenum, 3 value | | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlapiSetMatrixValue` | `N/record.Record#setMatrixHeaderValue`, `N/currentRecord.CurrentRecord#setMatrixHeaderValue` | options_object | 0 type, 1 fldnam, 2 column, 3 value, 4 firefieldchanged, 5 synchronous | `alternative_targets`, `multi_module` | The source's heading and parameter list give the arguments in different orders; check the 1.0 page before trusting the positions. |
| `nlapiSubmitField` | `N/record.submitFields` | options_object | 0 type, 1 id, 2 fields, 3 values, 4 doSourcing | | Field names and values merge into one `values` object. |

### Search, scheduling and context functions

Documented on [search, scheduling and context](../ss1/search-scheduling-context.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlapiCreateSearch` | `N/search.create` | options_object | 0 type, 1 filters, 2 columns | | |
| `nlapiGetContext` | `N/runtime.getCurrentScript`, `N/runtime.getCurrentSession`, `N/runtime.getCurrentUser` | split | - | | Pick the script, session or user object according to what the 1.0 code read from the context. |
| `nlapiGetDepartment` | `N/runtime.User#department` | pattern_change | - | | A property of the current user. |
| `nlapiGetLocation` | `N/runtime.User#location` | pattern_change | - | | `location` is a property of the current user. |
| `nlapiGetRole` | `N/runtime.User#role` | pattern_change | - | | A property of the current user. |
| `nlapiGetSubsidiary` | `N/runtime.User#subsidiary` | pattern_change | - | | A property of the current user. |
| `nlapiGetUser` | `N/runtime.getCurrentUser` | renamed | - | | Gives the whole `User` object; read its `id`. The 1.0 function returned a number. The source is flagged for mapping this to the user object rather than the ID; see the 1.0 page. |
| `nlapiLoadSearch` | `N/search.load` | options_object | 0 type, 1 id | | |
| `nlapiLogExecution` | `N/log.audit`, `N/log.debug`, `N/log.emergency`, `N/log.error` | split | 0 type, 1 title, 2 details | | The 1.0 `type` picks the function. |
| `nlapiScheduleScript` | `N/task.create` | options_object | 0 scriptId, 1 deployId, 2 params | | Create a scheduled-script task, then submit it. |
| `nlapiSearchDuplicate` | `N/search.duplicates` | options_object | 0 type, 1 fields, 2 id | | |
| `nlapiSearchGlobal` | `N/search.global` | options_object | 0 keywords | | |
| `nlapiSearchRecord` | `N/search.create`, `N/search.load` | split | 0 type, 1 id, 2 filters, 3 columns | | `create` for an ad hoc search, `load` for a saved search by ID. 1.0 returns `null` when nothing is found. |
| `nlapiSetRecoveryPoint` | `script:MapReduceScript` | approximate | - | | No equivalent in scheduled scripts; a map/reduce script yields automatically. |
| `nlapiYieldScript` | `script:MapReduceScript` | approximate | - | | No equivalent in scheduled scripts; a map/reduce script yields automatically. |

### UI and HTTP functions

Documented on [UI and HTTP](../ss1/ui-http.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlapiCreateAssistant` | `N/ui/serverWidget.createAssistant` | options_object | 0 title, 1 hideHeader | | |
| `nlapiCreateForm` | `N/ui/serverWidget.createForm` | options_object | 0 title, 1 hideNavbar | | |
| `nlapiCreateList` | `N/ui/serverWidget.createList` | options_object | 0 title, 1 hideNavbar | | |
| `nlapiCreateTemplateRenderer` | `N/render.create` | renamed | - | | |
| `nlapiRefreshPortlet` | `N/portlet.refresh` | renamed | - | | |
| `nlapiRequestURL` | `N/http.delete`, `N/http.get`, `N/http.post`, `N/http.put`, `N/http.request` | split | 0 url, 1 postdata, 2 headers, 3 callback, 4 httpMethod | | The HTTP method picks the function. In 1.0 server scripts there is no `callback`, so the method is the fourth argument. |
| `nlapiRequestURLWithCredentials` | `N/https.request` | options_object | 0 credentials, 1 url, 2 postdata, 3 headers, 4 httpMethod [sic] | | 1.0 allows it only in user event, scheduled, portlet and Suitelet scripts. The source names the method argument `httpsMethod` in its 1.0 reference and `httpMethod` in its API map; see the 1.0 page. |
| `nlapiResizePortlet` | `N/portlet.resize` | renamed | - | | |
| `nlapiResolveURL` | `N/url.resolveRecord`, `N/url.resolveScript`, `N/url.resolveTaskLink` | split | 0 type, 1 identifier, 2 id, 3 displayMode | | The 1.0 `type` picks the function. |
| `nlapiSetRedirectURL` | `N/redirect.redirect`, `N/redirect.toRecord`, `N/redirect.toSuitelet`, `N/redirect.toTaskLink` | split | 0 type, 1 identifier, 2 id, 3 editmode, 4 parameters | | The 1.0 `type` picks the function. |

### Utility functions

Documented on [utilities](../ss1/utilities.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlapiOutboundSSO` | - | no_equivalent | 0 id | `feature_retired` | The feature is retired; nothing replaces it. In 1.0 it required "SuiteSignOn" and "web services" [sic]; the second feature name is flagged, see the 1.0 page. |
| `nlapiAddDays` | - | no_equivalent | 0 d, 1 days | `use_native_date_setdate` | Use `Date.setDate(getDate() + n)` (or minus), or a third-party date library. |
| `nlapiAddMonths` | - | no_equivalent | 0 d, 1 months | `use_native_date_setmonth` | Use `Date.setMonth(getMonth() + n)` (or minus), or a third-party date library. |
| `nlapiCheckPasswordField` | `N/crypto.checkPasswordField` | options_object | 0 type, 1 id, 2 value, 3 field, 4 sublist, 5 line | | The 1.0 signature is not verified in the spec; these argument names come from the mapping only. |
| `nlapiCreateError` | `N/error.create` | options_object | 0 code, 1 details, 2 suppressNotification | | |
| `nlapiCreateFile` | `N/file.create` | options_object | 0 name, 1 type, 2 contents | | |
| `nlapiDateToString` | `N/format.format` | options_object | 0 d, 1 format | | |
| `nlapiDeleteFile` | `N/file.delete` | options_object | 0 id | | |
| `nlapiEncrypt` | `N/crypto`, `N/encode` | approximate | 0 s, 1 algorithm, 2 key | `multi_module` | Rebuild with `N/crypto` together with `N/encode`. |
| `nlapiEscapeXML` | `N/xml.escape` | options_object | 0 text | | |
| `nlapiExchangeRate` | `N/currency.exchangeRate` | options_object | 0 sourceCurrency, 1 targetCurrency, 2 effectiveDate | | |
| `nlapiFormatCurrency` | `N/format.format` | options_object | 0 str | | Currency formatting lives in `N/format`, not `N/currency`. |
| `nlapiGetJobManager` | `N/task.create` | options_object | 0 jobType | | |
| `nlapiGetLogin` | `N/auth.changeEmail`, `N/auth.changePassword` | split | - | | Call the `N/auth` function for the change you need. |
| `nlapiInitiateWorkflow` | `N/workflow.initiate` | options_object | 0 recordtype, 1 id, 2 workflowid, 3 initialvalues | | |
| `nlapiInitiateWorkflowAsync` | `N/task.WorkflowTriggerTask` | pattern_change | 0 recordtype, 1 id, 2 workflowid, 3 initialValues | | Build and submit a workflow trigger task. |
| `nlapiLoadConfiguration` | `N/config.load` | options_object | 0 type | | Returns a record object. |
| `nlapiLoadFile` | `N/file.load` | options_object | 0 id | | |
| `nlapiSelectNode` | `N/xml.XPath#select` | options_object | 0 node, 1 xpath | | |
| `nlapiSelectNodes` | `N/xml.XPath#select` | options_object | 0 node, 1 xpath | | |
| `nlapiSelectValue` | `N/xml.XPath#select`, `N/xml.Node#textContent` | approximate | 0 node, 1 xpath | | Select the node with XPath, then read its `textContent`. |
| `nlapiSelectValues` | `N/xml.XPath#select`, `N/xml.Node#textContent` | approximate | 0 node, 1 path | | Select the nodes with XPath, then loop over them reading `textContent`. |
| `nlapiSendCampaignEmail` | `N/email.sendCampaignEvent` | options_object | 0 campaigneventid, 1 recipientid | | |
| `nlapiSendEmail` | `N/email.send`, `N/email.sendBulk` | split | 0 author, 1 recipient, 2 subject, 3 body, 4 cc, 5 bcc, 6 records, 7 attachments | | The mapping lists no slots for the last three 1.0 arguments (`notifySenderOnBounce`, `internalOnly`, `replyTo`). |
| `nlapiSendFax` | - | no_equivalent | 0 author, 1 recipient, 2 subject, 3 body, 4 records, 5 attachments | | No 2.x equivalent; the source states no alternative. |
| `nlapiStringToDate` | `N/format.parse` | options_object | 0 str, 1 format | | |
| `nlapiStringToXML` | `N/xml.Parser#fromString` | options_object | 0 text | | |
| `nlapiSubmitConfiguration` | `N/record.Record#save` | options_object | 0 name | | Save the record that `config.load` returned. |
| `nlapiSubmitFile` | `N/file.File#save` | renamed | 0 file | | A method on the file object itself. |
| `nlapiTriggerWorkflow` | `N/workflow.trigger` | options_object | 0 recordtype, 1 id, 2 workflowid, 3 actionid, 4 stateid [sic] | | The source names the last argument differently in different places (the 1.0 signature has `workflowstateid`); see the 1.0 page. |
| `nlapiValidateXML` | `N/xml.validate` | options_object | 0 xmlDocument, 1 schemaDocument, 2 schemaFolderId | | |
| `nlapiXMLToPDF` | `N/render.xmlToPdf`, `N/render.TemplateRenderer#renderAsPdf` | split | 0 xmlstring | | The source equates `TemplateRenderer.renderAsPdf` with running `xmlToPdf` on the merged string. |
| `nlapiXMLToString` | `N/xml.Parser#toString` | options_object | 0 xml | | |

### Form objects

Documented on [form objects](../ss1/objects-forms.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlobjButton` | `N/ui/serverWidget.Button` | equivalent | - | | |
| `nlobjButton#setDisabled` | `N/ui/serverWidget.Button#isDisabled` | pattern_change | 0 disabled | `setter_to_property` | |
| `nlobjButton#setLabel` | `N/ui/serverWidget.Button#label` | pattern_change | 0 label | `setter_to_property` | |
| `nlobjButton#setVisible` | `N/ui/serverWidget.Button#isHidden` | pattern_change | 0 visible | `setter_to_property` | The property name reads as the opposite of the 1.0 argument; the source does not state the conversion. |
| `nlobjColumn` | `N/ui/serverWidget.ListColumn` | renamed | - | | |
| `nlobjColumn#addParamToURL` | `N/ui/serverWidget.ListColumn#addParamToURL` | options_object | 0 param, 1 value, 2 dynamic | | |
| `nlobjColumn#setLabel` | `N/ui/serverWidget.ListColumn#label` | pattern_change | 0 label | `setter_to_property` | |
| `nlobjColumn#setURL` | `N/ui/serverWidget.ListColumn#setURL` | options_object | 0 url, 1 dynamic | | |
| `nlobjField` | `N/ui/serverWidget.Field`, `N/record.Field` | equivalent | - | `alternative_targets`, `multi_module` | `serverWidget.Field` for Suitelet pages, `record.Field` for record field metadata. |
| `nlobjFieldGroup` | `N/ui/serverWidget.FieldGroup` | equivalent | - | | |
| `nlobjFieldGroup#setCollapsible` | `N/ui/serverWidget.FieldGroup#isCollapsible` | pattern_change | 0 collapsible, 1 hidden | `setter_to_property` | |
| `nlobjFieldGroup#setLabel` | `N/ui/serverWidget.FieldGroup#label` | pattern_change | 0 label | `setter_to_property` | |
| `nlobjFieldGroup#setShowBorder` | `N/ui/serverWidget.FieldGroup#isBorderHidden` | pattern_change | 0 show | `setter_to_property` | The property name reads as the opposite of the 1.0 argument; the source does not state the conversion. |
| `nlobjFieldGroup#setSingleColumn` | `N/ui/serverWidget.FieldGroup#isSingleColumn` | pattern_change | 0 column | `setter_to_property` | |
| `nlobjForm` | `N/ui/serverWidget.Form` | equivalent | - | | |
| `nlobjForm#addButton` | `N/ui/serverWidget.Form#addButton` | options_object | 0 name, 1 label, 2 script | | |
| `nlobjForm#addCredentialField` | `N/ui/serverWidget.Form#addCredentialField` | options_object | 0 id, 1 label, 2 website, 3 scriptId, 4 value, 5 entityMatch, 6 tab | | |
| `nlobjForm#addField` | `N/ui/serverWidget.Form#addField` | options_object | 0 name, 1 type, 2 label, 3 sourceOrRadio, 4 tab | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjForm#addFieldGroup` | `N/ui/serverWidget.Form#addFieldGroup` | options_object | 0 name, 1 label, 2 tab | | |
| `nlobjForm#addPageLink` | `N/ui/serverWidget.Form#addPageLink` | options_object | 0 type, 1 title, 2 url | | |
| `nlobjForm#addSubList` | `N/ui/serverWidget.Form#addSublist` | options_object | 0 name, 1 type, 2 label, 3 tab | | |
| `nlobjForm#addSubmitButton` | `N/ui/serverWidget.Form#addSubmitButton` | options_object | 0 label | | |
| `nlobjForm#addSubTab` | `N/ui/serverWidget.Form#addSubtab` | options_object | 0 name, 1 label, 2 tab | | |
| `nlobjForm#addTab` | `N/ui/serverWidget.Form#addTab` | options_object | 0 name, 1 label | | |
| `nlobjForm#getButton` | `N/ui/serverWidget.Form#getButton` | options_object | 0 name | | |
| `nlobjForm#getField` | `N/ui/serverWidget.Form#getField` | options_object | 0 name, 1 radio | | |
| `nlobjForm#getSubList` | `N/ui/serverWidget.Form#getSublist` | options_object | 0 name | | |
| `nlobjForm#getSubTab` | `N/ui/serverWidget.Form#getSubtab` | options_object | 0 name | | |
| `nlobjForm#getTab` | `N/ui/serverWidget.Form#getTab` | options_object | 0 name | | |
| `nlobjForm#getTabs` | `N/ui/serverWidget.Form#getTabs` | equivalent | - | | |
| `nlobjForm#insertField` | `N/ui/serverWidget.Form#insertField` | options_object | 0 field, 1 nextfld | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjForm#insertSubList` | `N/ui/serverWidget.Form#insertSublist` | options_object | 0 sublist, 1 nextsub | | |
| `nlobjForm#insertSubTab` | `N/ui/serverWidget.Form#insertSubtab` | options_object | 0 subtab, 1 nextsub | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjForm#insertTab` | `N/ui/serverWidget.Form#insertTab` | options_object | 0 tab, 1 nexttab | | |
| `nlobjForm#removeButton` | `N/ui/serverWidget.Form#removeButton` | options_object | 0 name | | |
| `nlobjForm#setFieldValues` | `N/ui/serverWidget.Form#updateDefaultValues` | options_object | 0 values | | |
| `nlobjForm#setScript` | `N/ui/serverWidget.Form#clientScriptFileId`, `N/ui/serverWidget.Form#clientScriptModulePath` | split | 0 script | | Set one of the two properties; `clientScriptFileId` names a file, not a script record. |
| `nlobjForm#setTitle` | `N/ui/serverWidget.Form#title` | pattern_change | 0 title | `setter_to_property` | |
| `nlobjSubList` | `N/ui/serverWidget.Sublist` | equivalent | - | | The source writes the 1.0 object name inconsistently, including on its API map page; see the 1.0 page. |
| `nlobjSubList#addButton` | `N/ui/serverWidget.Sublist#addButton` | options_object | 0 name, 1 label, 2 script | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjSubList#addField` | `N/ui/serverWidget.Sublist#addField` | options_object | 0 name, 1 type, 2 label, 3 source | | |
| `nlobjSubList#addMarkAllButtons` | `N/ui/serverWidget.Sublist#addMarkAllButtons` | equivalent | - | | |
| `nlobjSubList#addRefreshButton` | `N/ui/serverWidget.Sublist#addRefreshButton` | equivalent | - | | |
| `nlobjSubList#getLineItemCount` | `N/ui/serverWidget.Sublist#lineCount` | pattern_change | - | `getter_to_property` | |
| `nlobjSubList#getLineItemValue` | `N/ui/serverWidget.Sublist#getSublistValue` | options_object | 0 group, 1 fldnam, 2 linenum | | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjSubList#setAmountField` | `N/ui/serverWidget.Sublist#updateTotallingFieldId` | options_object | 0 field | | |
| `nlobjSubList#setDisplayType` | `N/ui/serverWidget.Sublist#displayType` | pattern_change | 0 type | `setter_to_property` | |
| `nlobjSubList#setHelpText` | `N/ui/serverWidget.Sublist#helpText` | pattern_change | 0 help | `setter_to_property` | |
| `nlobjSubList#setLabel` | `N/ui/serverWidget.Sublist#label` | pattern_change | 0 label | `setter_to_property` | |
| `nlobjSubList#setLineItemValue` | `N/ui/serverWidget.Sublist#setSublistValue` | options_object | 0 name, 1 linenum, 2 value | | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjSubList#setLineItemValues` | - | no_equivalent | 0 values | | No equivalent; set values one at a time with `N/ui/serverWidget.Sublist#setSublistValue`. |
| `nlobjSubList#setUniqueField` | `N/ui/serverWidget.Sublist#updateUniqueFieldId` | options_object | 0 name | | The target is a method; the source's own map note is flagged for calling it a property. |
| `nlobjTab` | `N/ui/serverWidget.Tab` | equivalent | - | | |
| `nlobjTab#setLabel` | `N/ui/serverWidget.Tab#label` | pattern_change | 0 label | `setter_to_property` | |
| `nlobjTab#setHelpText` | `N/ui/serverWidget.Tab#helpText` | pattern_change | 0 help | `setter_to_property` | |

### Other UI objects

Documented on [other UI objects](../ss1/objects-ui-other.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlobjAssistant` | `N/ui/serverWidget.Assistant` | equivalent | - | | |
| `nlobjAssistant#addField` | `N/ui/serverWidget.Assistant#addField` | options_object | 0 name, 1 type, 2 label, 3 source, 4 group | | |
| `nlobjAssistant#addFieldGroup` | `N/ui/serverWidget.Assistant#addFieldGroup` | options_object | 0 name, 1 label | | |
| `nlobjAssistant#addStep` | `N/ui/serverWidget.Assistant#addStep` | options_object | 0 name, 1 label | | |
| `nlobjAssistant#addSubList` | `N/ui/serverWidget.Assistant#addSublist` | options_object | 0 name, 1 type, 2 label | | |
| `nlobjAssistant#getAllFields` | `N/ui/serverWidget.Assistant#getFieldIds` | renamed | - | | |
| `nlobjAssistant#getAllFieldGroups` | `N/ui/serverWidget.Assistant#getFieldGroupIds` | renamed | - | | |
| `nlobjAssistant#getAllSteps` | `N/ui/serverWidget.Assistant#getSteps` | renamed | - | | |
| `nlobjAssistant#getAllSubLists` | `N/ui/serverWidget.Assistant#getSublistIds` | renamed | - | | |
| `nlobjAssistant#getCurrentStep` | `N/ui/serverWidget.Assistant#currentStep` | pattern_change | - | `getter_to_property` | |
| `nlobjAssistant#getField` | `N/ui/serverWidget.Assistant#getField` | options_object | 0 name | | |
| `nlobjAssistant#getFieldGroup` | `N/ui/serverWidget.Assistant#getFieldGroup` | options_object | 0 name | | |
| `nlobjAssistant#getLastAction` | `N/ui/serverWidget.Assistant#getLastAction` | equivalent | - | | |
| `nlobjAssistant#getLastStep` | `N/ui/serverWidget.Assistant#getLastStep` | equivalent | - | | |
| `nlobjAssistant#getNextStep` | `N/ui/serverWidget.Assistant#getNextStep` | equivalent | - | | |
| `nlobjAssistant#getStep` | `N/ui/serverWidget.Assistant#getStep` | options_object | 0 name | | |
| `nlobjAssistant#getSubList` | `N/ui/serverWidget.Assistant#getSublist` | options_object | 0 name | | |
| `nlobjAssistant#hasError` | `N/ui/serverWidget.Assistant#hasErrorHtml` | renamed | - | | |
| `nlobjAssistant#isFinished` | `N/ui/serverWidget.Assistant#isFinished` | equivalent | - | | |
| `nlobjAssistant#sendRedirect` | `N/ui/serverWidget.Assistant#sendRedirect` | options_object | 0 response | | |
| `nlobjAssistant#setCurrentStep` | `N/ui/serverWidget.Assistant#currentStep` | pattern_change | 0 step | `setter_to_property` | |
| `nlobjAssistant#setError` | `N/ui/serverWidget.Assistant#errorHtml` | pattern_change | 0 html | `setter_to_property` | |
| `nlobjAssistant#setFieldValues` | `N/ui/serverWidget.Assistant#updateDefaultValues` | renamed | 0 values | | |
| `nlobjAssistant#setFinished` | `N/ui/serverWidget.Assistant#finishedHtml` | pattern_change | 0 html | `setter_to_property` | |
| `nlobjAssistant#setNumbered` | `N/ui/serverWidget.Assistant#hideStepNumber` | pattern_change | 0 hasStepNumber | `setter_to_property` | The property name reads as the opposite of the 1.0 argument; the source does not state the conversion. |
| `nlobjAssistant#setOrdered` | `N/ui/serverWidget.Assistant#isNotOrdered` | pattern_change | 0 ordered | `setter_to_property` | The property name reads as the opposite of the 1.0 argument; the source does not state the conversion. |
| `nlobjAssistant#setScript` | `N/ui/serverWidget.Assistant#clientScriptFileId`, `N/ui/serverWidget.Assistant#clientScriptModulePath` | split | 0 script | | Set one of the two properties; `clientScriptFileId` names a file, not a script record. |
| `nlobjAssistant#setShortcut` | `N/ui/serverWidget.Assistant#hideAddToShortcutsLink` | pattern_change | 0 show | `setter_to_property` | The property name reads as the opposite of the 1.0 argument; the source does not state the conversion. |
| `nlobjAssistant#setSplash` | `N/ui/serverWidget.Assistant#setSplash` | options_object | 0 title, 1 text1, 2 text2 | | |
| `nlobjAssistant#setTitle` | `N/ui/serverWidget.Assistant#title` | pattern_change | 0 title | `setter_to_property` | |
| `nlobjAssistantStep` | `N/ui/serverWidget.AssistantStep` | equivalent | - | | |
| `nlobjAssistantStep#getAllFields` | `N/ui/serverWidget.AssistantStep#getFieldIds` | renamed | - | | |
| `nlobjAssistantStep#getAllLineItemFields` | `N/ui/serverWidget.AssistantStep#getSublistFieldIds` | options_object | 0 group | | |
| `nlobjAssistantStep#getAllLineItems` | `N/ui/serverWidget.AssistantStep#getSubmittedSublistIds` | renamed | - | | |
| `nlobjAssistantStep#getFieldValue` | `N/ui/serverWidget.AssistantStep#getValue` | options_object | 0 name | | |
| `nlobjAssistantStep#getFieldValues` | `N/ui/serverWidget.AssistantStep#getValue` | options_object | 0 name | | Same getter as the single-value form. |
| `nlobjAssistantStep#getLineItemCount` | `N/ui/serverWidget.AssistantStep#getLineCount` | options_object | 0 group | | |
| `nlobjAssistantStep#getLineItemValue` | `N/ui/serverWidget.AssistantStep#getSublistValue` | options_object | 0 group, 1 name, 2 line | | Line: 1-based in 1.0, 0-based in 2.x. The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjAssistantStep#getStepNumber` | `N/ui/serverWidget.AssistantStep#stepNumber` | pattern_change | - | `getter_to_property` | |
| `nlobjAssistantStep#setHelpText` | `N/ui/serverWidget.AssistantStep#helpText` | pattern_change | 0 help | `setter_to_property` | |
| `nlobjAssistantStep#setLabel` | `N/ui/serverWidget.AssistantStep#label` | pattern_change | 0 label | `setter_to_property` | |
| `nlobjList` | `N/ui/serverWidget.List` | equivalent | - | | |
| `nlobjList#addButton` | `N/ui/serverWidget.List#addButton` | options_object | 0 name, 1 label, 2 script | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjList#addColumn` | `N/ui/serverWidget.List#addColumn` | options_object | 0 name, 1 type, 2 label, 3 align | | |
| `nlobjList#addEditColumn` | `N/ui/serverWidget.List#addEditColumn` | options_object | 0 column, 1 showView, 2 showHrefCol | | The source types `showHrefCol` as boolean here but as string on `nlobjPortlet`; see the 1.0 page. |
| `nlobjList#addPageLink` | `N/ui/serverWidget.List#addPageLink` | options_object | 0 type, 1 title, 2 url | | |
| `nlobjList#addRow` | `N/ui/serverWidget.List#addRow` | options_object | 0 row | | |
| `nlobjList#addRows` | `N/ui/serverWidget.List#addRows` | options_object | 0 rows | | |
| `nlobjList#setScript` | `N/ui/serverWidget.List#clientScriptFileId`, `N/ui/serverWidget.List#clientScriptModulePath` | split | 0 script | | Set one of the two properties; `clientScriptFileId` names a file, not a script record. |
| `nlobjList#setStyle` | `N/ui/serverWidget.List#style` | pattern_change | 0 style | `setter_to_property` | |
| `nlobjList#setTitle` | `N/ui/serverWidget.List#title` | pattern_change | 0 title | `setter_to_property` | |
| `nlobjPortlet` | `script:Portlet` | approximate | - | `target_is_script_object` | Use the portlet object passed to `render` as `params.portlet`. |
| `nlobjPortlet#addColumn` | `script:Portlet.Portlet#addColumn` | options_object | 0 name, 1 type, 2 label, 3 just | | `name` goes to `options.id`, `just` to `options.align`. |
| `nlobjPortlet#addEditColumn` | `script:Portlet.Portlet#addEditColumn` | options_object | 0 column, 1 showView, 2 showHrefCol | | 2.x takes a column ID, not a column object. |
| `nlobjPortlet#addField` | `script:Portlet.Portlet#addField` | options_object | 0 name, 1 type, 2 label, 3 source | | `name` goes to `options.id`. |
| `nlobjPortlet#addLine` | `script:Portlet.Portlet#addLine` | options_object | 0 text, 1 url, 2 indent | | `indent` goes to `options.align`, which takes 0 to 5. |
| `nlobjPortlet#addRow` | `script:Portlet.Portlet#addRow` | options_object | 0 row | | |
| `nlobjPortlet#addRows` | `script:Portlet.Portlet#addRows` | options_object | 0 rows | | |
| `nlobjPortlet#setHtml` | `script:Portlet.Portlet#html` | pattern_change | 0 html | `setter_to_property` | |
| `nlobjPortlet#setRefreshInterval` | - | no_equivalent | 0 n | | Not supported in 2.x; the source states no alternative. |
| `nlobjPortlet#setScript` | `script:Portlet.Portlet#clientScriptFileId`, `script:Portlet.Portlet#clientScriptModulePath` | split | 0 scriptid | | Set one of the two properties; `clientScriptFileId` names a file, not a script record. |
| `nlobjPortlet#setSubmitButton` | `script:Portlet.Portlet#setSubmitButton` | options_object | 0 url, 1 label, 2 target | | |
| `nlobjPortlet#setTitle` | `script:Portlet.Portlet#title` | pattern_change | 0 title | `setter_to_property` | |
| `nlobjTemplateRenderer` | `N/render.TemplateRenderer` | equivalent | - | | |
| `nlobjTemplateRenderer#addRecord` | `N/render.TemplateRenderer#addRecord` | options_object | 0 var, 1 record | | |
| `nlobjTemplateRenderer#addSearchResults` | `N/render.TemplateRenderer#addSearchResults` | options_object | 0 var, 1 searchResult | | |
| `nlobjTemplateRenderer#renderToResponse` | `N/render.TemplateRenderer#renderToResponse` | equivalent | - | | The source's 1.0 reference lists no parameters, yet its examples pass a response object; see the 1.0 page. |
| `nlobjTemplateRenderer#renderToString` | `N/render.TemplateRenderer#renderAsString` | renamed | - | | |
| `nlobjTemplateRenderer#setTemplate` | `N/render.TemplateRenderer#templateContent` | pattern_change | 0 template | `setter_to_property` | The source's API map row for this method is flagged; see the 1.0 page. |

### Record and search objects

Documented on [record and search objects](../ss1/objects-record-search.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlobjRecord` | `N/record.Record`, `N/currentRecord.CurrentRecord` | split | - | `multi_module` | `CurrentRecord` for the record open in a client script, `Record` elsewhere. |
| `nlobjRecord#commitLineItem` | `N/record.Record#commitLine`, `N/currentRecord.CurrentRecord#commitLine` | options_object | 0 group | `alternative_targets`, `multi_module` | |
| `nlobjRecord#createCurrentLineItemSubrecord` | `N/record.Record#getCurrentSublistSubrecord`, `N/currentRecord.CurrentRecord#getCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | |
| `nlobjRecord#createSubrecord` | `N/record.Record#getSubrecord`, `N/currentRecord.CurrentRecord#getSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlobjRecord#editCurrentLineItemSubrecord` | `N/record.Record#getCurrentSublistSubrecord`, `N/currentRecord.CurrentRecord#getCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | |
| `nlobjRecord#editSubrecord` | `N/record.Record#getSubrecord`, `N/currentRecord.CurrentRecord#getSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlobjRecord#findLineItemMatrixValue` | `N/record.Record#findMatrixSublistLineWithValue`, `N/currentRecord.CurrentRecord#findMatrixSublistLineWithValue` | options_object | 0 group, 1 fldnam, 2 column, 3 val | `alternative_targets`, `multi_module` | |
| `nlobjRecord#findLineItemValue` | `N/record.Record#findSublistLineWithValue`, `N/currentRecord.CurrentRecord#findSublistLineWithValue` | options_object | 0 group, 1 fldnam, 2 value | `alternative_targets`, `multi_module` | The line found is 0-based in 2.x. |
| `nlobjRecord#getAllFields` | `N/record.Record#getFields` | renamed | - | | |
| `nlobjRecord#getAllLineItemFields` | `N/record.Record#getSublistFields` | options_object | 0 group | | |
| `nlobjRecord#getCurrentLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlobjRecord#getCurrentLineItemMatrixValue` | `N/record.Record#getCurrentMatrixSublistValue`, `N/currentRecord.CurrentRecord#getCurrentMatrixSublistValue` | options_object | 0 group, 1 fldnam, 2 column | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getCurrentLineItemValue` | `N/record.Record#getCurrentSublistValue`, `N/currentRecord.CurrentRecord#getCurrentSublistValue` | options_object | 0 type, 1 fldnam | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getCurrentLineItemValues` | `N/record.Record#getCurrentSublistValue`, `N/currentRecord.CurrentRecord#getCurrentSublistValue` | options_object | 0 type, 1 fldname | `alternative_targets`, `multi_module` | Same getter as the single-value form. |
| `nlobjRecord#getDateTimeValue` | `N/format` | approximate | 0 fieldId, 1 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlobjRecord#getField` | `N/record.Record#getField`, `N/currentRecord.CurrentRecord#getField` | options_object | 0 fldnam | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getFieldText` | `N/record.Record#getText`, `N/currentRecord.CurrentRecord#getText` | options_object | 0 name | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getFieldTexts` | `N/record.Record#getText`, `N/currentRecord.CurrentRecord#getText` | options_object | 0 name | `alternative_targets`, `multi_module` | Same getter as the single-value form. |
| `nlobjRecord#getFieldValue` | `N/record.Record#getValue`, `N/currentRecord.CurrentRecord#getValue` | options_object | 0 name | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getFieldValues` | `N/record.Record#getValue`, `N/currentRecord.CurrentRecord#getValue` | options_object | 0 name | `alternative_targets`, `multi_module` | Same getter as the single-value form. |
| `nlobjRecord#getId` | `N/record.Record#id` | pattern_change | - | `getter_to_property` | |
| `nlobjRecord#getLineItemCount` | `N/record.Record#getLineCount` | renamed | 0 group | | The source's API map row for this method is flagged; see the 1.0 page. |
| `nlobjRecord#getLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 lineNum, 3 timeZone | `module_level_only` | Line: 1-based in 1.0. Time-zone conversion with `N/format`. |
| `nlobjRecord#getLineItemField` | `N/record.Record#getSublistField`, `N/currentRecord.CurrentRecord#getSublistField` | options_object | 0 group, 1 fldnam, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#getLineItemMatrixField` | `N/record.Record#getMatrixSublistField`, `N/currentRecord.CurrentRecord#getMatrixSublistField` | options_object | 0 group, 1 fldnam, 2 linenum, 3 column | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#getLineItemMatrixValue` | `N/record.Record#getMatrixSublistValue`, `N/currentRecord.CurrentRecord#getMatrixSublistValue` | options_object | 0 group, 1 fldnam, 2 lineum [sic], 3 column | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjRecord#getLineItemText` | `N/record.Record#getSublistText`, `N/currentRecord.CurrentRecord#getSublistText` | options_object | 0 group, 1 fldnam, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#getLineItemValue` | `N/record.Record#getSublistValue`, `N/currentRecord.CurrentRecord#getSublistValue` | options_object | 0 group, 1 name, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#getLineItemValues` | `N/record.Record#getSublistValue`, `N/currentRecord.CurrentRecord#getSublistValue` | options_object | 0 type, 1 fldname, 2 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#getMatrixCount` | `N/record.Record#getMatrixHeaderCount`, `N/currentRecord.CurrentRecord#getMatrixHeaderCount` | options_object | 0 group, 1 fldnam | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getMatrixField` | `N/record.Record#getMatrixHeaderField`, `N/currentRecord.CurrentRecord#getMatrixHeaderField` | options_object | 0 group, 1 fldname, 2 column | `alternative_targets`, `multi_module` | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjRecord#getMatrixValue` | `N/record.Record#getMatrixHeaderValue`, `N/currentRecord.CurrentRecord#getMatrixHeaderValue` | options_object | 0 group, 1 fldnam, 2 column | `alternative_targets`, `multi_module` | |
| `nlobjRecord#getRecordType` | `N/record.Record#type` | pattern_change | - | `getter_to_property` | |
| `nlobjRecord#insertLineItem` | `N/record.Record#insertLine`, `N/currentRecord.CurrentRecord#insertLine` | options_object | 0 group, 1 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#removeLineItem` | `N/record.Record#removeLine`, `N/currentRecord.CurrentRecord#removeLine` | options_object | 0 group, 1 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#removeCurrentLineItemSubrecord` | `N/record.Record#removeCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | | |
| `nlobjRecord#removeSubrecord` | `N/record.Record#removeSubrecord`, `N/currentRecord.CurrentRecord#removeSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlobjRecord#selectLineItem` | `N/record.Record#selectLine`, `N/currentRecord.CurrentRecord#selectLine` | options_object | 0 group, 1 linenum | `alternative_targets`, `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#selectNewLineItem` | `N/record.Record#selectNewLine`, `N/currentRecord.CurrentRecord#selectNewLine` | options_object | 0 group | `alternative_targets`, `multi_module` | |
| `nlobjRecord#setCurrentLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 dateTime, 3 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlobjRecord#setCurrentLineItemMatrixValue` | `N/record.Record#setCurrentMatrixSublistValue`, `N/currentRecord.CurrentRecord#setCurrentMatrixSublistValue` | options_object | 0 group, 1 fldnam, 2 column, 3 value | `alternative_targets`, `multi_module` | |
| `nlobjRecord#setCurrentLineItemValue` | `N/record.Record#setCurrentSublistValue`, `N/currentRecord.CurrentRecord#setCurrentSublistValue` | options_object | 0 group, 1 name, 2 value | `alternative_targets`, `multi_module` | |
| `nlobjRecord#setDateTimeValue` | `N/format` | approximate | 0 fieldId, 1 dateTime, 2 timeZone | `module_level_only` | Time-zone conversion with `N/format`. |
| `nlobjRecord#setFieldText` | `N/record.Record#setText`, `N/currentRecord.CurrentRecord#setText` | options_object | 0 name, 1 text | `alternative_targets`, `multi_module` | |
| `nlobjRecord#setFieldTexts` | `N/record.Record#setText`, `N/currentRecord.CurrentRecord#setText` | options_object | 0 name, 1 text | `alternative_targets`, `multi_module` | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjRecord#setFieldValue` | `N/record.Record#setValue`, `N/currentRecord.CurrentRecord#setValue` | options_object | 0 name, 1 value | `alternative_targets`, `multi_module` | |
| `nlobjRecord#setFieldValues` | `N/record.Record#setValue`, `N/currentRecord.CurrentRecord#setValue` | options_object | 0 name, 1 value | `alternative_targets`, `multi_module` | Same setter as the single-value form. |
| `nlobjRecord#setLineItemDateTimeValue` | `N/format` | approximate | 0 type, 1 fieldId, 2 lineNum, 3 dateTime, 4 timeZone | `module_level_only` | Line: 1-based in 1.0. Time-zone conversion with `N/format`. |
| `nlobjRecord#setLineItemValue` | `N/record.Record#setSublistValue` | options_object | 0 group, 1 name, 2 linenum, 3 value | | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#setMatrixValue` | `N/record.Record#setMatrixHeaderValue`, `N/currentRecord.CurrentRecord#setMatrixHeaderValue` | options_object | 0 group, 1 fldnam, 2 column, 3 value | `alternative_targets`, `multi_module` | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjRecord#viewCurrentLineItemSubrecord` | `N/record.Record#getCurrentSublistSubrecord`, `N/currentRecord.CurrentRecord#getCurrentSublistSubrecord` | options_object | 0 sublist, 1 fldname | `alternative_targets`, `multi_module` | |
| `nlobjRecord#viewLineItemSubrecord` | `N/record.Record#getSublistSubrecord` | options_object | 0 sublist, 1 fldname, 2 linenum | | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRecord#viewSubrecord` | `N/record.Record#getSubrecord`, `N/currentRecord.CurrentRecord#getSubrecord` | options_object | 0 fldname | `alternative_targets`, `multi_module` | |
| `nlobjSearch` | `N/search.Search` | equivalent | - | | |
| `nlobjSearch#addColumn` | `N/search.Search#columns` | pattern_change | 0 column | | Add to the `columns` property. |
| `nlobjSearch#addColumns` | `N/search.Search#columns` | pattern_change | 0 columns | | Add to the `columns` property. |
| `nlobjSearch#addFilter` | `N/search.Search#filters` | pattern_change | 0 filter | | Add to the `filters` property. |
| `nlobjSearch#addFilters` | `N/search.Search#filters` | pattern_change | 0 filters | | Add to the `filters` property. |
| `nlobjSearch#deleteSearch` | `N/search.delete` | renamed | - | | A module function in 2.x. |
| `nlobjSearch#getColumns` | `N/search.Search#columns` | pattern_change | - | `getter_to_property` | |
| `nlobjSearch#getFilterExpression` | `N/search.Search#filterExpression` | pattern_change | - | `getter_to_property` | |
| `nlobjSearch#getFilters` | `N/search.Search#filters` | pattern_change | - | `getter_to_property` | |
| `nlobjSearch#getId` | `N/search.Search#searchId` | pattern_change | - | `getter_to_property` | In 1.0 this is the numeric internal ID (`null` for an ad hoc search); the source types it as a string here but as a number on `saveSearch`, so check the type you compare against. |
| `nlobjSearch#getIsPublic` | `N/search.Search#isPublic` | pattern_change | - | `getter_to_property` | |
| `nlobjSearch#getScriptId` | `N/search.Search#id` | pattern_change | - | `getter_to_property` | |
| `nlobjSearch#getSearchType` | `N/search.Search#searchType` | pattern_change | - | `getter_to_property` | |
| `nlobjSearch#runSearch` | `N/search.Search#run` | renamed | - | | |
| `nlobjSearch#saveSearch` | `N/search.Search#save` | renamed | 0 title, 1 scriptId | | |
| `nlobjSearch#setColumns` | `N/search.Search#columns` | pattern_change | 0 columns | `setter_to_property` | |
| `nlobjSearch#setFilterExpression` | `N/search.Search#filterExpression` | pattern_change | 0 filterExpression | `setter_to_property` | |
| `nlobjSearch#setFilters` | `N/search.Search#filters` | pattern_change | 0 filters | `setter_to_property` | |
| `nlobjSearch#setIsPublic` | `N/search.Search#isPublic` | pattern_change | 0 type | `setter_to_property` | |
| `nlobjSearch#setRedirectURLToSearch` | `N/redirect.toSavedSearch`, `N/redirect.toSearch` | split | - | | |
| `nlobjSearch#setRedirectURLToSearchResults` | `N/redirect.toSearchResult`, `N/redirect.toSavedSearchResult` | split | - | | |
| `nlobjSearchColumn` | `N/search.Column` | renamed | - | | |
| `nlobjSearchFilter` | `N/search.Filter` | renamed | - | | This row pairs the object only. The mapping has no entry for the constructor `new nlobjSearchFilter(name, join, operator, value1, value2)`, and the source does not state which 2.x call replaces it. Several of the constructor's parameters are flagged in the source; see the 1.0 page. |
| `nlobjSearchResult` | `N/search.Result` | renamed | - | | |
| `nlobjSearchResultSet` | `N/search.ResultSet` | renamed | - | | |
| `nlobjSelectOption` | `N/record` | approximate | - | `module_level_only` | |
| `nlobjSelectOption#getId` | `N/record.Field#getSelectOptions`, `N/currentRecord.Field#getSelectOptions` | renamed | - | `alternative_targets`, `multi_module` | |
| `nlobjSelectOption#getText` | `N/record.Field#getSelectOptions`, `N/currentRecord.Field#getSelectOptions` | renamed | - | `alternative_targets`, `multi_module` | |
| `nlobjSubrecord` | `N/record.Record` | approximate | - | | |
| `nlobjSubrecord#cancel` | `N/record.Record` | approximate | - | | The mapping names only the record object; no specific 2.x method is given. |
| `nlobjSubrecord#commit` | `N/record.Record` | approximate | - | | The mapping names only the record object; no specific 2.x method is given. |

### Service objects

Documented on [service objects](../ss1/objects-services.md).

| 1.0 symbol | 2.x symbol(s) | Relation | Arguments (1.0) | Tags | Notes |
|---|---|---|---|---|---|
| `nlobjConfiguration` | `N/record.Record` | renamed | - | | `config.load` returns a record. |
| `nlobjConfiguration#getAllFields` | `N/record.Record#getFields` | renamed | - | | |
| `nlobjConfiguration#getField` | `N/record.Record#getField` | options_object | 0 fldnam | | |
| `nlobjConfiguration#getFieldText` | `N/record.Record#getText` | options_object | 0 name | | |
| `nlobjConfiguration#getFieldTexts` | `N/record.Record#getText` | options_object | 0 name | | Same getter as the single-value form. |
| `nlobjConfiguration#getFieldValue` | `N/record.Record#getValue` | options_object | 0 name | | |
| `nlobjConfiguration#getFieldValues` | `N/record.Record#getValue` | options_object | 0 name | | Same getter as the single-value form. |
| `nlobjConfiguration#getType` | `N/record.Record#type` | pattern_change | - | `getter_to_property` | |
| `nlobjConfiguration#setFieldText` | `N/record.Record#setText` | options_object | 0 name, 1 text | | |
| `nlobjConfiguration#setFieldTexts` | `N/record.Record#setText` | options_object | 0 name, 1 text | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjConfiguration#setFieldValue` | `N/record.Record#setValue` | options_object | 0 name, 1 value | | |
| `nlobjConfiguration#setFieldValues` | `N/record.Record#setValue` | options_object | 0 name, 1 value | | Same setter as the single-value form. |
| `nlobjContext` | `N/runtime.Script`, `N/runtime.Session`, `N/runtime.User` | split | - | | |
| `nlobjContext#getCompany` | `N/runtime.accountId` | pattern_change | - | `getter_to_property`, `target_is_module_property` | Read `runtime.accountId`. |
| `nlobjContext#getDepartment` | `N/runtime.User#department` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getDeploymentId` | `N/runtime.Script#deploymentId` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getEmail` | `N/runtime.User#email` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getEnvironment` | `N/runtime.envType` | pattern_change | - | `getter_to_property`, `target_is_module_property` | Read `runtime.envType`. |
| `nlobjContext#getExecutionContext` | `N/runtime.executionContext` | pattern_change | - | `getter_to_property`, `target_is_module_property` | Read `runtime.executionContext`. |
| `nlobjContext#getFeature` | `N/runtime.isFeatureInEffect` | options_object | 0 name | | |
| `nlobjContext#getLocation` | `N/runtime.User#location` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getLogLevel` | `N/runtime.Script#logLevel` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getName` | `N/runtime.User#name` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getPercentComplete` | `N/runtime.Script#percentComplete` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getPermission` | `N/runtime.User#getPermission` | options_object | 0 name | | |
| `nlobjContext#getPreference` | `N/runtime.User#getPreference` | options_object | 0 name | | |
| `nlobjContext#getQueueCount` | `N/runtime.queueCount` | pattern_change | - | `getter_to_property`, `target_is_module_property` | Read `runtime.queueCount`. |
| `nlobjContext#getRemainingUsage` | `N/runtime.Script#getRemainingUsage` | equivalent | - | | |
| `nlobjContext#getRole` | `N/runtime.User#role` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getRoleCenter` | `N/runtime.User#roleCenter` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getRoleId` | `N/runtime.User#roleId` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getScriptId` | `N/runtime.Script#id` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getSessionObject` | `N/runtime.Session#get` | options_object | 0 name | | |
| `nlobjContext#getSetting` | `N/runtime.Script#getParameter`, `N/runtime.Session#get`, `N/runtime.isFeatureInEffect`, `N/runtime.User#getPermission` | split | 0 type, 1 name | | `SCRIPT` goes to `Script.getParameter`, `SESSION` to `Session.get`, `FEATURE` to `runtime.isFeatureInEffect`, `PERMISSION` to `User.getPermission`. |
| `nlobjContext#getSubsidiary` | `N/runtime.User#subsidiary` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getUser` | `N/runtime.User#id` | pattern_change | - | `getter_to_property` | |
| `nlobjContext#getVersion` | `N/runtime.version` | pattern_change | - | `getter_to_property`, `target_is_module_property` | Read `runtime.version`. |
| `nlobjContext#setPercentComplete` | `N/runtime.Script#percentComplete` | pattern_change | 0 pct | `setter_to_property` | |
| `nlobjContext#setSessionObject` | `N/runtime.Session#set` | options_object | 0 name, 1 value | | |
| `nlobjContext#setSetting` | `N/runtime.Session#set` | options_object | 0 type, 1 name, 2 value | | |
| `nlobjCredentialBuilder` | `N/https.SecureString` | pattern_change | 0 string, 1 domainString | | |
| `nlobjCredentialBuilder#append` | `N/https.SecureString#appendSecureString`, `N/https.SecureString#appendString` | split | 0 string | | The source's heading and parameter list name the arguments differently; see the 1.0 page. |
| `nlobjCredentialBuilder#base64` | `N/https.SecureString#convertEncoding` | renamed | - | | |
| `nlobjCredentialBuilder#replace` | `N/https.SecureString#replaceString` | options_object | 0 string1, 1 string2 | | The mapping names a target, but the transition material lists this method as having no 2.x equivalent and suggests replacing credential strings manually; the source is flagged for the contradiction. See the 1.0 page. |
| `nlobjCredentialBuilder#sha256` | `N/https.SecureString#hash` | renamed | - | | |
| `nlobjCredentialBuilder#utf8` | `N/https.SecureString#convertEncoding` | renamed | - | | |
| `nlobjCSVImport` | `N/task.CsvImportTask` | renamed | - | | |
| `nlobjCSVImport#setLinkedFile` | `N/task.CsvImportTask#linkedFiles` | pattern_change | 0 sublist, 1 file | `setter_to_property` | The source types `file` as a string although a file object is also accepted; see the 1.0 page. |
| `nlobjCSVImport#setMapping` | `N/task.CsvImportTask#mappingId` | pattern_change | 0 savedImport | `setter_to_property` | |
| `nlobjCSVImport#setOption` | `N/task.CsvImportTask#name` | pattern_change | 0 option, 1 value | `setter_to_property` | |
| `nlobjCSVImport#setPrimaryFile` | `N/task.CsvImportTask#importFile` | pattern_change | 0 file | `setter_to_property` | The source types `file` as a string although a file object is also accepted; see the 1.0 page. |
| `nlobjCSVImport#setQueue` | `N/task.CsvImportTask#queueId` | pattern_change | 0 string | `setter_to_property` | |
| `nlobjDuplicateJobRequest` | `N/task.EntityDeduplicationTask` | renamed | - | | |
| `nlobjDuplicateJobRequest#setEntityType` | `N/task.EntityDeduplicationTask#entityType` | pattern_change | 0 entityType | `setter_to_property` | |
| `nlobjDuplicateJobRequest#setMasterId` | `N/task.EntityDeduplicationTask#masterRecordId` | pattern_change | 0 masterID | `setter_to_property` | |
| `nlobjDuplicateJobRequest#setMasterSelectionMode` | `N/task.EntityDeduplicationTask#masterSelectionMode` | pattern_change | 0 mode | `setter_to_property` | |
| `nlobjDuplicateJobRequest#setOperation` | `N/task.EntityDeduplicationTask#dedupeMode` | pattern_change | 0 operation | `setter_to_property` | |
| `nlobjDuplicateJobRequest#setRecords` | `N/task.EntityDeduplicationTask#recordIds` | pattern_change | 0 dupeRecords | `setter_to_property` | |
| `nlobjEmailMerger` | `N/render.EmailMergeResult` | renamed | - | | The source is flagged for giving this object the same target as `nlobjMergeResult`; see the 1.0 page. |
| `nlobjEmailMerger#merge` | `N/render.mergeEmail` | approximate | - | | |
| `nlobjEmailMerger#setCustomRecord` | `N/render.mergeEmail` | approximate | 0 recordType, 1 recordId | | The setters become `mergeEmail` parameters. |
| `nlobjEmailMerger#setEntity` | `N/render.mergeEmail` | approximate | 0 entityType, 1 entityId | | The setters become `mergeEmail` parameters. |
| `nlobjEmailMerger#setRecipient` | `N/render.mergeEmail` | approximate | 0 recipientType, 1 recipientId | | The setters become `mergeEmail` parameters. |
| `nlobjEmailMerger#setSupportCase` | `N/render.mergeEmail` | approximate | 0 caseId | | The setters become `mergeEmail` parameters. |
| `nlobjEmailMerger#setTransaction` | `N/render.mergeEmail` | approximate | 0 transactionId | | The setters become `mergeEmail` parameters. |
| `nlobjError` | `N/error.SuiteScriptError` | renamed | - | | |
| `nlobjError#getCode` | `N/error.SuiteScriptError#name` | pattern_change | - | `getter_to_property` | |
| `nlobjError#getDetails` | `N/error.SuiteScriptError#message` | pattern_change | - | `getter_to_property` | |
| `nlobjError#getId` | `N/error.SuiteScriptError#id` | pattern_change | - | `getter_to_property` | |
| `nlobjError#getInternalId` | `N/error.SuiteScriptError#id` | pattern_change | - | `getter_to_property` | |
| `nlobjError#getStackTrace` | `N/error.SuiteScriptError#stack` | pattern_change | - | `getter_to_property` | |
| `nlobjError#getUserEvent` | - | no_equivalent | - | | No 2.x equivalent; the source states no alternative. |
| `nlobjFile` | `N/file.File` | equivalent | - | | |
| `nlobjFile#getDescription` | `N/file.File#description` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getFolder` | `N/file.File#folder` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getId` | `N/file.File#id` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getName` | `N/file.File#name` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getSize` | `N/file.File#size` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getType` | `N/file.File#fileType` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getURL` | `N/file.File#url` | pattern_change | - | `getter_to_property` | |
| `nlobjFile#getValue` | `N/file.File#getContents` | renamed | - | | |
| `nlobjFile#isInactive` | `N/file.File#isInactive` | pattern_change | - | | Read the property. |
| `nlobjFile#isOnline` | `N/file.File#isOnline` | pattern_change | - | | Read the property. |
| `nlobjFile#setDescription` | `N/file.File#description` | pattern_change | 0 description | `setter_to_property` | |
| `nlobjFile#setEncoding` | `N/file.File#encoding` | pattern_change | 0 encodingType | `setter_to_property` | |
| `nlobjFile#setFolder` | `N/file.File#folder` | pattern_change | 0 id | `setter_to_property` | |
| `nlobjFile#setIsInactive` | `N/file.File#isInactive` | pattern_change | 0 inactive | `setter_to_property` | |
| `nlobjFile#setIsOnline` | `N/file.File#isOnline` | pattern_change | 0 online | `setter_to_property` | |
| `nlobjFile#setName` | `N/file.File#name` | pattern_change | 0 name | `setter_to_property` | |
| `nlobjFuture` | `N/task.EntityDeduplicationTaskStatus` | renamed | - | | |
| `nlobjFuture#isCancelled` | `N/task.EntityDeduplicationTaskStatus#status` | pattern_change | - | | Check the `status` property. |
| `nlobjFuture#isDone` | `N/task.EntityDeduplicationTaskStatus#status` | pattern_change | - | | Check the `status` property. |
| `nlobjJobManager` | `N/task.EntityDeduplicationTaskStatus` | renamed | - | | The source is flagged for giving this object the same target as `nlobjFuture`; see the 1.0 page. |
| `nlobjJobManager#createJobRequest` | `N/task.create` | renamed | - | | |
| `nlobjJobManager#getFuture` | `N/task.checkStatus` | renamed | - | | The source's 1.0 reference lists no parameters, yet its example passes a job ID; see the 1.0 page. |
| `nlobjJobManager#submit` | `N/task.EntityDeduplicationTask#submit` | equivalent | 0 nlobjDuplicateJobRequest | | Called on the task object that replaces the job request. |
| `nlobjLogin` | `N/auth` | approximate | - | `module_level_only` | |
| `nlobjLogin#changeEmail` | `N/auth.changeEmail` | options_object | 0 currentPassword, 1 newEmail, 2 justThisAccount | | |
| `nlobjLogin#changePassword` | `N/auth.changePassword` | options_object | 0 currentPassword, 1 newPassword | | |
| `nlobjMergeResult` | `N/render.EmailMergeResult` | renamed | - | | |
| `nlobjMergeResult#getBody` | `N/render.EmailMergeResult#body` | pattern_change | - | `getter_to_property` | |
| `nlobjMergeResult#getSubject` | `N/render.EmailMergeResult#subject` | pattern_change | - | `getter_to_property` | |
| `nlobjRequest` | `N/http.ServerRequest`, `N/https.ServerRequest` | renamed | - | `multi_module` | The source's map notes for this object are flagged for writing read-only properties as option calls; see the 1.0 page. |
| `nlobjRequest#getAllHeaders` | `N/http.ServerRequest#headers`, `N/https.ServerRequest#headers` | pattern_change | - | `getter_to_property`, `multi_module` | |
| `nlobjRequest#getAllParameters` | `N/http.ServerRequest#parameters`, `N/https.ServerRequest#parameters` | pattern_change | - | `getter_to_property`, `multi_module` | The source's 1.0 return type (string array) disagrees with its description (an object); see the 1.0 page. |
| `nlobjRequest#getBody` | `N/http.ServerRequest#body`, `N/https.ServerRequest#body` | pattern_change | - | `getter_to_property`, `multi_module` | |
| `nlobjRequest#getFile` | `N/http.ServerRequest#files`, `N/https.ServerRequest#files` | pattern_change | 0 id | `getter_to_property`, `multi_module` | Read the file from the `files` property. |
| `nlobjRequest#getHeader` | `N/http.ServerRequest#headers`, `N/https.ServerRequest#headers` | pattern_change | 0 name | `getter_to_property`, `multi_module` | Read the header from the `headers` property. |
| `nlobjRequest#getLineItemCount` | `N/http.ServerRequest#getLineCount`, `N/https.ServerRequest#getLineCount` | options_object | 0 group | `multi_module` | |
| `nlobjRequest#getLineItemValue` | `N/http.ServerRequest#getSublistValue`, `N/https.ServerRequest#getSublistValue` | options_object | 0 group, 1 name, 2 line | `multi_module` | Line: 1-based in 1.0, 0-based in 2.x. |
| `nlobjRequest#getMethod` | `N/http.ServerRequest#method`, `N/https.ServerRequest#method` | pattern_change | - | `getter_to_property`, `multi_module` | |
| `nlobjRequest#getParameter` | `N/http.ServerRequest#parameters`, `N/https.ServerRequest#parameters` | pattern_change | 0 name | `getter_to_property`, `multi_module` | Read the value from the `parameters` property. |
| `nlobjRequest#getParameterValues` | `N/http.ServerRequest#parameters`, `N/https.ServerRequest#parameters` | pattern_change | 0 name | `getter_to_property`, `multi_module` | Read the value from the `parameters` property. |
| `nlobjRequest#getURL` | `N/http.ServerRequest#url`, `N/https.ServerRequest#url` | pattern_change | - | `getter_to_property`, `multi_module` | |
| `nlobjResponse` | `N/http.ServerResponse`, `N/https.ServerResponse` | renamed | - | `multi_module` | |
| `nlobjResponse#addHeader` | `N/http.ServerResponse#addHeader`, `N/https.ServerResponse#addHeader` | options_object | 0 name, 1 value | `multi_module` | |
| `nlobjResponse#getAllHeaders` | `N/http.ServerResponse#getHeader`, `N/https.ServerResponse#getHeader`, `N/http.ServerResponse#headers`, `N/https.ServerResponse#headers` | split | - | `multi_module` | |
| `nlobjResponse#getBody` | `N/http.ClientResponse#body`, `N/https.ClientResponse#body` | pattern_change | - | `getter_to_property`, `multi_module` | A client response in 2.x. |
| `nlobjResponse#getCode` | `N/http.ClientResponse#code`, `N/https.ClientResponse#code` | pattern_change | - | `getter_to_property`, `multi_module` | A client response in 2.x. |
| `nlobjResponse#getError` | - | no_equivalent | - | | No 2.x equivalent; the source states no alternative. |
| `nlobjResponse#getHeader` | `N/http.ServerResponse#getHeader`, `N/https.ServerResponse#getHeader` | options_object | 0 name | `multi_module` | |
| `nlobjResponse#getHeaders` | `N/http.ServerResponse#getHeader`, `N/https.ServerResponse#getHeader` | options_object | 0 name | `multi_module` | Returns an array when a header has several values. |
| `nlobjResponse#renderPDF` | `N/http.ServerResponse#renderPdf`, `N/https.ServerResponse#renderPdf` | options_object | 0 xmlString | `multi_module` | |
| `nlobjResponse#setCDNCacheable` | `N/http.ServerResponse#setCdnCacheable`, `N/https.ServerResponse#setCdnCacheable` | options_object | 0 type | `multi_module` | |
| `nlobjResponse#setContentType` | `N/http`, `N/https` | approximate | 0 type, 1 name, 2 disposition | `module_level_only`, `multi_module` | Set HTTP headers instead. |
| `nlobjResponse#setEncoding` | `N/https.ServerResponse`, `script:Suitelet` | approximate | 0 encodingType | `target_is_script_object` | Set HTTP headers instead; an `N/https` server response sends text as UTF-8 by default. |
| `nlobjResponse#setHeader` | `N/http.ServerResponse#setHeader`, `N/https.ServerResponse#setHeader` | options_object | 0 name, 1 value | `multi_module` | |
| `nlobjResponse#sendRedirect` | `N/http.ServerResponse#sendRedirect`, `N/https.ServerResponse#sendRedirect` | options_object | 0 type, 1 identifier, 2 id, 3 editmode, 4 parameters | `multi_module` | |
| `nlobjResponse#write` | `N/http.ServerResponse#write`, `N/https.ServerResponse#write` | options_object | 0 output | `multi_module` | |
| `nlobjResponse#writeLine` | `N/http.ServerResponse#writeLine`, `N/https.ServerResponse#writeLine` | options_object | 0 output | `multi_module` | |
| `nlobjResponse#writePage` | `N/http.ServerResponse#writePage`, `N/https.ServerResponse#writePage` | options_object | 0 pageobject | `multi_module` | |
