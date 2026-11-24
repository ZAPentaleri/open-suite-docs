# N/redirect
<!-- id: N/redirect -->

`N/redirect` decides where the user's browser goes once your server script is done. The module
offers six kinds of destination: a URL, a Suitelet, a record, a NetSuite page by task link, a saved
search, and an unsaved search (definition page or results). Because it steers a browser session, it
needs a context that was triggered from the UI.

- **Availability:** server scripts.
- **Where it works:** Suitelets, `beforeLoad` user events, and synchronous `afterSubmit` user
  events. It is not supported in `beforeSubmit`, in asynchronous `afterSubmit`, during CSV import,
  or in scheduled scripts. Individual functions narrow this further (see each section).
- **Errors:** the source does not state which errors most of these functions raise; only
  `toRecordTransform` has documented error codes.
- **Load it as:** `define(['N/redirect'], (redirect) => { ... })`
- Related: [N/url](N_url.md) (compute a URL without navigating),
  [N/https](N_https.md) (`ServerResponse.sendRedirect`), [N/search](N_search.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`redirect.redirect(options)`](#redirectredirectoptions) | function | Go to a URL (an externally available Suitelet) |
| [`redirect.toRecord(options)`](#redirecttorecordoptions) | function | Open an existing record |
| [`redirect.toRecordTransform(options)`](#redirecttorecordtransformoptions) | function | Open a new record transformed from another |
| [`redirect.toSavedSearch(options)`](#redirecttosavedsearchoptions) | function | Open a saved search's definition |
| [`redirect.toSavedSearchResult(options)`](#redirecttosavedsearchresultoptions) | function | Open a saved search's results |
| [`redirect.toSearch(options)`](#redirecttosearchoptions) | function | Open the definition page of a search object |
| [`redirect.toSearchResult(options)`](#redirecttosearchresultoptions) | function | Open the results of a search object |
| [`redirect.toSuitelet(options)`](#redirecttosuiteletoptions) | function | Go to a Suitelet deployment |
| [`redirect.toTaskLink(options)`](#redirecttotasklinkoptions) | function | Go to a NetSuite page by task ID |

## Functions

### redirect.redirect(options)
<!-- id: N/redirect.redirect -->

Sends the user to a URL; the target is a Suitelet that is available externally.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.parameters` | Object | No | |

- `options.url`: the target Suitelet's deployment must be set to be available without login.
- `options.parameters`: values cannot be arrays; turn an array into a string with
  `JSON.stringify` before passing it.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** server; Suitelets, `beforeLoad` user events and synchronous `afterSubmit` user
events, when triggered from the UI.

**Since:** 2015.2

### redirect.toRecord(options)
<!-- id: N/redirect.toRecord -->

Opens an existing record. The record has to exist already: to land on a brand-new record, create
it, fill in its required fields, save it, and then redirect to it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | string | Yes | |
| `options.isEditMode` | boolean | No | `false` |
| `options.parameters` | Object | No | |

- `options.isEditMode`: `true` opens the record for editing, `false` opens it in view mode.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** server; Suitelets, `beforeLoad` user events and synchronous `afterSubmit` user
events, when triggered from the UI.

**Since:** 2015.2

### redirect.toRecordTransform(options)
<!-- id: N/redirect.toRecordTransform -->

Opens a new transaction created by transforming an existing record into another type. The new
transaction opens in edit mode with its default values filled in, and the user can change it before
saving. Allowed source and target pairs are listed in the
[record transformations catalog](../gen/catalogs/record_transformations.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fromType` | string | Yes | |
| `options.fromId` | number | Yes | |
| `options.toType` | string | Yes | |
| `options.parameters` | Object | No | |

- `options.fromType` and `options.fromId`: taken from the workflow action's context (the record
  the workflow runs on).
- `options.toType`: the target record type, entered by you.
- `options.parameters`: field values for the new record, keyed in the form `record.<fieldId>`.
  Keys that are not valid field IDs are ignored.

**Returns:** `void`

**Errors:**

- `INVALID_RCRD_TYPE`: `options.toType` is not a valid record type.
- `INVALID_RCRD_TRANSFRM`: transforming `options.fromType` into `options.toType` is not allowed.

**Governance:** 0 units.

**Availability:** server; only in workflow action scripts.

> [!SOURCE-ERROR] The source limits this function to workflow action scripts, while the module
> lists Suitelets and user events as its supported contexts, and the method page itself only says
> "server scripts". The spec does not settle which applies; workflow action scripts are the one
> context the function-level rule clearly allows.
<!-- source-error: N/redirect.toRecordTransform | - | transform_workflow_action_only_vs_module_wide_contexts -->

**Since:** 2020.1

### redirect.toSavedSearch(options)
<!-- id: N/redirect.toSavedSearch -->

Opens the definition page of a saved search, already populated with that search's criteria.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |

- `options.id`: the saved search's internal ID. Its script ID (`customsearch...`) is not accepted.
  The internal ID is only known once the search has been loaded or saved.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 5 units.

**Availability:** server; user event scripts, `afterSubmit` only.

**Since:** 2015.2

### redirect.toSavedSearchResult(options)
<!-- id: N/redirect.toSavedSearchResult -->

Opens the results page of a saved search.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |

- `options.id`: the saved search's internal ID, not its `customsearch...` script ID. The internal ID
  becomes known after the search is loaded or saved.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 5 units.

**Availability:** server; user event scripts, `afterSubmit` only.

**Since:** 2015.2

### redirect.toSearch(options)
<!-- id: N/redirect.toSearch -->

Opens the search definition page for a `search.Search` object. The search is loaded into the user's
session so the page can show it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.search` | search.Search | Yes | |

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** server; user event scripts, `afterSubmit` only.

**Since:** 2015.2

### redirect.toSearchResult(options)
<!-- id: N/redirect.toSearchResult -->

Opens the results page for a `search.Search` object. It works with an on-demand search built in the
script and with a saved search you modified but did not save.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.search` | search.Search | Yes | |

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** server; user event scripts, `afterSubmit` only.

**Since:** 2015.2

### redirect.toSuitelet(options)
<!-- id: N/redirect.toSuitelet -->

Sends the user to a Suitelet deployment. The redirect happens after the current script has
finished.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |
| `options.deploymentId` | string | Yes | |
| `options.isExternal` | boolean | No | `false` |
| `options.parameters` | Object | No | |

- `options.isExternal`: whether to use the Suitelet's external URL.

> [!SOURCE-ERROR] The source gives `false` as this option's default but describes the default
> behaviour as though the external URL were used. The spec does not settle which is right; pass the
> value you need explicitly.
<!-- source-error: N/redirect.toSuitelet | options.isExternal | default_false_described_as_external -->

- `options.parameters`: values cannot be arrays; `JSON.stringify` an array first.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** server; Suitelets, `beforeLoad` user events and synchronous `afterSubmit` user
events, when triggered from the UI.

**Since:** 2015.2

### redirect.toTaskLink(options)
<!-- id: N/redirect.toTaskLink -->

Sends the user to a NetSuite page identified by its task ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.parameters` | Object | No | |

- `options.id`: the task ID of the target page.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** server; Suitelets, `beforeLoad` user events and synchronous `afterSubmit` user
events, when triggered from the UI.

**Since:** 2015.2

## Examples

A Suitelet where a returns clerk records an inspection note on a return authorization and is then
taken back to that return in view mode. The record already exists, so `toRecord` is safe.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/redirect', 'N/record'], (redirect, record) => {
  const onRequest = (context) => {
    const { request, response } = context;
    const rmaId = request.parameters.custpage_rma;

    if (request.method !== 'POST' || !rmaId) {
      response.write({ output: 'Open this page from a return authorization.' });
      return;
    }

    record.submitFields({
      type: record.Type.RETURN_AUTHORIZATION,
      id: rmaId,
      values: { memo: `Inspected: ${request.parameters.custpage_note || 'no notes'}` }
    });

    redirect.toRecord({
      type: record.Type.RETURN_AUTHORIZATION,
      id: String(rmaId),
      isEditMode: false
    });
  };

  return { onRequest };
});
```

A user event on vendor bills that, after a new bill is saved, shows the user every item receipt
from the same vendor so they can check three-way matching. The search is built on demand.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/redirect', 'N/search'], (redirect, search) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const vendorId = context.newRecord.getValue({ fieldId: 'entity' });
    const receipts = search.create({
      type: search.Type.ITEM_RECEIPT,
      filters: [['entity', 'anyof', vendorId]],
      columns: ['tranid', 'trandate', 'memo']
    });
    redirect.toSearchResult({ search: receipts });
  };

  return { afterSubmit };
});
```

A workflow action that opens a new item receipt prefilled from the return authorization the
workflow is running on (`returnauthorization` to `itemreceipt` is a listed transformation), with a
memo preset through `parameters`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType WorkflowActionScript
 */
define(['N/redirect'], (redirect) => {
  const onAction = (scriptContext) => {
    const rma = scriptContext.newRecord;
    redirect.toRecordTransform({
      fromType: rma.type,
      fromId: Number(rma.id),
      toType: 'itemreceipt',
      parameters: { 'record.memo': `Receiving against RMA ${rma.id}` }
    });
  };

  return { onAction };
});
```
