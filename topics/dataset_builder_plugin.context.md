# Dataset Builder Plug-in context
<!-- id: topic:dataset_builder_plugin.context -->

Part of: [Dataset Builder Plug-in](dataset_builder_plugin.md)

See also: [N/dataset](../modules/N_dataset.md), [Workbook Builder Plug-in context](workbook_builder_plugin.context.md).

This page covers the one interface function of the Dataset Builder Plug-in and the context object it
receives. The source gives neither of them a symbol ID, so they appear here as plain headings.

## createDataset(context)

The interface function that creates a dataset. NetSuite passes it a context object; your code builds
the dataset with [N/dataset](../modules/N_dataset.md) and stores it on `context.dataset`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |

- `context`: a plain JavaScript object with the properties `dataset`, `description`, `name`, `owner`
  and `role`, described below.

**Returns:** void. The dataset is delivered by assigning `context.dataset`, not by a return value.

**Errors:**

- `NO_DATASET_CREATED`: the function finished without assigning `context.dataset`.
- `WRONG_PARAMETER_TYPE`: something other than a dataset object was assigned to `context.dataset`.
- `READ_ONLY`: the code tried to assign `context.description`, `context.name`, `context.owner` or
  `context.role`.

**Governance:** the plug-in as a whole may use at most 1,000 usage units (see
[Dataset Builder Plug-in](dataset_builder_plugin.md)).

## Context properties

| Property | Type | Access | Meaning |
|---|---|---|---|
| `dataset` | [`dataset.Dataset`](../modules/N_dataset.md) | Write-only, required | The dataset the plug-in produces |
| `description` | string | Read-only | The description entered when the plug-in implementation was added |
| `name` | string | Read-only | The name entered when the plug-in implementation was added |
| `owner` | number | Read-only | Internal ID of the user who added the plug-in implementation |
| `role` | number | Read-only | Internal ID of the role that user had when adding the implementation |

### context.dataset

Required and write-only. It accepts a [`dataset.Dataset`](../modules/N_dataset.md) object, such as
one returned by [`dataset.create()`](../modules/N_dataset.md). Build the dataset with N/dataset
first, then assign it here.

- Leaving it unassigned raises `NO_DATASET_CREATED`.
- Assigning any other kind of object raises `WRONG_PARAMETER_TYPE`.

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

## Example

Written for this page: an item dataset whose name and description are copied from the plug-in
implementation record, so the two always match. The record and field IDs are illustrative; check
them in the [Records Catalog](records_catalog.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType datasetbuilderplugin
 */
define(['N/dataset', 'N/query'], (dataset, query) => {
  const createDataset = (context) => {
    const inactiveFlag = dataset.createColumn({ fieldId: 'isinactive' });

    const built = dataset.create({
      type: 'item',
      name: context.name,
      description: context.description || `Active items (owner ${context.owner})`,
      columns: [
        dataset.createColumn({ fieldId: 'itemid', alias: 'sku', label: 'SKU' }),
        dataset.createColumn({ fieldId: 'displayname', alias: 'item_name', label: 'Item' }),
        dataset.createColumn({ fieldId: 'lastpurchaseprice', alias: 'last_cost', label: 'Last cost' })
      ],
      condition: dataset.createCondition({
        column: inactiveFlag,
        operator: query.Operator.EQUAL,
        values: [false]
      })
    });

    // Assign, never return: an unassigned context.dataset raises NO_DATASET_CREATED.
    context.dataset = built;
  };

  return Object.freeze({ createDataset });
});
```
