# SuiteScript Analytic APIs
<!-- id: topic:analytic_apis -->

Sub-topics:

- [SuiteQL](suiteql.md)
- [Workbook API dataset concepts](analytic_apis.datasets.md)
- [Workbook API workbook concepts](analytic_apis.workbooks.md)
- [Dataset tutorial (Workbook API)](analytic_apis.tutorial_dataset.md)
- [Workbook tutorial (Workbook API)](analytic_apis.tutorial_workbook.md)

See also: [N/query](../modules/N_query.md), [N/dataset](../modules/N_dataset.md),
[N/datasetLink](../modules/N_datasetLink.md), [N/workbook](../modules/N_workbook.md),
[Records Catalog](records_catalog.md), [Dataset Builder Plug-in](dataset_builder_plugin.md),
[Workbook Builder Plug-in](workbook_builder_plugin.md).

The analytic APIs give a script the SuiteAnalytics view of NetSuite data instead of the saved-search
view. All of them run in server scripts only; none is available to client scripts.

## Which module does what

| Approach | Module(s) |
|---|---|
| Query objects | [N/query](../modules/N_query.md) |
| SuiteQL statements | [N/query](../modules/N_query.md) (see [SuiteQL](suiteql.md)) |
| Datasets and workbooks (the Workbook API) | [N/dataset](../modules/N_dataset.md), [N/datasetLink](../modules/N_datasetLink.md), [N/workbook](../modules/N_workbook.md) |

- The query API covers paged queries, joins and conditions.
- [N/dataset](../modules/N_dataset.md) fills roughly the role N/query fills, with one difference:
  what you build with it is a dataset that workbooks can use.
- The SuiteQL support you reach from SuiteScript does not include the SuiteAnalytics Connect channel;
  Connect is a separate route to SuiteQL with its own setup.

## The analytics data source

- Each of these APIs reads the analytics data source. That source is not the one saved searches
  read, so a record or field name that works in a search may not exist here, and the reverse.
- When you need analytics record and field names, look them up in the
  [Records Catalog](records_catalog.md). Do not take them from the Records Browser: it documents the
  search data source.

## What the Workbook API can do

The Workbook API (N/dataset, N/datasetLink, N/workbook) needs the feature `SuiteAnalytics Workbook`
enabled in the account. With it, a script can:

- create datasets, and give them conditions and joins;
- create workbooks that contain pivots and table views, and run the table views;
- load datasets and workbooks that already exist;
- put limiting filters and conditional filters on a pivot, and define pivot axes with sorting.

According to this topic, charting is not supported.

> [!SOURCE-ERROR] The topic says charts cannot be built, while [N/workbook](../modules/N_workbook.md)
> documents a chart builder (`workbook.createChart`). The source does not settle which is right; try
> chart creation in your own account before depending on it.
<!-- source-error: topic:analytic_apis | - | charting_unavailable_vs_documented_createChart -->

The topic also lists running table views among the capabilities, yet the only run method documented
on a workbook is [`Workbook.runPivot()`](../modules/N_workbook.md), which runs a pivot.

> [!SOURCE-ERROR] Running table views is claimed, but no method for it is documented; only
> `Workbook.runPivot()` exists. Treat running a table view from a script as unconfirmed.
<!-- source-error: topic:analytic_apis | - | run_table_views_claimed_only_runPivot_documented -->

## Dataset Builder and Workbook Builder plug-ins

Two plug-in types let your own code supply the dataset or workbook instead of a fixed, saved one:

- the [Dataset Builder Plug-in](dataset_builder_plugin.md), whose script implements
  `createDataset(context)` and hands the dataset back on the context object (see
  [Dataset Builder Plug-in context](dataset_builder_plugin.context.md));
- the [Workbook Builder Plug-in](workbook_builder_plugin.md), whose script implements
  `createWorkbook(context)` and hands the workbook back the same way (see
  [Workbook Builder Plug-in context](workbook_builder_plugin.context.md)).

Each plug-in script carries its own JSDoc script-type tag (`datasetbuilderplugin` or
`workbookbuilderplugin`) and is installed as a plug-in implementation record, as described on the two
plug-in pages. The [workbook tutorial](analytic_apis.tutorial_workbook.md) uses both.

If a plug-in's script file is set up incorrectly, the workbook built from it fails to open with an
error saying the workbook is unavailable.

## Example

A short scheduled script that reads analytics data through a saved dataset and logs each row. The
dataset id is invented; it must be the id of a dataset already saved in the account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/dataset', 'N/log'], (dataset, log) => {
  const execute = () => {
    const openCases = dataset.load({ id: 'custdataset_open_support_cases' });
    const rows = openCases.run().asMappedResults();
    rows.forEach((row, index) => {
      log.audit({ title: `Case row ${index + 1}`, details: JSON.stringify(row) });
    });
  };

  return { execute };
});
```
