# Workbook Builder Plug-in
<!-- id: topic:workbook_builder_plugin -->

Sub-topics:

- [Workbook Builder Plug-in context](workbook_builder_plugin.context.md)

See also: [SuiteScript Analytic APIs](analytic_apis.md), [N/workbook](../modules/N_workbook.md),
[N/dataset](../modules/N_dataset.md), [Dataset Builder Plug-in](dataset_builder_plugin.md),
[Workbook API workbook concepts](analytic_apis.workbooks.md),
[Workbook tutorial](analytic_apis.tutorial_workbook.md).

The Workbook Builder Plug-in lets your own code produce the workbook that the Workbook API works
with. The plug-in script implements a single entry function, `createWorkbook`, which assembles a
workbook with [N/workbook](../modules/N_workbook.md) (on top of datasets from
[N/dataset](../modules/N_dataset.md)) and passes it back on the context object.

## Why use it

A workbook that an ordinary script creates with N/workbook is static: it does not adapt to the user
or role who opens it later. A plug-in builds the workbook in code instead, which is meant for cases
such as:

- a workbook based on data specific to a user;
- a workbook based on data specific to a role;
- a workbook that applies custom logic or shows only part of the data.

The source does not state how the plug-in finds out which user or role is viewing the workbook at
run time. The `owner` and `role` values on the context object identify the person who added the
plug-in implementation, not the viewer (see [the context page](workbook_builder_plugin.context.md)).

## At a glance

| Item | Value |
|---|---|
| Entry (interface) function | `createWorkbook(context)` |
| JSDoc script-type tag | `@NScriptType workbookbuilderplugin` |
| SuiteScript versions | 2.0 and 2.1; helper library files the plug-in uses must be on these versions too |
| Maximum usage units | 1,000 |
| Time limit | 300 s (Workbook Builder plug-in row of [catalog:other_time_limits](../gen/catalogs/other_time_limits.md)) |
| SDF object type | `workbookbuilderplugin`, script ID prefix `customscript_` (see [catalog:sdf_object_types](../gen/catalogs/sdf_object_types.md)) |
| Required features | `Server SuiteScript` and `Workbook API` |

Because SuiteCloud Development Framework supports the `workbookbuilderplugin` object type, the plug-in
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
  For this plug-in there is only `createWorkbook`.
- The file name is your choice, but it must have the `.js` extension.
- The file may load helper or utility files.
- Any editor works: SuiteCloud IDE, another JavaScript IDE, or a plain text editor.
- Tag the file with `@NScriptType workbookbuilderplugin` in its JSDoc header.
- The source says plug-in code cannot use the JavaScript `case` statement, because of a limitation
  in how NetSuite handles strings. Branch with `if` / `else` or a lookup object instead (general
  JavaScript advice, not a SuiteScript rule).

The source's sample is written for API version 2.x, imports [N/workbook](../modules/N_workbook.md)
and [N/dataset](../modules/N_dataset.md), and returns a frozen object (`Object.freeze`) exposing the
entry function. It loads a dataset with [`dataset.load()`](../modules/N_dataset.md), builds the
workbook with [`workbook.create()`](../modules/N_workbook.md), and puts it on `context.workbook`. The
workbook has three visualizations:

- a table view;
- a pivot with row and column axes, sort definitions, a limiting filter, a conditional filter, and a
  filter expression of the "not any of" kind;
- a chart with a category, a legend, and a series that carries an aspect.

The sample's NOT expression passes its operand under the parameter name `expression`, which is what
the [`workbook.ExpressionType`](../modules/N_workbook.md) reference lists for `NOT`.

> [!SOURCE-ERROR] The sample calls `workbook.createConstant` with a bare number, but the reference
> defines `createConstant` as taking an options object (`options.constant`, optional `options.type`).
> Follow the reference signature and verify in your account.
<!-- source-error: topic:workbook_builder_plugin | - | sample_calls_createConstant_with_bare_number_not_options -->

The [workbook tutorial](analytic_apis.tutorial_workbook.md) adds that `createWorkbook` runs whenever
the workbook list is opened.

### createWorkbook(context)

The interface function NetSuite calls to get the workbook. Build a
[`workbook.Workbook`](../modules/N_workbook.md) and assign it to `context.workbook`; the function
itself returns nothing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |

- `context`: a plain JavaScript object with the properties `description`, `name`, `owner`, `role`
  and `workbook`, described on [Workbook Builder Plug-in context](workbook_builder_plugin.context.md).

**Returns:** void.

**Errors:**

- `NO_WORKBOOK_CREATED`: `context.workbook` was never assigned.
- `WRONG_PARAMETER_TYPE`: something other than a workbook object was assigned to `context.workbook`.
- `READ_ONLY`: the code tried to assign `description`, `name`, `owner` or `role`.
- `CAN_REFERENCE_ONLY_PERSISTED_DATASET`: the workbook refers to a dataset that has not been saved.
  Only datasets saved in the SuiteAnalytics Workbook UI or supplied by a dataset builder plug-in can
  be used.

**Governance:** one run of the plug-in may use at most 1,000 usage units.

### Limits and behaviour

- A value longer than its field's character limit is truncated.
- The workbook name shown in the SuiteAnalytics Workbook UI comes from the plug-in implementation
  record, not from the name your code passes to `workbook.create()`. Dataset names set in code, by
  contrast, are kept.

> [!SOURCE-ERROR] The source says this UI name comes from the plug-in's "deployment script record",
> yet a plug-in implementation has no deployment; what appears to be meant is the Name field of the
> plug-in implementation record. Confirm in your account if it matters.
<!-- source-error: topic:workbook_builder_plugin | - | ui_name_said_to_come_from_plugin_deployment_script_record -->

- A plug-in whose script file is set up wrongly makes the workbook fail with a "workbook unavailable"
  error (see [SuiteScript Analytic APIs](analytic_apis.md)).

## Testing and troubleshooting

- Before deploying, run the workbook-building code in the SuiteScript Debugger. Syntax errors are
  reported before anything executes; logic errors only appear while the code runs.
- To check the logic, run the visualizations in the debugger, for example a pivot with
  [`Workbook.runPivot()`](../modules/N_workbook.md), and inspect the output.
- Bad syntax or bad logic can produce a workbook that raises an error in the SuiteAnalytics Workbook
  UI. An invalid workbook still shows up in the workbook list but cannot be opened; trying to open it
  reports that the workbook does not exist or is no longer available.
- A workbook that is syntactically valid stays in the list, but logic errors can make its pivots or
  charts display wrongly.

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
| Name | The name shown in the list of plug-in implementations (and, for this plug-in, the workbook name shown in the UI). |
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

A plug-in written for this page: a stock-review table over a saved dataset, sorted by quantity on
hand, with quantities under 5 shaded orange. The dataset ID and column aliases are invented; the
dataset must already be saved.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType workbookbuilderplugin
 */
define(['N/workbook', 'N/dataset', 'N/query'], (workbook, dataset, query) => {
  const lowStockFormat = () => workbook.createConditionalFormat({
    rules: [
      workbook.createConditionalFormatRule({
        filter: workbook.createTableColumnFilter({ operator: query.Operator.LESS, values: [5] }),
        style: workbook.createStyle({ backgroundColor: workbook.Color.ORANGE })
      })
    ]
  });

  const createWorkbook = (context) => {
    const stock = dataset.load({ id: 'custdataset_bin_stock' });

    const table = workbook.createTable({
      id: 'stock_review',
      name: 'Stock review',
      dataset: stock,
      columns: [
        workbook.createTableColumn({ datasetColumnAlias: 'bin', conditionalFormats: [] }),
        workbook.createTableColumn({ datasetColumnAlias: 'sku', conditionalFormats: [] }),
        workbook.createTableColumn({
          datasetColumnAlias: 'on_hand',
          conditionalFormats: [lowStockFormat()],
          sort: workbook.createSort({ ascending: true })
        })
      ]
    });

    context.workbook = workbook.create({
      description: 'Bin stock with low quantities highlighted',
      tables: [table]
    });
  };

  return Object.freeze({ createWorkbook });
});
```
