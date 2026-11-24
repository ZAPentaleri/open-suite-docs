# N/dataset
<!-- id: N/dataset -->

`N/dataset` creates, loads, lists, describes and saves SuiteAnalytics datasets from a script. A
dataset is the data half of SuiteAnalytics: a root record type plus the columns you want, the
criteria that filter the rows, and the joins that pull in related records. A saved dataset appears in
the SuiteAnalytics Workbook UI, and one dataset can feed any number of workbooks (built with
[N/workbook](N_workbook.md)). Think of it as the workbook-oriented counterpart of
[N/query](N_query.md).

- **Availability:** server scripts only. The module is not supported in client contexts where the
  user is not authenticated.
- **Permission:** the SuiteAnalytics Workbook permission is required.
- **Load it as:** `define(['N/dataset'], (dataset) => { ... })`
- Related: [SuiteScript Analytic APIs](../topics/analytic_apis.md),
  [Workbook API dataset concepts](../topics/analytic_apis.datasets.md),
  [Dataset tutorial](../topics/analytic_apis.tutorial_dataset.md),
  [Dataset Builder Plug-in](../topics/dataset_builder_plugin.md) and its
  [context](../topics/dataset_builder_plugin.context.md),
  [Workbook Builder Plug-in](../topics/workbook_builder_plugin.md), [N/workbook](N_workbook.md),
  [N/datasetLink](N_datasetLink.md), [N/query](N_query.md),
  [Records Catalog](../topics/records_catalog.md).

Points that apply across the module:

- Record types and join sources are Records Catalog internal ids (the analytics data source), not
  `N/record` or `N/search` ids. Look them up in the account's Records Catalog. The
  [dataset concepts topic](../topics/analytic_apis.datasets.md) adds that the root type can be given
  as a [`query.Type`](N_query.md#querytype-1) value or its string equivalent, and that which types you
  can reach depends on enabled features and role permissions.
- Running a dataset returns the same result objects as [N/query](N_query.md)
  ([`query.ResultSet`](N_query.md#resultset-object), [`query.PagedData`](N_query.md#pageddata-object)),
  so N/query result-reading code applies unchanged.
- Give columns an alias: it makes linking a column into a workbook easier, because
  [`Dataset.getExpressionFromColumn`](#datasetgetexpressionfromcolumnoptions) can look a column up by
  alias. An alias may contain letters, numbers and underscores. A column's label is what users see in
  the UI. A field can be used in a workbook only if it is a column of the dataset.
- [`dataset.describe`](#datasetdescribeoptions) reports a dataset's name, description, columns and
  formulas, with labels and types.
- This module has no delete function. The spec says a dataset can be deleted through
  [N/query](N_query.md), but it does not name the function; the only delete function in N/query is
  [`query.delete`](N_query.md#querydeleteoptions), which the source documents for queries saved in
  the Workbook UI. The source does not state whether it takes a dataset id.
- Join targets can be polymorphic: one join field may point at several record types, and `target`
  picks one.
- A [`Dataset`](#dataset-object) can be assigned to `context.dataset` inside a
  [Dataset Builder Plug-in](../topics/dataset_builder_plugin.md). That plug-in type builds a dataset
  at run time (for example from user- or role-specific data), which plain N/dataset datasets cannot
  do because they are static.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`dataset.create(options)`](#datasetcreateoptions) | function | New in-memory dataset on a record type |
| [`dataset.createColumn(options)`](#datasetcreatecolumnoptions) | function | Column from a field or a formula |
| [`dataset.createCondition(options)`](#datasetcreateconditionoptions) | function | Criteria (single test or AND/OR group) |
| [`dataset.createJoin(options)`](#datasetcreatejoinoptions) | function | Join to a related record |
| [`dataset.createTranslation(options)`](#datasetcreatetranslationoptions) | function | Expression from a translation collection |
| [`dataset.describe(options)`](#datasetdescribeoptions) | function | Metadata of a saved dataset |
| [`dataset.describe.promise(options)`](#datasetdescribepromiseoptions) | function | Promise form of `describe` |
| [`dataset.list()`](#datasetlist) | function | All datasets in the account |
| [`dataset.listPaged(options)`](#datasetlistpagedoptions) | function | Dataset metadata in pages |
| [`dataset.load(options)`](#datasetloadoptions) | function | Load a saved dataset |
| [`dataset.load.promise(options)`](#datasetloadpromiseoptions) | function | Promise form of `load` |
| [`Dataset`](#dataset-object) | object | Columns, criteria and joins; run or save |
| [`Column`](#column-object) | object | One dataset field or formula |
| [`Condition`](#condition-object) | object | One criterion or a group of them |
| [`Join`](#join-object) | object | A link to a related record |

## Functions

### dataset.create(options)
<!-- id: N/dataset.create -->

Builds a new dataset in memory, rooted on a record type. Nothing is stored until you call
[`Dataset.save(options)`](#datasetsaveoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.condition` | dataset.Condition | No | |
| `options.type` | string | Yes | |
| `options.columns` | dataset.Column[] | No | |
| `options.description` | string | No | |
| `options.id` | string | No | |
| `options.name` | string | No | |

- `options.condition`: the dataset's criteria. A group condition combines its children with AND or
  OR (see [`createCondition`](#datasetcreateconditionoptions)). Criteria set here filter every
  workbook that uses the dataset.
- `options.type`: the internal id of the root record type, as listed in the Records Catalog. An
  unknown type raises `INVALID_SEARCH_TYPE`.
- `options.id`: when omitted, an id is generated. The concepts topic ties `description` and `id` to
  the Dataset Builder Plug-in.

**Returns:** `dataset.Dataset`, a dataset based on the given record type.

**Errors:**

- `INVALID_SEARCH_TYPE`: `options.type` is not a valid record type id.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### dataset.createColumn(options)
<!-- id: N/dataset.createColumn -->

Defines one column. A column comes either from a record field (`fieldId`, optionally reached through
a `join`) or from a formula together with the type of value the formula returns. The two cannot be
mixed: supplying `formula` together with `fieldId` or `join` is rejected.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.alias` | string | No | |
| `options.fieldId` | string | No | |
| `options.formula` | string | No | |
| `options.id` | number | No | |
| `options.type` | string | No | |
| `options.label` | string | No | |
| `options.join` | dataset.Join | No | |

- `options.alias`: a name you can later pass to
  [`Dataset.getExpressionFromColumn`](#datasetgetexpressionfromcolumnoptions) to find this column.
- `options.fieldId`: conditionally required; needed unless you give `formula` and `type`. Cannot be
  combined with `formula`.
- `options.formula`: conditionally required; needed unless you give `fieldId`. Field ids inside the
  formula go in curly braces, for instance a formula that just returns `{email}`, or one that
  subtracts the tax from the total.
- `options.id`: a numeric id you can later pass to `getExpressionFromColumn`.
- `options.type`: conditionally required; needed unless you give `fieldId`. The data type the formula
  returns; `INTEGER` and `STRING` are the source's examples. The source does not list the full set of
  accepted formula types. An invalid value raises `INVALID_FORMULA_TYPE`.
- `options.label`: the text shown for this column in the UI.
- `options.join`: the join that leads to the record holding `fieldId`.

**Returns:** `dataset.Column`

**Errors:**

- `INVALID_FORMULA_TYPE`: `options.type` is not a valid formula type.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: `options.formula` was combined with `options.fieldId` or
  `options.join`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### dataset.createCondition(options)
<!-- id: N/dataset.createCondition -->

Creates criteria for a dataset. There are two shapes:

- A **single test**: a `column`, an `operator` from [`query.Operator`](N_query.md#queryoperator) and
  the `values` to compare against. Pass `values` only when the operator needs them (`EMPTY`, for
  instance, takes none). The field must be a column of the dataset.
- A **group**: `children` (other conditions) combined by the operator `AND` or `OR`. For the top-level
  grouping condition the column is null, the operator is `AND` or `OR`, and `values` is empty. A
  dataset holds a single condition, so several tests are combined into one group.

`column` and `children` are mutually exclusive.

> [!SOURCE-ERROR] The source says a group combines its children with AND, yet it also accepts `OR` as
> a group operator. Treat the operator you pass as deciding the combination, and test `OR` groups
> before relying on them.
<!-- source-error: N/dataset.createCondition | - | children_said_combined_with_and_operator_allows_and_or -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.children` | dataset.Condition[] | No | |
| `options.column` | dataset.Column | No | |
| `options.operator` | string | Yes | |
| `options.values` | Array<null \| Object \| number \| string \| boolean \| Date> | No | |

- `options.children`: used when the operator is `AND` or `OR`.
- `options.column`: conditionally required; needed unless `children` is given.
- `options.operator`: a value from [`query.Operator`](N_query.md#queryoperator); when `children` is
  given, `AND` and `OR` are also accepted. An invalid operator raises `INVALID_OPERATOR`.

> [!SOURCE-ERROR] The source's example passes `query.Operator.OR`, but `query.Operator` has no `OR`
> value. For a group, pass the plain string `'OR'` (or `'AND'`) as this option describes.
<!-- source-error: N/dataset.createCondition | options.operator | example_uses_query_Operator_OR_absent_from_enum -->

- `options.values`: the comparison values. The [dataset tutorial](../topics/analytic_apis.tutorial_dataset.md)
  notes that date values follow the user's date format preference.

**Returns:** `dataset.Condition`

**Errors:**

- `INVALID_OPERATOR`: `options.operator` is not a valid operator.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `options.column` and `options.children` were supplied.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### dataset.createJoin(options)
<!-- id: N/dataset.createJoin -->

Creates a join that brings a related record's fields into the dataset. Three kinds are supported:
multi-level joins (a join from a joined record, via `join`), inverse joins (from another record type
that points back at this one, via `source`), and polymorphic joins (a field that can point at several
record types, narrowed with `target`). `source` and `target` cannot be used together. With
`fieldId` alone the direction is inferred; adding `source` or `target` makes it explicit. Multi-level
joins also let you reach types that are not directly related.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.join` | dataset.Join | No | |
| `options.source` | string | No | |
| `options.target` | string | No | |

- `options.fieldId`: the field the join goes through, for example `entity`.
- `options.join`: a parent join, for a multi-level join. Pass it only when a child join is being
  built from an existing join.
- `options.source`: for an inverse join, the internal id of the record type the join starts from;
  allowed values are the internal ids listed in the Records Catalog.
- `options.target`: for a polymorphic join, the record type to join to. Use it when the joined type
  has inherited types, such as the entity records behind `entity`.

**Returns:** `dataset.Join`

**Errors:**

- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `options.source` and `options.target` were supplied.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### dataset.createTranslation(options)
<!-- id: N/dataset.createTranslation -->

Returns a workbook expression whose text comes from a translation collection, identified by the
collection and a key inside it. It is useful wherever an expression is accepted instead of plain
text, for example the `name` or `description` of [`Dataset.save`](#datasetsaveoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.collection` | string | Yes | |
| `options.key` | string | Yes | |

**Returns:** `workbook.Expression` (see [Expression](N_workbook.md#expression-object) in N/workbook).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### dataset.describe(options)
<!-- id: N/dataset.describe -->

Reads the definition of a saved dataset without loading it: its name and description, and its
columns and formulas together with their labels and types.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `Object[]`, the metadata described above. The source does not state the property names
or layout of these objects, so inspect the output before depending on it.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.2

**Promise variant:** [`dataset.describe.promise(options)`](#datasetdescribepromiseoptions).

### dataset.describe.promise(options)
<!-- id: N/dataset.describe.promise -->

Promise form of [`dataset.describe(options)`](#datasetdescribeoptions): same parameters and errors,
but it returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `Promise`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.2

### dataset.list()
<!-- id: N/dataset.list -->

Lists the datasets in the account. Each entry carries the dataset's `id`, `name`, `record` (its root
record type) and `description`; the source states nothing further about the entries.

**Returns:** `Object[]`, one entry per dataset.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

### dataset.listPaged(options)
<!-- id: N/dataset.listPaged -->

Like [`dataset.list()`](#datasetlist), but returns dataset metadata in pages, optionally limited to
one owner category.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.category` | string | No | |
| `options.pageSize` | number | Yes | |

- `options.category`: must be an owner category value; anything else raises `INVALID_OWNER_CATEGORY`.
  The source does not list the owner category values.
- `options.pageSize`: the source states no range for this function.

**Returns:** `PagedInfoData`, paged dataset metadata. The source does not describe this type's
members.

**Errors:**

- `INVALID_OWNER_CATEGORY`: `options.category` is not a recognised owner category.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.2

### dataset.load(options)
<!-- id: N/dataset.load -->

Loads an existing, saved dataset so you can run it, inspect it, or change and re-save it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `dataset.Dataset`, the existing dataset.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

**Promise variant:** [`dataset.load.promise(options)`](#datasetloadpromiseoptions).

### dataset.load.promise(options)
<!-- id: N/dataset.load.promise -->

Promise form of [`dataset.load(options)`](#datasetloadoptions): same parameters and errors, returns a
Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `Promise`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

## Objects

### Dataset (object)
<!-- id: N/dataset.Dataset -->

A dataset: its columns, its criteria and its joins. You get one from
[`dataset.create(options)`](#datasetcreateoptions) or [`dataset.load(options)`](#datasetloadoptions).
Inside a [Dataset Builder Plug-in](../topics/dataset_builder_plugin.md), the dataset you build is
assigned to `context.dataset` (see the [plug-in context](../topics/dataset_builder_plugin.context.md)).
Save a dataset before you look at it in the UI. Server scripts, since 2020.2.

#### Dataset.getExpressionFromColumn(options)
<!-- id: N/dataset.Dataset#getExpressionFromColumn -->

Returns a workbook expression that refers to one of this dataset's columns; this is how a workbook
built with [N/workbook](N_workbook.md), or a link built with [N/datasetLink](N_datasetLink.md), points
at dataset data. Identify the column by its numeric `columnId` or by its `alias`, never both.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.columnId` | number | No | |
| `options.alias` | string | No | |

- `options.columnId`: conditionally required; needed unless `alias` is given.
- `options.alias`: conditionally required; needed unless `columnId` is given.

**Returns:** `workbook.Expression`

**Errors:**

- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `columnId` and `alias` were supplied.
- `NEITHER_ARGUMENT_DEFINED`: neither `columnId` nor `alias` was supplied.

> [!SOURCE-ERROR] The source's error descriptions spell the option `columnID` [sic], while the
> parameter itself is `columnId`. Pass `columnId`, as in the parameter table.
<!-- source-error: N/dataset.Dataset#getExpressionFromColumn | - | errors_spell_columnID_vs_param_columnId -->

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

#### Dataset.run()
<!-- id: N/dataset.Dataset#run -->

Runs the dataset and returns all results at once, as an N/query result set. Running a dataset in the
SuiteScript debugger is the technique the plug-in topic suggests for checking a Dataset Builder
Plug-in's logic.

**Returns:** `query.ResultSet` (see [ResultSet](N_query.md#resultset-object) in N/query).

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

**Promise variant:** [`Dataset.run.promise()`](#datasetrunpromise).

#### Dataset.run.promise()
<!-- id: N/dataset.Dataset#run.promise -->

Promise form of [`Dataset.run()`](#datasetrun): same (empty) parameter list and errors, returns a
Promise.

**Returns:** `Promise`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

#### Dataset.runPaged(options)
<!-- id: N/dataset.Dataset#runPaged -->

Runs the dataset and returns the results as pages that you fetch one at a time, which suits large
result sets.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.pageSize` | number | Yes | `50` |

- `options.pageSize`: between 5 and 1000 results per page; 50 when not given. The last page can hold
  fewer than 5 results.

> [!SOURCE-ERROR] The source marks `pageSize` as required while also giving it a default of 50; both
> cannot hold. Pass a page size explicitly so the call works whichever is right.
<!-- source-error: N/dataset.Dataset#runPaged | options.pageSize | marked_required_but_has_default -->

**Returns:** `query.PagedData` (see [PagedData](N_query.md#pageddata-object) in N/query).

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

#### Dataset.save(options)
<!-- id: N/dataset.Dataset#save -->

Stores the dataset in the account, after which it is visible in the SuiteAnalytics Workbook UI.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string \| workbook.Expression | Yes | |
| `options.id` | string | No | |
| `options.description` | string \| workbook.Expression | No | |

- `options.name`: plain text or a workbook expression (for example one from
  [`createTranslation`](#datasetcreatetranslationoptions)).
- `options.id`: must start with `custdataset`, or the call fails with `INVALID_ID_PREFIX`. Leave it
  out and an id is generated.
- `options.description`: plain text or a workbook expression.

**Returns:** `Object` that contains only the saved dataset's id.

**Errors:**

- `INVALID_ID_PREFIX`: `options.id` does not begin with `custdataset`.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

#### Dataset.columns
<!-- id: N/dataset.Dataset#columns -->

`dataset.Column[]`. The dataset's columns (see [`Column`](#column-object)). Since 2020.2.

#### Dataset.condition
<!-- id: N/dataset.Dataset#condition -->

`dataset.Condition`. The dataset's criteria (see [`Condition`](#condition-object)). Since 2020.2.

#### Dataset.description
<!-- id: N/dataset.Dataset#description -->

`string`. The dataset's description. Since 2020.2.

#### Dataset.id
<!-- id: N/dataset.Dataset#id -->

`string`. The dataset's id. Since 2020.2.

#### Dataset.name
<!-- id: N/dataset.Dataset#name -->

`string`. The dataset's name. Since 2020.2.

#### Dataset.type
<!-- id: N/dataset.Dataset#type -->

`string`. The internal id of the root record type, as listed in the Records Catalog. Since 2020.2.

### Column (object)
<!-- id: N/dataset.Column -->

One column of a dataset, the scripted counterpart of a field in a SuiteAnalytics dataset. Created
with [`dataset.createColumn(options)`](#datasetcreatecolumnoptions). All properties are read-only.
Server scripts, since 2020.2.

#### Column.fieldId
<!-- id: N/dataset.Column#fieldId -->

`string`, read-only. The id of the record field the column shows. Since 2020.2.

#### Column.id
<!-- id: N/dataset.Column#id -->

`number`, read-only. The column's id. Since 2020.2.

#### Column.formula
<!-- id: N/dataset.Column#formula -->

`string`, read-only. The SuiteAnalytics formula behind the column, for formula columns. Since 2020.2.

#### Column.type
<!-- id: N/dataset.Column#type -->

`string`, read-only. The data type the formula returns. Since 2020.2.

#### Column.label
<!-- id: N/dataset.Column#label -->

`string`, read-only. The column's label. Since 2020.2.

#### Column.alias
<!-- id: N/dataset.Column#alias -->

`string`, read-only. The column's alias. Since 2020.2.

#### Column.join
<!-- id: N/dataset.Column#join -->

`dataset.Join`, read-only. The join through which the column's field is reached (see
[`Join`](#join-object)). Since 2020.2.

### Condition (object)
<!-- id: N/dataset.Condition -->

Dataset criteria, the scripted counterpart of a criteria filter in SuiteAnalytics. Created with
[`dataset.createCondition(options)`](#datasetcreateconditionoptions). Server scripts, since 2020.2.

#### Condition.caseSensitive
<!-- id: N/dataset.Condition#caseSensitive -->

`boolean`. When `true`, the condition compares text case-sensitively. Since 2020.2.

#### Condition.children
<!-- id: N/dataset.Condition#children -->

`dataset.Condition[]`, read-only. The sub-conditions that this condition combines with AND or OR.
Since 2020.2.

#### Condition.column
<!-- id: N/dataset.Condition#column -->

`dataset.Column`, read-only. The column the condition tests (see [`Column`](#column-object)). Since
2020.2.

#### Condition.operator
<!-- id: N/dataset.Condition#operator -->

`string`, read-only. The condition's operator. Since 2020.2.

#### Condition.values
<!-- id: N/dataset.Condition#values -->

`string[] | number[] | boolean[] | Date[] | Object[]`, read-only. The values the column is compared
with. Since 2020.2.

### Join (object)
<!-- id: N/dataset.Join -->

A joined record inside a dataset. Created with [`dataset.createJoin(options)`](#datasetcreatejoinoptions).
All properties are read-only. Server scripts, since 2020.2.

#### Join.fieldId
<!-- id: N/dataset.Join#fieldId -->

`string`, read-only. The field the join goes through. Since 2020.2.

#### Join.join
<!-- id: N/dataset.Join#join -->

`dataset.Join`, read-only. The child join, present when this join is part of a multi-level join.
Since 2020.2.

#### Join.source
<!-- id: N/dataset.Join#source -->

`string`, read-only. For inverse joins, the internal id of the source record type, as listed in the
Records Catalog. Since 2020.2.

#### Join.target
<!-- id: N/dataset.Join#target -->

`string`, read-only. For polymorphic joins, the record type the join is narrowed to. Since 2020.2.

## Examples

The record type and field ids below are Records Catalog ids chosen for illustration; confirm them in
your account.

### Build and save a dataset with a polymorphic join

A scheduled script that saves a dataset of vendor bills. The `entity` join is narrowed to vendors
with `target`, a type column feeds the condition, and a formula column carries the memo text.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/dataset', 'N/query', 'N/log'], (dataset, query, log) => {
  const execute = () => {
    const vendorJoin = dataset.createJoin({ fieldId: 'entity', target: 'vendor' });

    const typeColumn = dataset.createColumn({ fieldId: 'type', alias: 'tran_type', label: 'Type' });
    const columns = [
      typeColumn,
      dataset.createColumn({ fieldId: 'tranid', alias: 'doc_number', label: 'Bill' }),
      dataset.createColumn({ fieldId: 'trandate', alias: 'doc_date', label: 'Date' }),
      dataset.createColumn({
        fieldId: 'companyname',
        join: vendorJoin,
        alias: 'vendor_name',
        label: 'Vendor'
      }),
      dataset.createColumn({ formula: '{memo}', type: 'STRING', alias: 'note', label: 'Note' })
    ];

    const billsOnly = dataset.createCondition({
      column: typeColumn,
      operator: query.Operator.ANY_OF,
      values: ['VendBill']
    });

    const bills = dataset.create({
      type: query.Type.TRANSACTION,
      columns,
      condition: billsOnly
    });

    const saved = bills.save({
      name: 'Vendor bills with memo',
      id: 'custdataset_vendor_bill_memo',
      description: 'Maintained by the AP reporting job'
    });
    log.audit({ title: 'Dataset saved', details: JSON.stringify(saved) });
  };

  return { execute };
});
```

### A Dataset Builder Plug-in

A [Dataset Builder Plug-in](../topics/dataset_builder_plugin.md) implementation builds its dataset
when a workbook asks for it, and hands it over through `context.dataset`. Here the customers shown are
the ones whose sales rep is the implementation's owner (`context.owner`, read-only). The operator is
passed as its mapped string, which works without loading N/query.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType datasetbuilderplugin
 */
define(['N/dataset'], (dataset) => {
  const createDataset = (context) => {
    const repColumn = dataset.createColumn({ fieldId: 'salesrep', alias: 'rep', label: 'Sales Rep' });

    const myAccounts = dataset.createCondition({
      column: repColumn,
      operator: 'ANY_OF',
      values: [context.owner]
    });

    context.dataset = dataset.create({
      type: 'customer',
      columns: [
        dataset.createColumn({ fieldId: 'entityid', alias: 'customer_id', label: 'Customer' }),
        dataset.createColumn({ fieldId: 'email', alias: 'email', label: 'Email' }),
        repColumn
      ],
      condition: myAccounts
    });
  };

  return Object.freeze({ createDataset });
});
```

### Read a saved dataset in pages

A Map/Reduce `getInputData` stage that pages through a saved dataset and hands the rows on as plain
objects:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/dataset'], (dataset) => {
  const getInputData = () => {
    const bills = dataset.load({ id: 'custdataset_vendor_bill_memo' });
    const paged = bills.runPaged({ pageSize: 500 });
    let rows = [];
    paged.pageRanges.forEach((range) => {
      const page = paged.fetch({ index: range.index });
      rows = rows.concat(page.data.asMappedResults());
    });
    return rows;
  };

  const map = (context) => {
    const row = JSON.parse(context.value);
    context.write({ key: String(row.vendor_name), value: String(row.doc_number) });
  };

  return { getInputData, map };
});
```

### List and describe

Finding datasets rooted on a given record type with `list`, then describing each match:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/dataset'], (dataset) => {
  const onRequest = (context) => {
    const wanted = context.request.parameters.recordType || 'transaction';
    const matches = dataset.list().filter((entry) => entry.record === wanted);
    const report = matches.map((entry) => ({
      id: entry.id,
      name: entry.name,
      definition: dataset.describe({ id: entry.id })
    }));
    context.response.write({ output: JSON.stringify(report) });
  };

  return { onRequest };
});
```
