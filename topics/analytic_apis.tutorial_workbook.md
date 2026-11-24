# Workbook tutorial (Workbook API)
<!-- id: topic:analytic_apis.tutorial_workbook -->

Part of: [SuiteScript Analytic APIs](analytic_apis.md)

Sub-topics:

- [Workbook tutorial pivot steps](analytic_apis.tutorial_workbook.pivot.md)

See also: [N/workbook](../modules/N_workbook.md), [N/dataset](../modules/N_dataset.md),
[Workbook API workbook concepts](analytic_apis.workbooks.md),
[Dataset tutorial](analytic_apis.tutorial_dataset.md),
[Dataset Builder Plug-in](dataset_builder_plugin.md), [Workbook Builder Plug-in](workbook_builder_plugin.md).

This page condenses the workbook walkthrough. It builds a dataset, then a workbook holding a table
view (with conditional formatting) and a pivot (with data dimensions and measures). Both are put into
the account through plug-ins: the dataset through the
[Dataset Builder Plug-in](dataset_builder_plugin.md), the workbook through the
[Workbook Builder Plug-in](workbook_builder_plugin.md). The code below is written for this page.

## Before you start

| Need | Detail |
|---|---|
| Features | `SuiteAnalytics Workbook` must be enabled, plus at least one of `Server SuiteScript` or `Client SuiteScript` (either one is enough) |
| Permissions | access to the Analytics tab; permission to view datasets and workbooks; permission to create datasets and workbooks |

> [!SOURCE-ERROR] The tutorial's prerequisites let `Client SuiteScript` stand in for
> `Server SuiteScript`, while the Analytic APIs overview says these APIs run on the server only. The
> source does not resolve the conflict; treat the plug-ins as server-side and check your account
> before depending on `Client SuiteScript` by itself.
<!-- source-error: topic:analytic_apis.tutorial_workbook | - | prereq_allows_client_suitescript_overview_says_server_only -->

## Order of work

1. Check the prerequisites.
2. Read through the complete scripts.
3. Create the dataset.
4. Set up the workbook.
5. Create the table view.
6. Create the pivot (see [Workbook tutorial pivot steps](analytic_apis.tutorial_workbook.pivot.md)).

## The dataset, through the dataset builder plug-in

- The dataset starts from transactions and joins to transaction lines, then from the lines on to
  transaction accounting lines. The walkthrough names these joins `transactionlines` and
  `accountingimpact`.
- The plug-in's entry point is `createDataset`. NetSuite calls it when someone opens the dataset
  list.
- The plug-in script returns its entry point inside an object passed through `Object.freeze`.
- Inside `createDataset`, put the dataset you built on `context.dataset`.

## The workbook, through the workbook builder plug-in

- The plug-in's entry point is `createWorkbook`. NetSuite calls it when someone opens the workbook
  list.
- The ID the workbook uses to reach the dataset is the script ID of the dataset builder plug-in.
- The walkthrough keeps sort definitions short with a small helper function around
  [`workbook.createSort()`](../modules/N_workbook.md).

## Table view with conditional formatting

Build the pieces in this order:

1. [`workbook.createConditionalFormatRule()`](../modules/N_workbook.md), once per rule.
2. [`workbook.createConditionalFormat()`](../modules/N_workbook.md), to group the rules.
3. [`workbook.createTableColumn()`](../modules/N_workbook.md), once per column. Each table column
   refers to a dataset column by that column's alias.
4. [`workbook.createTable()`](../modules/N_workbook.md), with the dataset and the table columns.

Rules are evaluated in order, and **the first rule that matches decides the format**. In the
walkthrough, one rule paints values below 10 yellow and a later rule paints values of 0 or more pink;
a value of 5 satisfies both, and it ends up yellow because the yellow rule is listed first.

## Examples

A dataset builder plug-in for the general-ledger view: transactions with their posting lines'
debits and credits. The join and field IDs are illustrative; confirm them in the
[Records Catalog](records_catalog.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType datasetbuilderplugin
 */
define(['N/dataset'], (dataset) => {
  const createDataset = (context) => {
    const lines = dataset.createJoin({ fieldId: 'transactionlines' });
    const impact = dataset.createJoin({ fieldId: 'accountingimpact', join: lines });

    context.dataset = dataset.create({
      type: 'transaction',
      columns: [
        dataset.createColumn({ fieldId: 'trandate', alias: 'posted_on' }),
        dataset.createColumn({ fieldId: 'debit', join: impact, alias: 'debit_amt' }),
        dataset.createColumn({ fieldId: 'credit', join: impact, alias: 'credit_amt' })
      ]
    });
  };

  return Object.freeze({ createDataset });
});
```

A workbook builder plug-in that uses that dataset. It loads the dataset by the dataset plug-in's
script ID (invented here) and shades the debit column with two overlapping rules: a debit of 40
matches both, and it is shown orange because the orange rule comes first.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType workbookbuilderplugin
 */
define(['N/workbook', 'N/dataset', 'N/query'], (workbook, dataset, query) => {
  const rule = (operator, limit, color) => workbook.createConditionalFormatRule({
    filter: workbook.createTableColumnFilter({ operator, values: [limit] }),
    style: workbook.createStyle({ backgroundColor: color })
  });

  const createWorkbook = (context) => {
    const glLines = dataset.load({ id: 'customscript_gl_lines_ds' });

    const debitFormat = workbook.createConditionalFormat({
      rules: [
        rule(query.Operator.LESS, 100, workbook.Color.ORANGE),
        rule(query.Operator.GREATER_OR_EQUAL, 0, workbook.Color.GREEN)
      ]
    });

    const table = workbook.createTable({
      id: 'gl_lines_table',
      name: 'Posting lines',
      dataset: glLines,
      columns: [
        workbook.createTableColumn({ datasetColumnAlias: 'posted_on', conditionalFormats: [] }),
        workbook.createTableColumn({ datasetColumnAlias: 'debit_amt', conditionalFormats: [debitFormat] }),
        workbook.createTableColumn({ datasetColumnAlias: 'credit_amt', conditionalFormats: [] })
      ]
    });

    context.workbook = workbook.create({ description: 'Debits and credits by posting date', tables: [table] });
  };

  return Object.freeze({ createWorkbook });
});
```
