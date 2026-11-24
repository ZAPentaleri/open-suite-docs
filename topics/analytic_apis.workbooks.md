# Workbook API workbook concepts
<!-- id: topic:analytic_apis.workbooks -->

Part of: [SuiteScript Analytic APIs](analytic_apis.md)

See also: [N/workbook](../modules/N_workbook.md), [N/dataset](../modules/N_dataset.md),
[N/datasetLink](../modules/N_datasetLink.md), [Workbook API dataset concepts](analytic_apis.datasets.md),
[Workbook tutorial](analytic_apis.tutorial_workbook.md), [Workbook Builder Plug-in](workbook_builder_plugin.md).

A workbook is the presentation layer that sits on top of datasets. It carries a name, a description,
and any number of table views and pivots. This page explains how the pieces fit together; parameter
details are on the [N/workbook](../modules/N_workbook.md) page.

## The workbook itself

Create it with [`workbook.create()`](../modules/N_workbook.md).

- A workbook contains a description, a name, pivots and tables.
- The dataset is not attached to the workbook. Each visualization (table or pivot) names its own
  dataset, so a single workbook can draw on several datasets.
- Every field a visualization displays must exist as a column in that visualization's dataset.
- A workbook produced for the [Workbook Builder Plug-in](workbook_builder_plugin.md) needs a name
  and a description.

## Table views

Create a table view with [`workbook.createTable()`](../modules/N_workbook.md). It needs `columns`,
`dataset`, `id` and `name`.

Each column comes from [`workbook.createTableColumn()`](../modules/N_workbook.md), which needs
`datasetColumnAlias` and `conditionalFormats`. The alias must already be defined as a column alias in
the dataset.

### Conditional formatting

Formatting is built from the bottom up:

1. [`workbook.createTableColumnFilter()`](../modules/N_workbook.md): an operator plus values. The
   operators are those of the [`query.Operator`](../modules/N_query.md) enum, or the equivalent
   strings.
2. [`workbook.createStyle()`](../modules/N_workbook.md): the look to apply, for example a cell
   background color, a font size or a font style.
3. [`workbook.createConditionalFormatRule()`](../modules/N_workbook.md): pairs a `filter` with a
   `style`; both are required.
4. [`workbook.createConditionalFormat()`](../modules/N_workbook.md): groups one or more rules.
5. [`workbook.createTableColumn()`](../modules/N_workbook.md): attaches the conditional formats to a
   column.

Styles made with `createStyle` serve two purposes: conditional format rules on tables and report
style rules on pivots.

## Pivots

Create a pivot with [`workbook.createPivot()`](../modules/N_workbook.md). According to this topic it
needs a `columnAxis`, an `id`, a `name`, and either a `dataset` or a `datasetLink`; you give one of
those two, never both. Run a pivot with [`Workbook.runPivot()`](../modules/N_workbook.md).

> [!SOURCE-ERROR] The reference for `createPivot` lists `dataset` as required and has no `datasetLink`
> parameter, while this concept material lets a pivot use a dataset link instead. The source does not
> settle which is right; test pivots on linked datasets before relying on them.
<!-- source-error: topic:analytic_apis.workbooks | - | createPivot_reference_omits_datasetLink -->

### Dimensions

1. Create data dimension items with
   [`workbook.createDataDimensionItem()`](../modules/N_workbook.md). An item's expression normally
   comes from [`Dataset.getExpressionFromColumn()`](../modules/N_dataset.md).
2. Combine the items into a data dimension with
   [`workbook.createDataDimension()`](../modules/N_workbook.md).
3. Place the dimension on a pivot axis.

A typical example is transaction type broken down by posting period. The label of a dimension item is
what the workbook UI displays. Where a dimension's total line goes is chosen from the
[`workbook.TotalLine`](../modules/N_workbook.md) enum, and a dimension can nest child dimensions or
measures under it.

### Measures

There are two kinds:

| Kind | Builder | What it computes |
|---|---|---|
| Data measure | [`workbook.createDataMeasure()`](../modules/N_workbook.md) | An aggregation over values; the aggregation is taken from [`workbook.Aggregation`](../modules/N_workbook.md) |
| Calculated measure | [`workbook.createCalculatedMeasure()`](../modules/N_workbook.md) | Arithmetic on other values |

- A data measure takes `expression` when it has one expression, or `expressions` when it has
  several.
- At the top level, a calculated measure's expression must be a plus, minus, divide or multiply
  function. Other operand types may appear further down, inside the nested expressions.
- Typical uses: counting unique values (data measure) and the difference between two measures
  (calculated measure).

### Axes and sorting

- An axis root is either a single data dimension or, when the axis holds several, a section.
- A sort definition may refer only to items of the root dimension.

### Selectors

Selectors choose which rows, columns or measures something applies to.

| Selector | Builder |
|---|---|
| `DimensionSelector` | `workbook.createDimensionSelector()` |
| `MeasureSelector` | `workbook.createMeasureSelector()` |
| `MeasureValueSelector` | `workbook.createMeasureValueSelector()` |
| `PathSelector` | `workbook.createPathSelector()` |
| `ChildNodesSelector` | none (no create method) |
| `DescendantorSelfNodesSelector` | none (no create method) |

The source does not state how to obtain the two selectors that have no create method.

## Expressions

Expressions come from two places:

- **Column expressions**, from [`Dataset.getExpressionFromColumn()`](../modules/N_dataset.md). Name
  the column either by `alias` or by `columnId`, not both. Column expressions feed
  `workbook.createCalculatedMeasure()`, `workbook.createDataDimensionItem()`,
  `workbook.createDataMeasure()` and `workbook.createReportStyleRule()`.
- **Custom expressions**, from [`workbook.createExpression()`](../modules/N_workbook.md).

## Example

A library module written for this page: it builds a table view over a saved dataset and paints the
days-overdue cell red from 30 days up. The dataset ID and column aliases are invented.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/workbook', 'N/dataset', 'N/query'], (workbook, dataset, query) => {
  const buildAgingWorkbook = () => {
    const ds = dataset.load({ id: 'custdataset_open_ar' });

    const overdueStyle = workbook.createStyle({
      backgroundColor: workbook.Color.RED,
      color: workbook.Color.WHITE
    });
    const overdueRule = workbook.createConditionalFormatRule({
      filter: workbook.createTableColumnFilter({
        operator: query.Operator.GREATER_OR_EQUAL,
        values: [30]
      }),
      style: overdueStyle
    });

    const columns = [
      workbook.createTableColumn({ datasetColumnAlias: 'cust_name', conditionalFormats: [] }),
      workbook.createTableColumn({
        datasetColumnAlias: 'days_overdue',
        conditionalFormats: [workbook.createConditionalFormat({ rules: [overdueRule] })]
      })
    ];

    const table = workbook.createTable({
      id: 'aging_view',
      name: 'Open receivables aging',
      dataset: ds,
      columns
    });

    return workbook.create({
      name: 'Collections review',
      description: 'Open receivables with overdue highlighting',
      tables: [table]
    });
  };

  return { buildAgingWorkbook };
});
```
