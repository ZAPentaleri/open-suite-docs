# Workbook tutorial pivot steps
<!-- id: topic:analytic_apis.tutorial_workbook.pivot -->

Part of: [Workbook tutorial (Workbook API)](analytic_apis.tutorial_workbook.md)

See also: [N/workbook](../modules/N_workbook.md), [N/dataset](../modules/N_dataset.md),
[Workbook API workbook concepts](analytic_apis.workbooks.md),
[Workbook Builder Plug-in](workbook_builder_plugin.md).

The pivot part of the workbook walkthrough builds its parts in a set order and puts them together at
the end.

## Order of work

1. Data dimensions
2. Measures
3. Selectors
4. Report style
5. Axes
6. The pivot itself, and after it the workbook that holds it

## Dimensions

Every data dimension in the walkthrough contains exactly one item.

## Measures

The walkthrough demonstrates three kinds of measure:

| Measure | How it is built |
|---|---|
| Count of distinct transaction line IDs | A data measure with distinct-count aggregation |
| Credit and debit sums | Data measures with sum aggregation over a simple-consolidate expression of the amount |
| Credit minus debit | A calculated measure that subtracts one measure value from the other |

Simple consolidation converts amounts using the current user's subsidiary and the current period.

## Selectors and report style

Selectors decide which rows, columns and measures something applies to. To set up the report style
the walkthrough:

1. uses a descendant-or-self selector for both rows and columns,
2. creates a measure selector for the measure it wants to watch,
3. joins them together with [`workbook.createMeasureValueSelector()`](../modules/N_workbook.md).

A report style cannot do without such a measure value selector. In the walkthrough, the style
highlights cells whose measure goes above a threshold.

## Axes

- The row axis shows the date first, then the period.
- The column axis shows the transaction type, with its total placed first.
- Creating a pivot axis needs both a root element and a sort definition.

> [!SOURCE-ERROR] The walkthrough treats a sort definition as required for every pivot axis, while the
> reference for `workbook.createPivotAxis` marks `sortDefinitions` as optional. The source does not
> reconcile the two; supplying a sort definition is the safe choice.
<!-- source-error: topic:analytic_apis.tutorial_workbook.pivot | - | tutorial_requires_axis_sort_reference_marks_optional -->

## Example

Building the two sums and their difference, written for this page. Column aliases are invented;
`ds` is a loaded `dataset.Dataset`.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/workbook'], (workbook) => {
  const buildNetMeasures = (ds) => {
    const consolidated = (alias) => workbook.createExpression({
      functionId: workbook.ExpressionType.SIMPLE_CONSOLIDATE,
      parameters: { expression: ds.getExpressionFromColumn({ alias }) }
    });

    const creditSum = workbook.createDataMeasure({
      label: 'Credits',
      aggregation: workbook.Aggregation.SUM,
      expression: consolidated('credit_amt')
    });
    const debitSum = workbook.createDataMeasure({
      label: 'Debits',
      aggregation: workbook.Aggregation.SUM,
      expression: consolidated('debit_amt')
    });

    const net = workbook.createCalculatedMeasure({
      label: 'Net credit',
      expression: workbook.createExpression({
        functionId: workbook.ExpressionType.MINUS,
        parameters: {
          operand1: workbook.createExpression({
            functionId: workbook.ExpressionType.MEASURE_VALUE,
            parameters: { measure: creditSum }
          }),
          operand2: workbook.createExpression({
            functionId: workbook.ExpressionType.MEASURE_VALUE,
            parameters: { measure: debitSum }
          })
        }
      })
    });

    return { creditSum, debitSum, net };
  };

  return { buildNetMeasures };
});
```
