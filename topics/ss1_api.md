# SuiteScript 1.0 API conventions
<!-- id: topic:ss1_api -->

> **Legacy material.** SuiteScript 1.0 is documented here only so that tools and developers can
> read, convert and minimally repair existing 1.0 scripts. Do not start new development in 1.0;
> write new scripts in SuiteScript 2.1 and see [Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).

Part of: [Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md)

Sub-topics:

- [SuiteScript 1.0 bundle installation scripts](ss1_api.bundle_installation.md)
- [SuiteScript 1.0 client scripts](ss1_api.client_scripts.md)
- [SuiteScript 1.0 fields](ss1_api.fields.md)
- [SuiteScript 1.0 record initialization defaults](ss1_api.initialization_defaults.md)
- [SuiteScript 1.0 inline editing](ss1_api.inline_editing.md)
- [Mass update script type (SS 1.0)](ss1_api.mass_update.md)
- [SuiteScript 1.0 portlet scripts](ss1_api.portlets.md)
- [SuiteScript 1.0 record modes](ss1_api.records.md)
- [SuiteScript 1.0 RESTlets](ss1_api.restlets.md)
- [SuiteScript 1.0 scheduled scripts](ss1_api.scheduled_scripts.md)
- [SuiteScript 1.0 script types](ss1_api.script_types.md)
- [SuiteScript 1.0 searches](ss1_api.search.md)
- [SuiteScript 1.0 sublists](ss1_api.sublists.md)
- [SuiteScript 1.0 subrecords](ss1_api.subrecords.md)
- [SuiteScript 1.0 Suitelets](ss1_api.suitelets.md)
- [SuiteScript 1.0 user event scripts](ss1_api.user_events.md)
- [SuiteScript 1.0 workflow action scripts](ss1_api.workflow_action.md)

The 1.0 function and object reference lives in the `ss1/` pages:
[record functions](../ss1/records.md), [field and sublist functions](../ss1/fields-sublists.md),
[search, scheduling and context functions](../ss1/search-scheduling-context.md),
[UI and HTTP functions](../ss1/ui-http.md), [utility functions](../ss1/utilities.md),
[record and search objects](../ss1/objects-record-search.md),
[service objects](../ss1/objects-services.md), [form objects](../ss1/objects-forms.md) and
[other UI objects](../ss1/objects-ui-other.md).

## Identifiers

- Every internal ID you pass to a 1.0 API is lower case: record types, fields, sublists, tabs,
  search joins and search columns alike. A legacy script that passes mixed-case IDs is a likely
  source of bugs.
- When a script adds its own UI objects (fields, tabs, sublists, buttons) to a page that NetSuite
  already renders, their IDs must start with `custpage`.
- Radio buttons that belong to one group all carry the same field name; they are told apart by
  their `source` value.

## Values the API expects

| What | 1.0 convention |
|---|---|
| Checkbox values (get and set) | The strings `'T'` and `'F'`, never JavaScript `true` / `false` |
| `firefieldchanged` and `synchronous` arguments | Real booleans (`true` / `false`), not `'T'` / `'F'` |
| Sublist line numbers | Start at 1 |
| Matrix column numbers | Start at 1 |
| Date-time strings used by the date-time value functions | `mm/dd/yyyy hh:mm:ss am` or `pm` |

The two boolean arguments are the one place where the `'T'`/`'F'` habit is wrong, so a converted
or repaired script must not pass `'F'` to `firefieldchanged`: a non-empty string is truthy (general
JavaScript behaviour).

## Editing sublist lines

Lines are edited with a select, set, commit cycle: select a line (a new one with
`nlapiSelectNewLineItem`), set values on the current line with `nlapiSetCurrentLineItemValue`, then
commit it with `nlapiCommitLineItem`. See [SuiteScript 1.0 sublists](ss1_api.sublists.md) and the
[field and sublist functions](../ss1/fields-sublists.md).

## Avoiding fieldChanged loops

A field-change handler that writes to the very field it is watching can trigger itself forever.
Pass `false` as the `firefieldchanged` argument on that write so the change does not fire the event
again.

## The page DOM is off limits

Scripts must not reach into the NetSuite UI through the browser DOM. Use the SuiteScript APIs for
anything on the page.

## How the API is organised

- Some functions belong to more than one category but are documented only once. For example
  `nlapiLookupField` is both a field function and a search function; it is described with the
  [field functions](../ss1/fields-sublists.md).
- The `nlobj*` objects fall into two groups: standard objects, which deal with back-end data and
  with request handling, and UI objects, which build pages, forms and lists. UI objects are used in
  Suitelets, user event scripts and portlets.

## Server scripts, client scripts and web services

- A preference that disables server-side scripting for web services makes server scripts skip
  requests that arrive through web services.
- Web services requests never run client scripts. Business rules that must always hold therefore
  belong in server scripts, not in client scripts.

## nlapiRequestURL on the server

[`nlapiRequestURL`](../ss1/ui-http.md) has a callback parameter only in client scripts. In server
scripts there is no callback, so the HTTP method moves up to become the **fourth** argument. Keep
this in mind when reading legacy server code or converting it.

## Search filter expressions

Besides arrays of `nlobjSearchFilter` objects, 1.0 searches accept filter expressions, both in
client and in server scripts.

- An expression is built from three kinds of element: operators, filter terms and nested
  expressions.
- The operators are `AND`, `OR` and `NOT`; they are not case-sensitive.
- As soon as an expression uses any operator, an `AND` or `OR` must stand between every pair of
  terms.
- Nesting can go at most three levels deep, not counting the outermost brackets.
- A filter term is an array: identifier, operator, operand, then any further operands.
- The identifier can be a filter name, a join ID and a filter name separated by a dot, a formula
  type followed by a colon and the formula, or an aggregate function wrapped around one of these.
- A list of filter objects with no operators at all is treated as all filters joined by AND.

Operators per field type are listed in
[catalog:search_operator_field_types](../gen/catalogs/search_operator_field_types.md); relative
date values in [catalog:search_date_filters](../gen/catalogs/search_date_filters.md). See also
[SuiteScript 1.0 searches](ss1_api.search.md).

## Select field sources

When a script adds a select field, its source can be given either as a name or as the matching
negative number:

| Source | Number |
|---|---|
| `account` | -112 |
| `accountingperiod` | -105 |
| `phonecall` | -22 |
| `campaign` | -24 |
| `campaignevent` | -107 |
| `supportcase` | -23 |
| `classification` | -101 |
| `competitor` | -108 |
| `contact` | -6 |
| `currency` | -122 |
| `customer` | -2 |
| `customercategory` | -109 |
| `department` | -102 |
| `emailtemplate` | -120 |
| `employee` | -4 |
| `employeetype` | -111 |
| `customerstatus` | -104 |
| `calendarevent` | -20 |
| `customfield` | -124 |
| `issue` | -26 |
| `item` | -10 |
| `itemtype` | -106 |
| `location` | -103 |
| `issuemodule` | -116 |
| `opportunity` | -31 |
| `partner` | -5 |
| `issueproduct` | -115 |
| `issueproductbuild` | -114 |
| `issueproductversion` | -113 |
| `job` | -7 |
| `projecttask` | -27 |
| `promotioncode` | -121 |
| record type (number only) | -123 |
| role (number only) | -118 |
| saved search (number only) | -119 |
| scripted record type (number only) | -125 |
| `solution` | -25 |
| `subsidiary` | -117 |
| `task` | -21 |
| `transaction` | -30 |
| `transactiontype` | -100 |
| `vendor` | -3 |
| `vendorcategory` | -110 |

Four sources have no text ID at all: record type, role, saved search and scripted record type.
For those, only the negative number works. For a custom record type, prefer its script ID as the
source: script IDs survive bundling, internal IDs do not.

## Examples (1.0, for reading legacy code)

A client field-change function that tidies the field it watches. The `false` in the last call is
what keeps it from re-triggering itself; `type` is null because `title` is a body field.

```js
function tidySupportCaseTitle(type, name, linenum) {
  if (type !== null || name !== 'title') {
    return;
  }
  var raw = nlapiGetFieldValue('title') || '';
  var cleaned = raw.replace(/\s+/g, ' ').trim();
  if (cleaned !== raw) {
    nlapiSetFieldValue('title', cleaned, false);
  }
}
```

A filter expression with one nested group, run through `nlapiSearchRecord`:

```js
function findWeakOpportunities(departmentId) {
  var filters = [
    ['probability', 'lessthan', 30],
    'and',
    [['department', 'anyof', departmentId], 'or', ['department', 'anyof', '@NONE@']],
    'and',
    ['expectedclosedate', 'onorbefore', 'today']
  ];
  var columns = [new nlobjSearchColumn('title'), new nlobjSearchColumn('projectedtotal')];
  var rows = nlapiSearchRecord('opportunity', null, filters, columns);
  return rows || [];
}
```
