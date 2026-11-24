# Search, scheduling and context functions (SuiteScript 1.0)

> **Legacy API.** This page exists so that tools and engineers can read, convert and minimally repair
> existing SuiteScript 1.0 scripts. It is not a guide for writing new 1.0 code; new work belongs in
> SuiteScript 2.1. Each section names the 2.x replacement where the mapping gives one.

In 1.0 every `nlapi*` function takes **positional** arguments, so the order in each parameter table
below is the order the call must use. Legacy code typically fills a skipped optional slot with
`null` (general JavaScript practice for positional calls; individual functions note where the source
says so explicitly). A `**Since:**` line appears where the source states the release that introduced
a function.

Related reading:

- Concepts: [SuiteScript 1.0 API overview](../topics/ss1_api.md),
  [1.0 searches](../topics/ss1_api.search.md),
  [1.0 scheduled scripts](../topics/ss1_api.scheduled_scripts.md),
  [1.0 Suitelets](../topics/ss1_api.suitelets.md),
  [1.0 user events](../topics/ss1_api.user_events.md),
  [1.0 client scripts](../topics/ss1_api.client_scripts.md),
  [governance and logging limits](../topics/governance.md).
- Catalogs: [search date filters](../gen/catalogs/search_date_filters.md) (special date values such as
  `today` for filter values) and [search operators by field type](../gen/catalogs/search_operator_field_types.md).
- Search objects (`nlobjSearch`, `nlobjSearchFilter` with its `new nlobjSearchFilter(...)`
  constructor, `nlobjSearchColumn`, `nlobjSearchResult`):
  [1.0 record and search objects](objects-record-search.md). Context object:
  [1.0 service objects](objects-services.md).
- Converting: [SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).

## Searching

### nlapiCreateSearch(type, filters, columns)
<!-- id: ss1:nlapiCreateSearch -->

Builds a search definition in memory without running or saving it. You get back an `nlobjSearch`
that can be run ad hoc; to keep it as a saved search, call `nlobjSearch.saveSearch` on it afterwards.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `filters` | nlobjSearchFilter \| nlobjSearchFilter[] \| Object[] | No | |
| `columns` | nlobjSearchColumn \| nlobjSearchColumn[] | No | |

- `filters`: a single `nlobjSearchFilter`, an array of them, or a filter expression (the `Object[]`
  form).
- `columns`: a single `nlobjSearchColumn` or an array of them.

**Returns:** `nlobjSearch`, an unsaved search you can run directly.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2012.1

**2.x:** `search.create(options)` in [N/search](../modules/N_search.md); the positional arguments
become `options.type`, `options.filters` and `options.columns`.

### nlapiLoadSearch(type, id)
<!-- id: ss1:nlapiLoadSearch -->

Loads an existing saved search, whether it was built in the UI or saved from a script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | No | |
| `id` | string | Yes | |

- `type`: the record type, not case-sensitive. If you do not know the saved search's record type,
  pass `null`.
- `id`: the saved search's internal ID or its script ID.

**Returns:** `nlobjSearch`

**Governance:** 5 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2012.1

**2.x:** `search.load(options)` in [N/search](../modules/N_search.md).

### nlapiSearchDuplicate(type, fields, id)
<!-- id: ss1:nlapiSearchDuplicate -->

Looks for duplicate records using the duplicate-detection settings configured in the account. There
are two ways to call it: give `fields` (and no `id`) to ask "which records would duplicate a record
with these values?", or give `id` (and no `fields`) to find the duplicates of an existing record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `fields` | string[] | No | |
| `id` | number | No | |

- `type`: only `customer`, `lead`, `prospect`, `contact`, `partner` and `vendor` are supported.

**Returns:** `nlobjSearchResult[]`, at most 1000 rows. When nothing matches you get `null`, not an
empty array, so test for `null` before reading `.length`. (The declared type does not mention
`null`, but the documented behaviour does.)

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** `search.duplicates(options)` in [N/search](../modules/N_search.md).

### nlapiSearchGlobal(keywords)
<!-- id: ss1:nlapiSearchGlobal -->

Runs a global keyword search across the record types in the account.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `keywords` | string | Yes | |

- `keywords` can be narrowed to one record type with a type prefix; for example `cu: keyword`
  restricts the search to customers.

**Returns:** `nlobjSearchResult[] | null`. Each result exposes the columns `name`, `type`, `info1` and
`info2`. At most 1000 rows come back; when nothing matches the result is `null`, not an empty array.

**Governance:** 10 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** `search.global(options)` in [N/search](../modules/N_search.md).

### nlapiSearchRecord(type, id, filters, columns)
<!-- id: ss1:nlapiSearchRecord -->

The workhorse of 1.0 searching. It either runs an ad hoc search built from `filters` and `columns`,
or runs a saved search identified by `id` (optionally with extra filters and columns added on top). It
can also search custom lists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | No | |
| `id` | number \| string | No | |
| `filters` | nlobjSearchFilter \| nlobjSearchFilter[] \| Object[] | No | |
| `columns` | nlobjSearchColumn \| nlobjSearchColumn[] | No | |

- `type`: the record type to search. When you run a saved search and do not know its record type,
  pass `null` here together with the saved search ID.
- `id`: a saved search internal ID or script ID. If it is `null`, empty or omitted, the call runs as
  an ad hoc search. It becomes required when `type` is omitted. An ID that does not identify a valid
  saved search raises a user error; the source does not tie that case to a specific code (the list
  below does include `SSS_INVALID_SRCH_ID` for an invalid search ID value).
- `filters`: a single `nlobjSearchFilter`, an array of them, or a filter expression. Valid operators
  per field type are listed in [search_operator_field_types](../gen/catalogs/search_operator_field_types.md);
  special date values in [search_date_filters](../gen/catalogs/search_date_filters.md).
- `columns`: a single `nlobjSearchColumn` or an array of them.

**Returns:** `nlobjSearchResult[]`, a read-only array of at most 1000 results. When nothing matches
the call returns `null` rather than an empty array; legacy code that reads `.length` without a
`null` check fails on an empty result.

**Errors** (each is raised for an invalid or missing argument; the meaning beyond that is read from
the code's name):

- `SSS_INVALID_RECORD_TYPE`: an invalid value, the record type.
- `SSS_TYPE_ARG_REQD`: a required argument is missing, the record type.
- `SSS_INVALID_SRCH_ID`: an invalid value, the saved search ID.
- `SSS_INVALID_SRCH_FILTER`: an invalid filter.
- `SSS_INVALID_SRCH_FILTER_JOIN`: an invalid filter join.
- `SSS_INVALID_SRCH_OPERATOR`: an invalid filter operator.
- `SSS_INVALID_SRCH_COL_NAME`: an invalid column name.
- `SSS_INVALID_SRCH_COL_JOIN`: an invalid column join.
- `SSS_INVALID_SRCH_FILTER_EXPR`: an invalid filter expression.
- `SSS_INVALID_SRCH_FILTER_EXPR_DANGLING_OP`: an invalid filter expression (an operator with nothing
  after it).
- `SSS_INVALID_SRCH_FILTER_EXPR_OBJ_TYPE`: an invalid filter expression (an element of the wrong
  object type).
- `SSS_INVALID_SRCH_FILTER_EXPR_PAREN_DEPTH`: an invalid filter expression (nesting depth).
- `SSS_INVALID_SRCH_FILTER_LIST_PARENS`: an invalid filter list (parentheses).
- `SSS_INVALID_SRCH_FILTER_LIST_TERM`: an invalid filter list (a term).

**Governance:** 10 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

- Long text values in results are cut off: at 1,300 characters when the account has multiple
  languages enabled, and at 3,900 characters in a single-language account.
- To control ordering, use `nlobjSearchColumn.setSort`, base the call on a saved search that is sorted
  by internal ID, or sort the returned array yourself in JavaScript.

**2.x:** split between `search.create(options)` (ad hoc) and `search.load(options)` (saved search) in
[N/search](../modules/N_search.md).

## Scheduling and long-running scripts

### nlapiScheduleScript(scriptId, deployId, params)
<!-- id: ss1:nlapiScheduleScript -->

Puts a scheduled script into the processing queue so it runs as soon as possible, instead of waiting
for a schedule. For an on-demand run the target deployment needs the status "Not Scheduled": a
deployment in "Testing" status is not queued by this call, and one in "Scheduled" status runs at the
times set on the deployment instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `scriptId` | string \| number | Yes | |
| `deployId` | string \| number | No | |
| `params` | Object | No | |

- `scriptId`: the script's internal ID or script ID.
- `deployId`: when omitted, the first free deployment is used. A deployment counts as free when its
  status is either "Not Scheduled" or "Completed". The argument is required when a script re-queues
  itself and when the target script has more than one deployment.
- `params`: values that override the deployment's script parameters for this run only. Keys are the
  script parameters' internal IDs.

**Returns:** `string`, the queue status:

- `QUEUED`: this call queued the script.
- `INQUEUE`: the script was already waiting in the queue; retry after that run finishes.
- `INPROGRESS`: the script is running right now.
- `SCHEDULED`: the deployment's status is "Scheduled", so it runs on its own schedule.

It returns `null` when the script is not deployed or the arguments do not identify a valid script
(the declared type is `string`, but `null` is the documented result in that case).

**Governance:** 20 units. A script that re-queues itself is charged 0 units for the call; any other
call costs 20.

**Availability:** user event, portlet, scheduled and Suitelet scripts.

- Requires the `"Administrator"` permission; for user event scripts this means the deployment must
  run with the Administrator role.

**2.x:** `task.create(options)` in [N/task](../modules/N_task.md) (a scheduled script task, then
submit it).

### nlapiSetRecoveryPoint()
<!-- id: ss1:nlapiSetRecoveryPoint -->

Saves a snapshot of the running scheduled script's execution state. When the script is later
resumed, it carries on from this point rather than from the beginning, and its usage-unit count is
reset at the resume. Use it together with [`nlapiYieldScript`](#nlapiyieldscript), which sets a
recovery point and gives up the queue slot in one step.

**Returns:** `Object` with `status`, `reason`, `size` and `information` properties.

- `status` is `SUCCESS`, `FAILURE` or `RESUME`.
- `reason` is one of `SS_NLAPIYIELDSCRIPT`, `SS_ABORT`, `SS_MAJOR_RELEASE`,
  `SS_EXCESSIVE_MEMORY_FOOTPRINT`, `SS_CANCELLED`, `SS_DISALLOWED_OBJECT_REFERENCE`,
  `SSS_FILE_OBJECT_NOT_SERIALIZABLE` or `SSS_SCRIPT_DESERIALIZATION_FAILURE`.
- When `reason` is `SS_DISALLOWED_OBJECT_REFERENCE`, `information` names the type of object that
  could not be saved.

**Governance:** 100 units.

**Availability:** scheduled scripts only; calling it from any other script type throws an error.

What can be saved in the snapshot: native JavaScript values, `nlobjConfiguration`, `nlobjContext`,
`nlobjError`, `nlobjFile` up to 5 MB, `nlobjRecord`, `nlobjSubrecord`, `nlobjSearchColumn`,
`nlobjSearchFilter`, `nlobjSearchResult`, `nlobjSearchResultCell`, and objects from third-party XML
libraries.

> [!SOURCE-ERROR] The source writes the size limit for serializable files as "5 BM" in one place and
> "5 MB" elsewhere. The 5 MB reading is the plausible one, but it has not been confirmed; check before
> relying on the exact figure.
<!-- source-error: ss1:nlapiSetRecoveryPoint | - | serializable_file_limit_written_5bm -->

Rules for a successful snapshot:

- Set variables holding record objects to `null` before the call; otherwise the resume can fail with
  `SSS_SCRIPT_DESERIALIZATION_FAILURE`.
- Set variables holding files larger than 5 MB to `null` before the call; otherwise it fails with
  `SSS_FILE_OBJECT_NOT_SERIALIZABLE`.
- Do not call it from inside array iteration callbacks (`every`, `filter`, `forEach`, `map`, `some`).
  Use a plain `for` loop, or `forEachResult` on search results, instead.
- The 50 MB memory limit is not reset by a recovery point, only the usage units.

**2.x:** no direct function; the mapping points (approximately) to the Map/Reduce script type
([MapReduceScript](../script-types/MapReduceScript.md)).

### nlapiYieldScript()
<!-- id: ss1:nlapiYieldScript -->

Creates a recovery point, then puts the script at the back of the scheduling queue so other work can
run. When the script is picked up again it continues right after this call, with its usage units
reset. In other words the call does not return until the script resumes, unless the yield itself
fails.

**Returns:** `Object` with `status`, `reason`, `size` and `information`. You only see it when the
yield failed (`status` is `FAILURE`) or after the script has resumed (`status` is `RESUME`). `reason`
takes the same values as for [`nlapiSetRecoveryPoint`](#nlapisetrecoverypoint), and `information`
names the offending object type when `reason` is `SS_DISALLOWED_OBJECT_REFERENCE`.

**Governance:** 0 units.

**Availability:** scheduled scripts only; calling it elsewhere throws an error.

- On a yield, pending `finally` blocks run but `catch` blocks are skipped. Because the script resumes
  inside the same code, statements in a `finally` block may run a second time after the resume, so
  keep anything that changes the script's flow out of `finally` blocks.
- The 50 MB memory limit is not reset by yielding.
- Null out record references before yielding (else `SSS_SCRIPT_DESERIALIZATION_FAILURE`) and file
  references over 5 MB (else `SSS_FILE_OBJECT_NOT_SERIALIZABLE`).
- Do not yield from inside `every`, `filter`, `forEach`, `map` or `some` callbacks; use a `for` loop
  or `forEachResult`.
- Serializable state is the same as for `nlapiSetRecoveryPoint`: native JavaScript values,
  `nlobjConfiguration`, `nlobjContext`, `nlobjError`, `nlobjFile` up to 5 MB, `nlobjRecord`,
  `nlobjSubrecord`, `nlobjSearchColumn`, `nlobjSearchFilter`, `nlobjSearchResult`,
  `nlobjSearchResultCell` and third-party XML library objects.

**2.x:** no direct function; the mapping points (approximately) to the Map/Reduce script type
([MapReduceScript](../script-types/MapReduceScript.md)).

## Execution context

### nlapiGetContext()
<!-- id: ss1:nlapiGetContext -->

Returns the context object for the current execution. Its main use is letting a script branch on
the context it is running in. Script parameters are also read through it, with
`nlobjContext.getSetting('SCRIPT', name)`.

**Returns:** `nlobjContext`

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** split across `runtime.getCurrentScript()`, `runtime.getCurrentSession()` and
`runtime.getCurrentUser()` in [N/runtime](../modules/N_runtime.md).

### nlapiGetDepartment()
<!-- id: ss1:nlapiGetDepartment -->

Returns the internal ID of the current user's department.

**Returns:** `number`

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** read the `department` property of the user object (`runtime.User#department`) from
[N/runtime](../modules/N_runtime.md).

### nlapiGetLocation()
<!-- id: ss1:nlapiGetLocation -->

Returns the internal ID of the current user's location.

**Returns:** `number`; `-1` when the user has no location set.

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** the `location` property of the user object (`runtime.User#location`) in
[N/runtime](../modules/N_runtime.md).

### nlapiGetRole()
<!-- id: ss1:nlapiGetRole -->

Returns the internal ID of the role the current user is logged in with.

**Returns:** `number`. When no user is authenticated, as in Suitelets available without login and in
online forms, it returns `-31`.

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** the `role` property of the user object (`runtime.User#role`) in
[N/runtime](../modules/N_runtime.md).

### nlapiGetSubsidiary()
<!-- id: ss1:nlapiGetSubsidiary -->

Returns the internal ID of the current user's subsidiary.

**Returns:** `number`. It returns `1` when the user has no subsidiary set or the account does not use
subsidiaries, so `1` alone does not prove a real subsidiary assignment.

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** the `subsidiary` property of the user object (`runtime.User#subsidiary`) in
[N/runtime](../modules/N_runtime.md).

### nlapiGetUser()
<!-- id: ss1:nlapiGetUser -->

Returns the internal ID of the current user.

**Returns:** `number`. When no user is authenticated, as in Suitelets available without login and in
online forms, it returns `-4`.

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**2.x:** the mapping names `runtime.getCurrentUser()` in [N/runtime](../modules/N_runtime.md).

> [!SOURCE-ERROR] That mapping target returns a user object, not the numeric user ID this function
> returns. When converting, take the ID from the user object rather than using the object itself where
> a number is expected.
<!-- source-error: ss1:nlapiGetUser | - | getuser_mapped_to_getcurrentuser_not_user_id -->

### nlapiLogExecution(type, title, details)
<!-- id: ss1:nlapiLogExecution -->

Writes an entry to the execution log of the script deployment. An entry is kept only if its `type`
is at or above the log level set on the deployment. Unless changed, that level is `ERROR` for
released deployments and `DEBUG` for deployments in testing, so `DEBUG` lines silently disappear once
a script is released.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `title` | string | No | |
| `details` | string | No | |

- `type`: `DEBUG`, `AUDIT`, `ERROR` or `EMERGENCY`.
- `title`: up to 99 characters. A `null` or empty title shows as "Untitled".
- `details`: up to 3999 characters.

> [!SOURCE-ERROR] The source marks `title` as optional, yet also says that leaving it out throws
> `SSS_MISSING_REQD_ARGUMENT`. The source does not resolve which is true; when repairing legacy code,
> always pass a title.
<!-- source-error: ss1:nlapiLogExecution | - | title_optional_but_missing_title_throws -->

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.

**Governance:** 0 units. The function's own documentation does not state a cost; this figure was
supplied separately for the project rather than taken from the vendor material.

**Availability:** all server scripts, and client scripts deployed at record level.

- HTML markup and JavaScript in the logged text are shown as plain text, not rendered.
- When the script runs in the Script Debugger, entries go to the debugger's own execution log
  instead of the deployment's.
- Logging volume is capped per company within each 60-minute window. The function's entry gives no
  figure; the [governance topic](../topics/governance.md) states 100,000 log calls per company per
  60 minutes and also describes how the log level is raised automatically for scripts that log
  excessively.

**2.x:** split into `log.debug`, `log.audit`, `log.error` and `log.emergency` in
[N/log](../modules/N_log.md), chosen by the 1.0 `type` value.

## Examples

These 1.0 snippets are plain functions, written to show how legacy code is shaped so it can be read
and converted.

Open invoices for one customer that are already past due, largest open amount first, handling the
`null` that `nlapiSearchRecord` returns for "no rows":

```js
function findPastDueInvoices(customerId) {
  var filters = [
    new nlobjSearchFilter('entity', null, 'anyof', customerId),
    new nlobjSearchFilter('duedate', null, 'before', 'today'),
    new nlobjSearchFilter('amountremaining', null, 'greaterthan', 0)
  ];
  var columns = [
    new nlobjSearchColumn('duedate'),
    new nlobjSearchColumn('amountremaining')
  ];
  var rows = nlapiSearchRecord('invoice', null, filters, columns);
  if (rows === null) {
    return [];
  }
  var overdue = [];
  for (var i = 0; i < rows.length; i++) {
    overdue.push({
      id: rows[i].getId(),
      due: rows[i].getValue('duedate'),
      open: parseFloat(rows[i].getValue('amountremaining'))
    });
  }
  overdue.sort(function (a, b) {
    return b.open - a.open;
  });
  return overdue;
}
```

A scheduled script that walks a saved search named in a script parameter and yields when usage runs
low. It loops with `for`, never inside `forEach`, because yielding from a callback is not allowed:

```js
function rebuildCommissionTotals() {
  var context = nlapiGetContext();
  var searchId = context.getSetting('SCRIPT', 'custscript_commission_source');
  var rows = nlapiSearchRecord(null, searchId);
  if (!rows) {
    nlapiLogExecution('AUDIT', 'Nothing to rebuild', 'Search ' + searchId + ' returned no rows');
    return;
  }
  for (var i = 0; i < rows.length; i++) {
    applyCommissionRow(rows[i]);
    if (context.getRemainingUsage() < 300) {
      var outcome = nlapiYieldScript();
      if (outcome.status === 'FAILURE') {
        nlapiLogExecution('ERROR', 'Yield failed', outcome.reason + ' (' + outcome.size + ')');
        return;
      }
    }
  }
}

function applyCommissionRow(row) {
  nlapiLogExecution('DEBUG', 'Commission row', row.getId());
}
```

A Suitelet handler that hands heavy work to a scheduled script, overriding one script parameter for
that run only. Leaving out the deployment ID would make the call pick the first deployment whose
status is "Not Scheduled" or "Completed"; it is passed here because the script has several:

```js
function requestPriceRefresh(request, response) {
  var priceLevel = request.getParameter('custpage_price_level');
  var status = nlapiScheduleScript(
    'customscript_refresh_price_level',
    'customdeploy_refresh_on_demand',
    { custscript_target_price_level: priceLevel }
  );
  if (status === null) {
    response.write('The refresh job is not deployed.');
    return;
  }
  response.write('Refresh job status: ' + status);
}
```
