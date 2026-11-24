# Dataset Builder Plug-in
<!-- id: topic:dataset_builder_plugin -->

Sub-topics:

- [Dataset Builder Plug-in context](dataset_builder_plugin.context.md)

See also: [SuiteScript Analytic APIs](analytic_apis.md), [N/dataset](../modules/N_dataset.md),
[N/workbook](../modules/N_workbook.md), [Workbook Builder Plug-in](workbook_builder_plugin.md),
[Workbook tutorial](analytic_apis.tutorial_workbook.md).

The Dataset Builder Plug-in lets your own code produce the dataset that the Workbook API works with.
The plug-in script implements a single entry function, `createDataset`, which assembles a dataset
with [N/dataset](../modules/N_dataset.md) and passes it back on the context object.

## Why use it

A dataset that an ordinary script creates and saves with N/dataset is static: once saved it is fixed,
and it has no notion of the user or role who opens it later. A plug-in builds the dataset in code
instead, which is meant for cases such as:

- a dataset based on data specific to a user;
- a dataset based on data specific to a role;
- a dataset that applies custom logic or exposes only part of the data.

The source does not state how the plug-in finds out which user or role is viewing the dataset at run
time. The `owner` and `role` values on the context object identify the person who added the plug-in
implementation, not the viewer (see [the context page](dataset_builder_plugin.context.md)).

## At a glance

| Item | Value |
|---|---|
| Entry (interface) function | `createDataset(context)` |
| JSDoc script-type tag | `@NScriptType datasetbuilderplugin` |
| SuiteScript versions | 2.0 and 2.1; helper library files the plug-in uses must be on these versions too |
| Maximum usage units | 1,000 |
| Time limit | 300 s (Dataset Builder plug-in row of [catalog:other_time_limits](../gen/catalogs/other_time_limits.md)) |
| SDF object type | `datasetbuilderplugin`, script ID prefix `customscript_` (see [catalog:sdf_object_types](../gen/catalogs/sdf_object_types.md)) |
| Required features | `Server SuiteScript` and `Workbook API` |

Because SuiteCloud Development Framework supports the `datasetbuilderplugin` object type, the plug-in
can live in an SDF project.

The documentation for this plug-in is split by role. Developers write the plug-in script;
administrators install and look after the plug-in implementation in an account. Both start by
enabling the same features.

## Developing the plug-in

### 1. Enable the features

In the development account, go to Setup > Company > Enable Features, open the SuiteCloud subtab, and
in its SuiteScript section check both boxes: `Server SuiteScript` and `Workbook API`.

### 2. Write the plug-in script file

- All interface functions go into one JavaScript file, and every one of them must be implemented.
  For this plug-in there is only `createDataset`.
- The file name is your choice, but it must have the `.js` extension.
- The file may load helper or utility files.
- Any editor works: SuiteCloud IDE, another JavaScript IDE, or a plain text editor.
- Tag the file with `@NScriptType datasetbuilderplugin` in its JSDoc header.
- The source says plug-in code cannot use the JavaScript `case` statement, because of a limitation
  in how NetSuite handles strings. Branch with `if` / `else` or a lookup object instead (general
  JavaScript advice, not a SuiteScript rule).

The source's sample is written for API version 2.x, imports only
[N/dataset](../modules/N_dataset.md), and returns a frozen object (`Object.freeze`) exposing the entry
function. It creates a transaction dataset with [`dataset.create()`](../modules/N_dataset.md), joins
to the created-by entity with [`dataset.createJoin()`](../modules/N_dataset.md), defines four
columns with aliases through [`dataset.createColumn()`](../modules/N_dataset.md), and puts the result
on `context.dataset`.

The [workbook tutorial](analytic_apis.tutorial_workbook.md) adds that `createDataset` runs whenever
the dataset list is opened, and that a workbook reaches the plug-in's dataset by the plug-in's script
ID.

### createDataset(context)

The interface function NetSuite calls to get the dataset. Build a
[`dataset.Dataset`](../modules/N_dataset.md) and assign it to `context.dataset`; the function itself
returns nothing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |

- `context`: a plain JavaScript object with the properties `dataset`, `description`, `name`, `owner`
  and `role`, described on [Dataset Builder Plug-in context](dataset_builder_plugin.context.md).

**Returns:** void.

**Errors:**

- `NO_DATASET_CREATED`: `context.dataset` was never assigned.
- `WRONG_PARAMETER_TYPE`: something other than a dataset object was assigned to `context.dataset`.
- `READ_ONLY`: the code tried to assign `description`, `name`, `owner` or `role`.

**Governance:** one run of the plug-in may use at most 1,000 usage units.

### Limits and behaviour

- A value longer than its field's character limit is truncated.
- A plug-in whose script file is set up wrongly makes the workbook fail with a "workbook unavailable"
  error (see [SuiteScript Analytic APIs](analytic_apis.md)).

## Testing and troubleshooting

- Before deploying, run the dataset-building code in the SuiteScript Debugger. Syntax errors are
  reported before anything executes; logic errors only appear while the code runs.
- To check the logic, run the dataset in the debugger with
  [`Dataset.run()`](../modules/N_dataset.md) and inspect the rows that come back.
- Bad syntax or bad logic can produce a dataset that raises an error in the SuiteAnalytics Workbook
  UI. An invalid dataset is left out of the dataset list and cannot be used by workbooks.

## Administering the plug-in

### 1. Enable the features

Enable `Server SuiteScript` and `Workbook API` in the account, the same way as under "Developing the
plug-in".

### 2. Add the plug-in implementation

Go to Customization > Plug-ins > Plug-in Implementations > New, which opens the page for uploading a
plug-in implementation. Then:

1. Add the script file with the plus icon next to "Script File".
2. Choose the file and save it.
3. Click "Create Plug-in Implementation".
4. Fill in the fields of the implementation record (table below).
5. On the "Unhandled Errors" subtab, choose who is notified of unhandled errors.
6. Save.

| Field | What it does |
|---|---|
| Name | The name shown in the list of plug-in implementations. |
| ID | The ID scripts use to refer to the implementation. Leave it blank and one is generated on save. |
| Status | `Testing`: only the owner can see the implementation. `Released`: ready for production use. |
| Log Level | `Debug`, `Audit`, `Error` or `Emergency`. Log entries appear on the "Execution Log" subtab. |
| Execute As Role | `Current Role` runs the plug-in with the permissions of the logged-in user. The source suggests testing with a custom role and then bundling that role together with the implementation. |
| Description | Optional. |
| Owner | Defaults to the current user. |
| Inactive | Turns the implementation off temporarily without deleting it. |

On the "Unhandled Errors" subtab, the option that notifies the script owner is checked by default.
Separate the addresses in "Notify Emails" with semicolons.

## Example

A plug-in written for this page: vendor bills with the vendor's name pulled in through a join. The
record and field IDs are illustrative; take the real analytics names from the
[Records Catalog](records_catalog.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType datasetbuilderplugin
 */
define(['N/dataset', 'N/query'], (dataset, query) => {
  const createDataset = (context) => {
    const vendorJoin = dataset.createJoin({ fieldId: 'entity' });

    const billType = dataset.createColumn({ fieldId: 'type', alias: 'tran_type' });

    const columns = [
      dataset.createColumn({ fieldId: 'tranid', alias: 'bill_no', label: 'Bill number' }),
      dataset.createColumn({ fieldId: 'trandate', alias: 'bill_date', label: 'Bill date' }),
      dataset.createColumn({ fieldId: 'foreigntotal', alias: 'bill_total', label: 'Bill total' }),
      dataset.createColumn({ fieldId: 'entityid', join: vendorJoin, alias: 'vendor', label: 'Vendor' }),
      billType
    ];

    context.dataset = dataset.create({
      type: 'transaction',
      columns,
      condition: dataset.createCondition({
        column: billType,
        operator: query.Operator.ANY_OF,
        values: ['VendBill']
      })
    });
  };

  return Object.freeze({ createDataset });
});
```
