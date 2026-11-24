# Dataset tutorial (Workbook API)
<!-- id: topic:analytic_apis.tutorial_dataset -->

Part of: [SuiteScript Analytic APIs](analytic_apis.md)

See also: [N/dataset](../modules/N_dataset.md), [N/workbook](../modules/N_workbook.md),
[N/query](../modules/N_query.md), [Workbook API dataset concepts](analytic_apis.datasets.md),
[Records Catalog](records_catalog.md).

This page condenses the dataset walkthrough: a script builds a customer dataset piece by piece, and
you then confirm the result in the SuiteAnalytics Workbook UI. The code at the end is written for
this page and is not the walkthrough's own.

## Before you start

| Need | Detail |
|---|---|
| Features | `SuiteAnalytics Workbook` must be enabled, plus at least one of `Server SuiteScript` or `Client SuiteScript` (either one is enough) |
| Permissions | access to the Analytics tab; permission to view datasets and workbooks; permission to create datasets and workbooks |
| Debugger | permission to run server scripts in the SuiteScript debugger |

> [!SOURCE-ERROR] The tutorial's prerequisites accept `Client SuiteScript` as an alternative to
> `Server SuiteScript`, but the Analytic APIs overview describes these APIs as server-only. The source
> does not settle which is right; plan on server-side scripts and verify in your account before
> relying on `Client SuiteScript` alone.
<!-- source-error: topic:analytic_apis.tutorial_dataset | - | prereq_allows_client_suitescript_overview_says_server_only -->

All three Workbook API modules, [N/dataset](../modules/N_dataset.md),
[N/datasetLink](../modules/N_datasetLink.md) and [N/workbook](../modules/N_workbook.md), depend on the
`SuiteAnalytics Workbook` feature. The walkthrough recommends loading the dataset and workbook modules
together.

The walkthrough's code is meant to be run in the SuiteScript debugger. To check what it built, open
the dataset afterwards in the SuiteAnalytics Workbook UI.

## The steps

1. Load the N/dataset module.
2. Create the dataset.
3. Create its joins, columns and conditions.
4. Attach those components to the dataset.
5. Save the dataset, then run it.

## Root record

The walkthrough's dataset starts from the customer record. In the UI you can see a dataset's root
record type in its dataset information popup. Take the root record type name from the
[Records Catalog](records_catalog.md).

## Joins

The walkthrough shows two kinds of join:

- an ordinary join that follows the customer's sales rep field;
- an inverse join, which runs from the customer to the transactions that reference it.

A join that starts from another record type needs two things: `source`, the record type you are
joining from, and `fieldId`, the field on that record type that points back. In the UI, fields that
come through a join are highlighted at the top of the fields list.

## Columns

The walkthrough adds three columns: the customer's entity ID (`entityid`), the sales rep's email
reached through the sales rep join, and the transaction date (`trandate`) reached through the inverse
join.

## Conditions

- A field you filter on must also be a column of the dataset.
- A dataset holds one condition only. To filter on several things, wrap them as children of a single
  condition whose operator is AND.
- Condition operators are the values of the [`query.Operator`](../modules/N_query.md) enum.
- Date values are written in the date format the running user has set in their preferences.
- The walkthrough filters out inactive customers (`isinactive`) and keeps transactions dated on or
  before March 3, 2020.
- The condition applies to every workbook built on the dataset. Inside a workbook it is shown in the
  criteria summary above the data grid.

## Saving and running

- A dataset ID must begin with `custdataset`.
- Save the dataset before you look for it in the UI.
- Running the dataset gives a [`query.ResultSet`](../modules/N_query.md); running it in pages gives
  [`query.PagedData`](../modules/N_query.md).

## Example

Customers together with their sales orders from a recent period, built with an inverse join from the
transaction record. The `source` value and field IDs are illustrative; confirm them in the Records
Catalog. The date string has to match the running user's date format. Per
[`dataset.createCondition()`](../modules/N_dataset.md), a condition that has children also accepts
the operator string `'AND'` (or `'OR'`).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/dataset', 'N/query', 'N/log'], (dataset, query, log) => {
  const execute = () => {
    const ordersJoin = dataset.createJoin({ fieldId: 'entity', source: 'transaction' });

    const custCol = dataset.createColumn({ fieldId: 'entityid', alias: 'customer' });
    const dateCol = dataset.createColumn({ fieldId: 'trandate', join: ordersJoin, alias: 'order_date' });
    const typeCol = dataset.createColumn({ fieldId: 'type', join: ordersJoin, alias: 'order_type' });

    const recentOrders = dataset.createCondition({
      operator: 'AND',
      children: [
        dataset.createCondition({ column: typeCol, operator: query.Operator.ANY_OF, values: ['SalesOrd'] }),
        dataset.createCondition({ column: dateCol, operator: query.Operator.ON_OR_AFTER, values: ['10/1/2026'] })
      ]
    });

    const ds = dataset.create({
      type: 'customer',
      columns: [custCol, dateCol, typeCol],
      condition: recentOrders
    });
    ds.save({ name: 'Customers with recent orders', id: 'custdataset_recent_orders' });

    ds.run().asMappedResults().forEach((row) => {
      log.debug({ title: row.customer, details: `${row.order_type} on ${row.order_date}` });
    });
  };

  return { execute };
});
```
