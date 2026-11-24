# Workbook API dataset concepts
<!-- id: topic:analytic_apis.datasets -->

Part of: [SuiteScript Analytic APIs](analytic_apis.md)

See also: [N/dataset](../modules/N_dataset.md), [N/datasetLink](../modules/N_datasetLink.md),
[N/query](../modules/N_query.md), [Workbook API workbook concepts](analytic_apis.workbooks.md),
[Dataset tutorial](analytic_apis.tutorial_dataset.md).

The Workbook API has two halves. Datasets decide *what data* you get: columns, conditions, joins and
links between datasets. Workbooks decide *how it is shown*: table views and pivots (see
[workbook concepts](analytic_apis.workbooks.md)). This page covers the dataset half.

## Creating a dataset

Build one with [`dataset.create()`](../modules/N_dataset.md).

- The only required value is the root record `type`. Take it from the
  [`query.Type`](../modules/N_query.md) enum, or pass the equivalent string.
- If you omit the dataset ID, one is generated for you.
- A dataset meant for the dataset builder plugin needs a description and an ID.
- Which record types and fields you can reach depends on the account's features and the role's
  permissions.

## Columns

Create columns with [`dataset.createColumn()`](../modules/N_dataset.md). Give either a `fieldId`, or a
`formula` together with its `type`.

- Column aliases may contain only letters, digits and underscores.
- A workbook can only show fields that are columns of its dataset, so add every field you plan to
  display.

## Conditions

Create conditions with [`dataset.createCondition()`](../modules/N_dataset.md), in one of two shapes:

- a `column`, an `operator` and `values`; or
- `children` (other conditions) plus an `operator` that joins them, either AND or OR.

Pass `values` only when the operator needs them; operators come from
[`query.Operator`](../modules/N_query.md). An "is empty" test, for example, takes no values.

A dataset's condition filters the data for every workbook built on that dataset.

## Joins

Create joins with [`dataset.createJoin()`](../modules/N_dataset.md). The combination of options
decides what kind of join you get:

| Options given | Result |
|---|---|
| `fieldId` only | The join direction and target are inferred |
| `fieldId` and `join` | A multilevel join (a join hanging off another join) |
| `fieldId` and `source` or `target` | The direction is stated explicitly |

- Use `target` when the field points at a type that has inherited types; entity records are the
  usual case.
- Multilevel joins can reach record types that are not directly related to the root.
- Pass `join` only when there is a child join to attach.

## Linking datasets

When two record types cannot be joined, link two datasets instead with
[`datasetLink.create()`](../modules/N_datasetLink.md).

- The datasets need a field in common, and the linking columns must have the same data type.
- Linked datasets can be used only in pivots.

## Example

A scheduled script, written for this page, that saves a dataset of active customers with their sales
rep's email. Field IDs are illustrative; confirm analytics names in the
[Records Catalog](records_catalog.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/dataset', 'N/query', 'N/log'], (dataset, query, log) => {
  const execute = () => {
    const repJoin = dataset.createJoin({ fieldId: 'salesrep' });

    const nameCol = dataset.createColumn({ fieldId: 'entityid', alias: 'cust_name' });
    const repMail = dataset.createColumn({ fieldId: 'email', join: repJoin, alias: 'rep_email' });
    const inactive = dataset.createColumn({ fieldId: 'isinactive', alias: 'is_inactive' });

    const activeOnly = dataset.createCondition({
      column: inactive,
      operator: query.Operator.IS,
      values: [false]
    });

    const ds = dataset.create({
      type: query.Type.CUSTOMER,
      columns: [nameCol, repMail, inactive],
      condition: activeOnly
    });

    ds.save({ name: 'Active customers by rep', id: 'custdataset_active_by_rep' });
    log.audit({ title: 'Dataset saved', details: ds.id });
  };

  return { execute };
});
```
