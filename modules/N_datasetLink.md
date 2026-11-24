# N/datasetLink
<!-- id: N/datasetLink -->

`N/datasetLink` ties two datasets together so a workbook can treat them as one source. Use it when
the record types behind the datasets cannot be joined explicitly inside a single dataset. The link
is logical only: the datasets are not merged and no join is created. You simply declare which column
in each dataset carries the shared data; a date column that both datasets have is the usual case.

- **Availability:** server scripts.
- **Load it as:** `define(['N/datasetLink'], (datasetLink) => { ... })`
- Related: [N/dataset](N_dataset.md), [N/workbook](N_workbook.md),
  [Workbook API dataset concepts](../topics/analytic_apis.datasets.md),
  [Workbook Builder Plug-in](../topics/workbook_builder_plugin.md),
  [SuiteScript Analytic APIs](../topics/analytic_apis.md).

**Typical sequence.**

1. Build the datasets with [N/dataset](N_dataset.md) and save them. A dataset has to be saved before
   it can take part in a link.
2. Load the saved datasets. The spec places this step inside a
   [Workbook Builder Plug-in](../topics/workbook_builder_plugin.md) implementation, the plug-in type
   whose `createWorkbook(context)` function builds a workbook with N/workbook and N/dataset.
3. On each dataset, call
   [`Dataset.getExpressionFromColumn`](N_dataset.md#datasetgetexpressionfromcolumnoptions) for the
   column the datasets have in common.
4. Call [`datasetLink.create(options)`](#datasetlinkcreateoptions) with the datasets, those
   expressions and an id.
5. Use the link as the data source of a pivot, through
   [`Pivot.datasetLink`](N_workbook.md#pivotdatasetlink) in N/workbook.

Rules from the [dataset concepts topic](../topics/analytic_apis.datasets.md): the datasets need a
common field, the linking columns must share a data type, and linked datasets can be used only in
pivots.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`datasetLink.create(options)`](#datasetlinkcreateoptions) | function | Link datasets on a shared column |
| [`DatasetLink`](#datasetlink-object) | object | The resulting link |

## Functions

### datasetLink.create(options)
<!-- id: N/datasetLink.create -->

Creates a [`DatasetLink`](#datasetlink-object). Every dataset you pass needs a column holding the same
data as the others; you identify those columns with expressions obtained from
[`Dataset.getExpressionFromColumn`](N_dataset.md#datasetgetexpressionfromcolumnoptions). Linking on a
date column that both datasets share is the standard illustration.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | No | |
| `options.expressions` | Array<workbook.Expression[]> | Yes | |
| `options.datasets` | dataset.Dataset[] | Yes | |

- `options.id`: the link's id. Leave it out and an id is generated for you.
- `options.expressions`: an array of arrays. Going by the source's syntax sample, each inner array
  holds one expression per dataset, in the same order as `options.datasets`.
- `options.datasets`: the datasets to link. They must already be saved.

**Returns:** `datasetLink.DatasetLink`

**Errors:**

- `NO_DATASET_DEFINED`: `options.datasets` is missing or empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

## Objects

### DatasetLink (object)
<!-- id: N/datasetLink.DatasetLink -->

Two datasets that have been linked, as returned by
[`datasetLink.create(options)`](#datasetlinkcreateoptions). Assign it to a pivot's `datasetLink` to
use it. Server scripts, since 2021.2.

#### DatasetLink.datasets
<!-- id: N/datasetLink.DatasetLink#datasets -->

`dataset.Dataset[]`. The datasets the link connects. Since 2021.2.

#### DatasetLink.expressions
<!-- id: N/datasetLink.DatasetLink#expressions -->

`Array<workbook.Expression[]>`. The expressions for the columns the datasets are linked on. Since
2021.2.

#### DatasetLink.id
<!-- id: N/datasetLink.DatasetLink#id -->

`string`, read-only. The id of the link. Since 2021.2.

## Examples

A small custom module meant to be called from a Workbook Builder Plug-in implementation. It links a
saved dataset of support cases with a saved dataset of time entries on the date column both share,
so a pivot can set case volume against hours logged per day. The dataset ids and aliases are
invented; both datasets must already be saved, with an alias on their date columns. The inner array
lists one expression per dataset, in the order of `datasets`.

```js
/**
 * @NApiVersion 2.1
 * @NModuleScope SameAccount
 */
define(['N/dataset', 'N/datasetLink'], (dataset, datasetLink) => {
  const linkCasesWithHours = () => {
    const cases = dataset.load({ id: 'custdataset_cases_by_day' });
    const hours = dataset.load({ id: 'custdataset_hours_by_day' });

    const caseDate = cases.getExpressionFromColumn({ alias: 'opened_on' });
    const hoursDate = hours.getExpressionFromColumn({ alias: 'worked_on' });

    return datasetLink.create({
      id: 'cases_vs_hours',
      datasets: [cases, hours],
      expressions: [[caseDate, hoursDate]]
    });
  };

  return { linkCasesWithHours };
});
```
