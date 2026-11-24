# Workbook Builder Plug-in context
<!-- id: topic:workbook_builder_plugin.context -->

Part of: [Workbook Builder Plug-in](workbook_builder_plugin.md)

See also: [N/workbook](../modules/N_workbook.md), [N/dataset](../modules/N_dataset.md),
[N/query](../modules/N_query.md), [Dataset Builder Plug-in context](dataset_builder_plugin.context.md).

This page covers the one interface function of the Workbook Builder Plug-in and the context object it
receives. The source gives neither of them a symbol ID, so they appear here as plain headings.

## createWorkbook(context)

The interface function that creates a workbook. NetSuite passes it a context object; your code builds
the workbook with [N/workbook](../modules/N_workbook.md) and stores it on `context.workbook`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |

- `context`: a plain JavaScript object with the properties `description`, `name`, `owner`, `role`
  and `workbook`, described below.

**Returns:** void. The workbook is delivered by assigning `context.workbook`, not by a return value.

**Errors:**

- `NO_WORKBOOK_CREATED`: the function finished without assigning `context.workbook`.
- `WRONG_PARAMETER_TYPE`: something other than a workbook object was assigned to `context.workbook`.
- `READ_ONLY`: the code tried to assign `context.description`, `context.name`, `context.owner` or
  `context.role`.
- `CAN_REFERENCE_ONLY_PERSISTED_DATASET`: a visualization refers to a dataset that has not been saved.

**Governance:** the plug-in as a whole may use at most 1,000 usage units (see
[Workbook Builder Plug-in](workbook_builder_plugin.md)).

### Which datasets a workbook may use

A plug-in-built workbook can refer only to datasets that already exist in the account, meaning
datasets saved through the SuiteAnalytics Workbook UI or produced by a
[Dataset Builder Plug-in](dataset_builder_plugin.md). Load them with
[`dataset.load()`](../modules/N_dataset.md). A dataset built on the fly with `dataset.create()` and
never saved triggers `CAN_REFERENCE_ONLY_PERSISTED_DATASET`.

### What the source's sample shows

The source's sample for this function imports [N/workbook](../modules/N_workbook.md),
[N/dataset](../modules/N_dataset.md) and [N/query](../modules/N_query.md) and assigns its result to
`context.workbook`. Along the way it demonstrates:

- a table column filter and a column condition;
- field contexts `RAW` and `HIERARCHY`;
- sorting with locale and nulls-placement options, through
  [`workbook.createSort()`](../modules/N_workbook.md);
- a count-distinct measure, through [`workbook.createDataMeasure()`](../modules/N_workbook.md);
- total-line options, through [`workbook.createSection()`](../modules/N_workbook.md);
- a bar chart with normal stacking.

Two parts of that sample do not hold up against the reference:

> [!SOURCE-ERROR] The sample loads its dataset with an ID that still contains an unfilled template
> placeholder (`%s`), so it cannot run as printed. Pass the real ID of a saved dataset.
<!-- source-error: topic:workbook_builder_plugin.context | - | sample_loads_dataset_with_unfilled_percent_s_id -->

> [!SOURCE-ERROR] The sample passes an `id` option to `workbook.create()` and a `row` option to a sort
> definition, but neither option appears among the documented parameters of those functions. Stick
> to the documented parameters on [N/workbook](../modules/N_workbook.md) and verify anything else.
<!-- source-error: topic:workbook_builder_plugin.context | - | sample_passes_id_to_workbook_create_and_row_to_sort_def -->

## Context properties

| Property | Type | Access | Meaning |
|---|---|---|---|
| `description` | string | Read-only | The description entered when the plug-in implementation was added |
| `name` | string | Read-only | The name entered when the plug-in implementation was added |
| `owner` | number | Read-only | Internal ID of the user who added the plug-in implementation |
| `role` | number | Read-only | Internal ID of the role that user had when adding the implementation |
| `workbook` | [`workbook.Workbook`](../modules/N_workbook.md) | Write-only, required | The workbook the plug-in produces |

### context.description

Read-only string: the description typed in when the plug-in implementation was added. Assigning to
it raises `READ_ONLY`.

### context.name

Read-only string: the name typed in when the plug-in implementation was added. Assigning to it raises
`READ_ONLY`.

### context.owner

Read-only number: the internal ID of the user who added the plug-in implementation. Assigning to it
raises `READ_ONLY`.

### context.role

Read-only number: the internal ID of the role held by the user who added the plug-in implementation.
Assigning to it raises `READ_ONLY`.

### context.workbook

Required and write-only. It accepts a [`workbook.Workbook`](../modules/N_workbook.md) object, such
as one returned by [`workbook.create()`](../modules/N_workbook.md). Build the workbook with
N/workbook first, then assign it here.

- Leaving it unassigned raises `NO_WORKBOOK_CREATED`.
- Assigning any other kind of object raises `WRONG_PARAMETER_TYPE`.

## Example

Written for this page: two table views, each over its own saved dataset, in one workbook. Both
datasets are loaded rather than created, because only persisted datasets may be referenced. Dataset
IDs and column aliases are invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType workbookbuilderplugin
 */
define(['N/workbook', 'N/dataset'], (workbook, dataset) => {
  const plainColumns = (aliases) => aliases.map((alias) =>
    workbook.createTableColumn({ datasetColumnAlias: alias, conditionalFormats: [] }));

  const createWorkbook = (context) => {
    const openPos = dataset.load({ id: 'custdataset_open_purchase_orders' });
    const receipts = dataset.load({ id: 'custdataset_recent_receipts' });

    const poTable = workbook.createTable({
      id: 'open_pos',
      name: 'Open purchase orders',
      dataset: openPos,
      columns: plainColumns(['po_number', 'vendor', 'expected_date'])
    });

    const receiptTable = workbook.createTable({
      id: 'recent_receipts',
      name: 'Receipts this month',
      dataset: receipts,
      columns: [
        ...plainColumns(['receipt_number', 'vendor']),
        workbook.createTableColumn({
          datasetColumnAlias: 'received_on',
          conditionalFormats: [],
          sort: workbook.createSort({ ascending: false, nullsLast: true })
        })
      ]
    });

    // The UI shows the implementation record's name; context.name is read here, never assigned.
    context.workbook = workbook.create({
      name: context.name,
      description: 'Inbound purchasing overview',
      tables: [poTable, receiptTable]
    });
  };

  return Object.freeze({ createWorkbook });
});
```
