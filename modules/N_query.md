# N/query
<!-- id: N/query -->

`N/query` lets a script build, run, load and delete SuiteAnalytics Workbook queries, and run SuiteQL
statements. You describe a query as a tree: a root component of some query type, optional joins to
other components, result columns, a condition (which can nest `AND` / `OR` / `NOT`), and sorts. You
then run it all at once (up to 5,000 rows) or page through it. A query definition can also be turned
into a SuiteQL statement, and raw SuiteQL can be run directly.

- **Permission:** the running user needs the SuiteAnalytics Workbook permission.
- **Availability:** client and server scripts, but not in client-side contexts where no user is
  authenticated.
- **Load it as:** `define(['N/query'], (query) => { ... })`
- **Data source:** queries read the analytics data source, which is not the one `N/search` uses, so
  field IDs differ. Take query field IDs from the [Records Catalog](../topics/records_catalog.md),
  not from the Records Browser (the pages under `out/gen/records/` list N/search IDs).
- Related: [Query definitions](../topics/query_definition.md), [SuiteQL](../topics/suiteql.md),
  [SuiteQL in the query definition](../topics/query_definition.suiteql.md),
  [SuiteScript Analytic APIs](../topics/analytic_apis.md), [N/search](N_search.md),
  [N/dataset](N_dataset.md), [N/workbook](N_workbook.md).

What the module covers, in short: multi-level joins, conditions combined with and/or/not, formula
conditions and conditions on relative dates and periods, sorting on several columns, loading and
deleting queries that were saved in the Workbook UI, listing a workbook's table views
([`query.listTables`](#querylisttablesoptions)), paged results, promise variants, and converting a
query to SuiteQL and running it. A query loaded from the Workbook UI can be modified, joined onto and
run just like one created in the script.

Things it does **not** do:

- It cannot save a query. Save queries in the SuiteAnalytics Workbook UI; scripts can then load,
  modify, join onto, run or delete them.
- A query deleted with [`query.delete`](#querydeleteoptions) cannot be used again.

Other module-wide facts:

- A column takes either a field ID or a formula, never both.
- `Query.id` and `Query.name` stay `null` unless the query came from [`query.load`](#queryloadoptions).
- `Query.join` and `Component.join` behave the same as the corresponding `autoJoin`.

> [!SOURCE-ERROR] One source listing gives `Query.run.promise()` a `ResultSet` return type, although a
> promise variant hands back a Promise (which resolves to a `ResultSet`). Rely on the Promise return
> documented under [`Query.run.promise`](#queryrunpromiseoptions).
<!-- source-error: N/query | - | query_run_promise_listed_returning_resultset -->

> [!SOURCE-ERROR] One source listing types `Query.sort` as an array of columns. The property holds
> `query.Sort` objects (see [`Query.sort`](#querysort)); build them with `createSort`.
<!-- source-error: N/query | - | query_sort_typed_as_column_array -->

## Members

| Member | Kind | Summary |
|---|---|---|
| [`query.create(options)`](#querycreateoptions) | function | Start a new query definition of a given type |
| [`query.load(options)`](#queryloadoptions) | function | Load a query saved in the Workbook UI |
| [`query.load.promise(options)`](#queryloadpromiseoptions) | function | Promise form of `load` |
| [`query.delete(options)`](#querydeleteoptions) | function | Delete a saved query |
| [`query.listTables(options)`](#querylisttablesoptions) | function | List the table views of a workbook |
| [`query.runSuiteQL(options)`](#queryrunsuiteqloptions) | function | Run a SuiteQL statement (up to 5,000 rows) |
| [`query.runSuiteQL.promise(options)`](#queryrunsuiteqlpromiseoptions) | function | Promise form of `runSuiteQL` |
| [`query.runSuiteQLPaged(options)`](#queryrunsuiteqlpagedoptions) | function | Run a SuiteQL statement page by page |
| [`query.runSuiteQLPaged.promise(options)`](#queryrunsuiteqlpagedpromiseoptions) | function | Promise form of `runSuiteQLPaged` |
| [`query.createRelativeDate(options)`](#querycreaterelativedateoptions) | function | Build a date relative to now for conditions |
| [`query.createPeriod(options)`](#querycreateperiodoptions) | function | Build an accounting period reference for conditions |
| [`Query`](#query-object) | object | A query definition |
| [`Component`](#component-object) | object | One query type in the join tree |
| [`Column`](#column-object) | object | A result column |
| [`Condition`](#condition-object) | object | A filter, possibly with child conditions |
| [`Sort`](#sort-object) | object | A sort on a column |
| [`ResultSet`](#resultset-object) | object | Rows returned by a run |
| [`Result`](#result-object) | object | One row |
| [`PagedData`](#pageddata-object) | object | Handle to paged results |
| [`Page`](#page-object) | object | One page of results |
| [`PageRange`](#pagerange-object) | object | Index and size of one page |
| [`SuiteQL`](#suiteql-object) | object | A SuiteQL statement with parameters |
| [`RelativeDate`](#relativedate-object) | object | A moment or range relative to today |
| [`Period`](#period-object) | object | A period reference for conditions |
| [`query.Type`](#querytype-1) | enum | Query types for `create` |
| [`query.Operator`](#queryoperator) | enum | Condition operators |
| [`query.Aggregate`](#queryaggregate) | enum | Aggregate functions |
| [`query.ReturnType`](#queryreturntype) | enum | Formula return types |
| [`query.FieldContext`](#queryfieldcontext) | enum | How column values are presented |
| [`query.SortLocale`](#querysortlocale) | enum | Locale collations for sorts |
| [`query.DateId`](#querydateid) | enum | Units for `createRelativeDate` |
| [`query.RelativeDateRange`](#queryrelativedaterange) | enum | Ready-made relative date ranges |
| [`query.PeriodCode`](#queryperiodcode) | enum | Period codes for `createPeriod` |
| [`query.PeriodAdjustment`](#queryperiodadjustment) | enum | Period adjustment for `createPeriod` |
| [`query.PeriodType`](#queryperiodtype) | enum | Period start/end for `createPeriod` |

## Functions

### query.create(options)
<!-- id: N/query.create -->

Starts a new query definition. The type you pass becomes the root component's type. You can pass
columns, a condition and sorts here, or add them afterwards on the returned [`Query`](#query-object).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.columns` | Object[] | No | |
| `options.condition` | Object | No | |
| `options.sort` | Object[] | No | |

- `options.type`: a [`query.Type`](#querytype-1) value. It becomes [`Query.type`](#querytype).
- `options.columns`: each object is passed to `createColumn` for you. Although optional here, every
  query definition needs at least one column.
- `options.condition`: the object is passed to `createCondition` for you.
- `options.sort`: for each object, a column is created with `createColumn` and then a sort with
  `createSort`.

**Returns:** `query.Query`, the new definition.

**Errors:**

- `INVALID_RCRD_TYPE`: the type argument is not acceptable.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

- When validation happens depends on the type: standard types are checked when `create` is called,
  custom record types only when the query runs.
- The supported types are the same ones SuiteAnalytics Workbook supports.
- Queries cannot be saved from a script (use the Workbook UI to save), but saved ones can be loaded
  and deleted.

### query.load(options)
<!-- id: N/query.load -->

Loads a query that someone built and saved in the SuiteAnalytics Workbook UI. Once loaded you can
change its definition, add joins and run it exactly like a query you created yourself. The IDs are
found in the Workbook UI.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: either a workbook ID or a dataset ID.

**Returns:** `query.Query`

**Errors:**

- `UNABLE_TO_LOAD_QUERY`: no query with that ID exists, or the user lacks permission to it.
- `WORKBOOK_MORE_TABLEVIEWS_ARE_ASSIGNED`: the workbook has several table views, so it is unclear
  which one to load. Call [`query.listTables`](#querylisttablesoptions) to get the ID of the table
  view you want.
- `WORKBOOK_NO_TABLEVIEW_IS_ASSIGNED`: the workbook has no table view.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2018.2

- If the loaded query has no sort, page through it with `runPaged` (which applies a default sort) or
  add your own sort before running it.

### query.load.promise(options)
<!-- id: N/query.load.promise -->

Promise form of [`query.load`](#queryloadoptions): it returns a Promise instead of the query, and is
declared to take the same parameters and raise the same errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `Promise`

**Errors:**

- `UNABLE_TO_LOAD_QUERY`: missing query or no permission.
- `WORKBOOK_MORE_TABLEVIEWS_ARE_ASSIGNED`: the workbook has more than one table view.
- `WORKBOOK_NO_TABLEVIEW_IS_ASSIGNED`: the workbook has no table view.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2018.2

### query.delete(options)
<!-- id: N/query.delete -->

Deletes a query that was saved in the Workbook UI. After deletion the query cannot be loaded or used
again.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: the script ID of the query.

**Returns:** `void`

**Errors:**

- `UNABLE_TO_DELETE_QUERY`: the query does not exist, or the user may not delete it.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2018.2

### query.listTables(options)
<!-- id: N/query.listTables -->

Lists the table views in a workbook. Use it when [`query.load`](#queryloadoptions) complains that a
workbook has more than one table view, to find the ID of the one you want.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.workbookId` | string | Yes | |

- `options.workbookId`: the ID of the workbook.

**Returns:** `Object` describing the workbook's table views: for each one, its name and its
`scriptId`. The source does not state the layout of the object beyond that.

**Errors:**

- `MISSING_REQD_ARGUMENT`: a required argument is missing.
- `SCRIPT_ID_OF_WORKBOOK_IS_REQUIRED`: no workbook ID was supplied. It is also raised when the ID
  belongs to some other analytical record rather than a workbook.
- `SSS_INVALID_SCRIPT_ID_1`: the ID value is not valid.
- `WRONG_PARAMETER_TYPE`: an argument has the wrong type.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.1

### query.runSuiteQL(options)
<!-- id: N/query.runSuiteQL -->

Runs a SuiteQL statement and returns all of its rows in one go, as a result set. The input can be a
plain SuiteQL string, a [`query.SuiteQL`](#suiteql-object) object, or an object holding the statement
and its parameters. See the [SuiteQL topic](../topics/suiteql.md) for the dialect and its limits.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.query` | string | Yes | |
| `options.params` | Array<string \| number \| boolean> | No | |
| `options.metaDataProvider` | string | No | `SUITE_QL` |
| `options.customScriptId` | string | No | |

- `options.params`: the values for the statement's `?` placeholders, each a string, number or
  boolean.

> [!SOURCE-ERROR] The source's type cell and its error row for `params` name only two of the types,
> while its own syntax sample passes a boolean. The spec keeps all three; check boolean binding in
> your account before depending on it.
<!-- source-error: N/query.runSuiteQL | options.params | type_cell_and_error_row_omit_third_type -->
<!-- source-error: N/query.runSuiteQL | options.params | source_type_list_incomplete -->

- `options.metaDataProvider`: `SUITE_QL` or `STATIC`. With `SUITE_QL` the query fails when the user
  lacks permission to a field or record it touches; with `STATIC` it still runs and simply returns no
  data for the fields the user may not see.
- `options.customScriptId`: must be unique. It identifies the query when performance fixes are
  applied; if several queries share it, a fix aimed at one affects them all.

**Returns:** `query.ResultSet` holding the rows.

**Errors:**

- `MISSING_REQD_ARGUMENT`: a required argument (such as the statement) is missing.
- `SSS_INVALID_TYPE_ARG`: an entry in `options.params` has an unsupported type.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.1

- At most 5,000 rows come back. For more, use [`query.runSuiteQLPaged`](#queryrunsuiteqlpagedoptions).

> [!SOURCE-ERROR] The source gives this call two ceilings: 5,000 rows, and also 100,000 rows across
> all pages when SuiteAnalytics Connect is disabled (none when it is enabled). `SuiteQL.run`, said to
> be equivalent, carries the 100,000 figure. The larger ceiling reads like the paged limit; plan for
> 5,000 rows here and page for anything more.
<!-- source-error: N/query.runSuiteQL | - | max_results_5000_vs_100000_or_unlimited_across_pages -->

### query.runSuiteQL.promise(options)
<!-- id: N/query.runSuiteQL.promise -->

Promise form of [`query.runSuiteQL`](#queryrunsuiteqloptions), declared to take the same parameters
and raise the same errors. It accepts the same three input forms (string, `query.SuiteQL` object, or
object with statement and parameters) and is also limited to 5,000 rows; page with
`runSuiteQLPaged` for more.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.query` | string | Yes | |
| `options.params` | Array<string \| number \| boolean> | No | |
| `options.customScriptId` | string | No | |

> [!SOURCE-ERROR] As for `runSuiteQL`, the source's type cell and error row list two types for
> `params` while its syntax sample uses a boolean. Verify boolean parameters before relying on them.
<!-- source-error: N/query.runSuiteQL.promise | options.params | type_cell_and_error_row_omit_third_type -->
<!-- source-error: N/query.runSuiteQL.promise | options.params | source_type_list_incomplete -->

- `options.metaDataProvider`: the spec's parameter list for this variant does not include it, even
  though it says the parameters match `runSuiteQL`. The source does not state whether the promise
  form accepts it.

**Returns:** `Promise`

**Errors:**

- `MISSING_REQD_ARGUMENT`: a required argument is missing.
- `SSS_INVALID_TYPE_ARG`: an entry in `options.params` has an unsupported type.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.1

### query.runSuiteQLPaged(options)
<!-- id: N/query.runSuiteQLPaged -->

Runs a SuiteQL statement and returns a [`PagedData`](#pageddata-object) handle, so you can read the
rows a page at a time. Like `runSuiteQL`, it accepts a SuiteQL string, a `query.SuiteQL` object, or
an object holding statement and parameters.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.query` | string | Yes | |
| `options.pageSize` | number | No | `50` |
| `options.params` | Array<string \| number \| boolean> | No | |
| `options.metaDataProvider` | string | No | `SUITE_QL` |
| `options.customScriptId` | string | No | |

- `options.pageSize`: rows per page, from 5 to 1,000.
- `options.params`: values for the `?` placeholders.

> [!SOURCE-ERROR] The source's type cell and error row for `params` omit one of the three types that
> its syntax sample uses (boolean). The spec keeps string, number and boolean; verify booleans in your
> account.
<!-- source-error: N/query.runSuiteQLPaged | options.params | type_cell_and_error_row_omit_third_type -->
<!-- source-error: N/query.runSuiteQLPaged | options.params | source_type_list_incomplete -->

- `options.metaDataProvider`: `SUITE_QL` fails when permissions are missing; `STATIC` skips the data
  the user is not allowed to see.

**Returns:** `query.PagedData`

**Errors:**

- `MISSING_REQD_ARGUMENT`: a required parameter is missing.
- `SSS_INVALID_TYPE_ARG`: `options.params` contains something other than strings, numbers or
  booleans.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.1

- **Sort order matters.** The statement must order rows by something unique and unambiguous (for
  example the `id`); otherwise rows can repeat or go missing between pages.
- At most 1,000 pages. In total at most 100,000 rows when SuiteAnalytics Connect is not enabled; no
  total ceiling when it is.
- The results do not tell you the column names. When you need them, build a `query.SuiteQL` object
  and use [`SuiteQL.runPaged`](#suiteqlrunpagedoptions) instead.

### query.runSuiteQLPaged.promise(options)
<!-- id: N/query.runSuiteQLPaged.promise -->

Promise form of [`query.runSuiteQLPaged`](#queryrunsuiteqlpagedoptions), declared with the same
parameters and errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.query` | string | Yes | |
| `options.pageSize` | number | No | `50` |
| `options.params` | Array<string \| number \| boolean> | No | |
| `options.customScriptId` | string | No | |

- `options.pageSize`: 5 to 1,000 rows per page.

> [!SOURCE-ERROR] Here too the source's type cell and error row for `params` list only two types while
> the syntax sample passes a boolean. Treat boolean support as something to confirm.
<!-- source-error: N/query.runSuiteQLPaged.promise | options.params | type_cell_and_error_row_omit_third_type -->
<!-- source-error: N/query.runSuiteQLPaged.promise | options.params | source_type_list_incomplete -->

- `options.metaDataProvider`: not in the spec's parameter list for this variant, although it is
  declared to share the parameters of the non-promise function. The source does not state whether it
  is accepted here.

**Returns:** `Promise`

**Errors:**

- `MISSING_REQD_ARGUMENT`: a required parameter is missing.
- `SSS_INVALID_TYPE_ARG`: `options.params` holds a value that is not a string, number or boolean.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.1

- Order by unique, unambiguous columns, or pages may duplicate or skip rows.
- Up to 1,000 pages and, without SuiteAnalytics Connect, up to 100,000 rows in total.
- Column names cannot be read from the results; use [`SuiteQL.runPaged`](#suiteqlrunpagedoptions)
  when you need them.

### query.createRelativeDate(options)
<!-- id: N/query.createRelativeDate -->

Builds a [`RelativeDate`](#relativedate-object) to use in condition values: a point counted back from,
or forward from, now. Three weeks back, three weeks ahead and five days back are typical uses.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.dateId` | string | Yes | |
| `options.value` | number | Yes | |

- `options.dateId`: a [`query.DateId`](#querydateid) value, which sets the unit and direction.
- `options.value`: how many of that unit; its meaning comes from `dateId`. With `DAYS_AGO`, a value
  of 5 means five days back.

**Returns:** `query.RelativeDate`

**Errors:**

- `INVALID_DATE_ID`: `options.dateId` is not a `query.DateId` value.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.1

### query.createPeriod(options)
<!-- id: N/query.createPeriod -->

Builds a [`Period`](#period-object) object that you can use as a condition value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.code` | string | Yes | |
| `options.adjustment` | string | No | `NOT_LAST` |
| `options.type` | string | No | `START` |

- `options.code`: a [`query.PeriodCode`](#queryperiodcode) value.
- `options.adjustment`: a [`query.PeriodAdjustment`](#queryperiodadjustment) value.
- `options.type`: a [`query.PeriodType`](#queryperiodtype) value.

**Returns:** `query.Period`

**Errors:**

- `INVALID_PERIOD_ADJUSTMENT`: the adjustment value is not valid.
- `INVALID_PERIOD_CODE`: the code value is not valid.
- `INVALID_PERIOD_TYPE`: the type value is not valid.
- `WRONG_PARAMETER_TYPE`: an argument has the wrong type.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.1

## Objects

### Query (object)
<!-- id: N/query.Query -->

Holds one query definition (see [Query definitions](../topics/query_definition.md)). You get one from
[`query.create`](#querycreateoptions) or [`query.load`](#queryloadoptions). Every query always has a
root [`Component`](#component-object), reachable as [`Query.root`](#queryroot). Available in client
and server scripts since 2018.1.

#### Query.autoJoin(options)
<!-- id: N/query.Query#autoJoin -->

Creates the first join of the query, starting at the root. It is shorthand for calling
`Query.root.autoJoin`; for joins further down the tree, call `autoJoin` on the component you are
joining from ([`Component.autoJoin`](#componentautojoinoptions)).

> [!SOURCE-ERROR] The source's note on this shortcut shows `Query.root.join` rather than
> `Query.root.autoJoin`. Both behave alike (`join` is an alias of `autoJoin`), but the note seems to
> have been copied from the `join` page; treat `Query.root.autoJoin` as the equivalent.
<!-- source-error: N/query.Query#autoJoin | - | shortcut_note_shows_query_root_join -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `query.Component`, the joined component.

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: this join relationship already exists in the query.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Query.join(options)
<!-- id: N/query.Query#join -->

An alias of [`Query.autoJoin`](#queryautojoinoptions); it does the same as `Query.root.join`. For
simple joins, `autoJoin` is the recommended name; for anything that needs a direction, use
[`joinTo`](#queryjointooptions) or [`joinFrom`](#queryjoinfromoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

**Returns:** `query.Component`

**Errors:** the source lists none for this method itself; since it is an alias of `autoJoin`, expect
`RELATIONSHIP_ALREADY_USED` when the join relationship already exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.joinFrom(options)
<!-- id: N/query.Query#joinFrom -->

Creates the first join as an inverse join, going from a child type back to the root. It sets the new
component's [`source`](#componentsource).

> [!SOURCE-ERROR] The source's module summary row describes this method in terms of a search
> definition. It works on the query definition, like the other join methods on `Query`.
<!-- source-error: N/query.Query#joinFrom | - | module_row_says_search_definition -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.source` | string | Yes | |

**Returns:** `query.Component`

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: the join relationship already exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Query.joinTo(options)
<!-- id: N/query.Query#joinTo -->

Creates the first join as a forward join, from the root to a target type. It sets the new component's
[`target`](#componenttarget).

> [!SOURCE-ERROR] The source's shortcut note for this method shows `Query.root.autoJoin` rather than
> `Query.root.joinTo`, which looks copied from the `autoJoin` page. Read `Query.joinTo` as shorthand
> for joining forward from the root with `joinTo`.
<!-- source-error: N/query.Query#joinTo | - | shortcut_note_shows_query_root_autojoin -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.target` | string | Yes | |

**Returns:** `query.Component`

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: the join relationship already exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Query.createColumn(options)
<!-- id: N/query.Query#createColumn -->

Creates a result column on the root type, the initial type of the query. It does the same as
`Query.root.createColumn` and is comparable to
[`Component.createColumn`](#componentcreatecolumnoptions), which you use for columns on joined types.
Give it a field ID, or a formula with an optional return type, and put the result in
[`Query.columns`](#querycolumns). Look field IDs up in the Records Catalog.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.fieldId` | string | No | |
| `options.formula` | string | No | |
| `options.type` | string | No | |
| `options.aggregate` | string | No | |
| `options.groupBy` | boolean | No | `false` |
| `options.context` | Object | No | |
| `options.context.name` | string | No | |
| `options.context.params` | Object | No | |
| `options.context.params.date` | query.RelativeDate \| Date | No | |
| `options.context.params.currencyId` | number | No | |
| `options.alias` | string | No | |
| `options.label` | string | No | |

- `options.fieldId` / `options.formula`: supply one of the two (each is conditionally required).
- `options.type`: a [`query.ReturnType`](#queryreturntype) value, the formula's return type;
  conditionally required (needed with a formula).
- `options.context`: how values are presented. Without it, the column uses
  `query.FieldContext.RAW`. `name`, `params` and the two `params` members are conditionally required,
  depending on the context chosen (the `CONVERTED` context needs `params`; see
  [`Component.createColumn`](#componentcreatecolumnoptions)). If `params.date` is a relative date
  given without saying whether its start or end is meant, its end is used. Example uses: a net amount
  converted to currency 4 at the rate on 1 January 2019, or converted at the rate at the start of the
  last fiscal quarter.
- `options.alias`: the key the column gets in mapped results. Some cases need it (see
  [`Column.alias`](#columnalias)).
- `options.label`: a label used when the query feeds a printed document.

**Returns:** `query.Column`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.createCondition(options)
<!-- id: N/query.Query#createCondition -->

Creates a [`Condition`](#condition-object) that narrows the results. It is shorthand for
`Query.root.createCondition`; for a condition on a joined component, call
[`Component.createCondition`](#componentcreateconditionoptions) on that component instead. Conditions
are like `N/search` filters, with one difference: a condition can contain other conditions. You build
one either from a field ID, an operator and values (for example, city equals Boston) or from a formula
plus its return type. Combine conditions with [`and`](#queryandconditions),
[`or`](#queryorconditions) and [`not`](#querynotcondition), then assign the result to
[`Query.condition`](#querycondition).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | No | |
| `options.operator` | string | Yes | |
| `options.values` | string \| number \| Date \| string[] \| number[] \| Date[] \| query.RelativeDate[] \| query.Period[] | No | |
| `options.formula` | string | No | |
| `options.type` | string | No | |
| `options.aggregate` | string | No | |
| `options.caseSensitive` | boolean | No | `false` |

- `options.fieldId`: sets [`Condition.fieldId`](#conditionfieldid). Conditionally required (when no
  formula is used). Field IDs come from the Records Catalog.
- `options.operator`: a [`query.Operator`](#queryoperator) value; sets
  [`Condition.operator`](#conditionoperator).
- `options.values`: sets [`Condition.values`](#conditionvalues). Conditionally required: leave it out
  for `EMPTY` and `EMPTY_NOT`.
- `options.formula`: sets [`Condition.formula`](#conditionformula). Required when `fieldId` is not
  used.
- `options.type`: a [`query.ReturnType`](#queryreturntype) value; sets
  [`Condition.type`](#conditiontype). Required with a formula.
- `options.aggregate`: a [`query.Aggregate`](#queryaggregate) value; sets
  [`Condition.aggregate`](#conditionaggregate). The condition then compares one value computed over
  the matching values.
- `options.caseSensitive`: only valid for text fields; using it with numbers or dates can cause script
  errors. It can make the query faster.

**Returns:** `query.Condition`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.createSort(options)
<!-- id: N/query.Query#createSort -->

Creates a [`Sort`](#sort-object) on a column. It is the same as `Query.root.createSort` and works like
[`Component.createSort`](#componentcreatesortoptions). Put the result in [`Query.sort`](#querysort).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.column` | query.Column | Yes | |
| `options.ascending` | boolean | No | `true` |
| `options.caseSensitive` | boolean | No | `false` |
| `options.locale` | string | No | |
| `options.nullsLast` | boolean | No | |

- `options.ascending`: `true` sorts ascending.
- `options.caseSensitive`: `true` puts uppercase before lowercase in an ascending sort.
- `options.locale`: a [`query.SortLocale`](#querysortlocale) value, for locale-specific sort rules.
- `options.nullsLast`: when omitted, it takes the same value as `ascending`.

**Returns:** `query.Sort`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.and(conditions)
<!-- id: N/query.Query#and -->

Returns a new condition that is the logical AND of the conditions passed to it. Pass the conditions
as separate arguments, from one up to as many as you need; there is no limit. Assign the result to
[`Query.condition`](#querycondition) (or nest it further).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `condition` | query.Condition[] | Yes | |

- `condition`: the spec types this parameter as an array of conditions and describes the arguments
  as condition 1 through condition n, that is, one or more conditions given one after another.

**Returns:** `query.Condition`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.or(conditions)
<!-- id: N/query.Query#or -->

Returns a new condition that is the logical OR of the conditions passed in, with no limit on their
number. Assign it to [`Query.condition`](#querycondition) or nest it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `condition` | query.Condition[] | Yes | |

- `condition`: typed as an array of conditions; the arguments are condition 1 through condition n,
  passed one after another.

**Returns:** `query.Condition`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.not(condition)
<!-- id: N/query.Query#not -->

Returns a new condition that negates the one passed in. Assign it to
[`Query.condition`](#querycondition) or nest it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `condition` | query.Condition | Yes | |

**Returns:** `query.Condition`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.run(options)
<!-- id: N/query.Query#run -->

Runs the query and returns every row at once. At most 5,000 rows come back; for more, use
[`runPaged`](#queryrunpagedoptions) or its promise form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.customScriptId` | string | No | |
| `options.metaDataProvider` | string | No | `SUITE_QL` |

- `options.customScriptId`: names the query so that performance fixes can target it. Keep it unique;
  a fix applies to every query that shares the ID.
- `options.metaDataProvider`: `SUITE_QL` makes the run fail when the user lacks permission to a field
  or record; `STATIC` lets it succeed, returning no data for the fields the user cannot see.

**Returns:** `query.ResultSet`

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.run.promise(options)
<!-- id: N/query.Query#run.promise -->

Promise form of [`Query.run`](#queryrunoptions). The spec declares that it takes the same parameters
and raises the same errors as `Query.run` (it records them only on `run`; they are repeated here).
The Promise resolves to a [`query.ResultSet`](#resultset-object).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.customScriptId` | string | No | |
| `options.metaDataProvider` | string | No | `SUITE_QL` |

**Returns:** `Promise` that resolves to a `query.ResultSet`.

**Errors:** the same as `Query.run`, for which the source states none.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Query.runPaged(options)
<!-- id: N/query.Query#runPaged -->

Runs the query and returns a [`PagedData`](#pageddata-object) handle for reading the results a page
at a time. Pages hold 5 to 1,000 rows; the last page may hold fewer than the minimum.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.pageSize` | string | No | `50` |
| `options.customScriptId` | string | No | |
| `options.metaDataProvider` | string | No | `SUITE_QL` |

> [!SOURCE-ERROR] The source types `pageSize` as a string, but it is a row count (default 50, range
> 5 to 1,000), and every other paged call types it as a number. Pass a number.
<!-- source-error: N/query.Query#runPaged | options.pageSize | page_size_typed_as_string -->

- `options.customScriptId`: identifies the query for performance fixes; must be unique, or a fix
  reaches every query using the same ID.
- `options.metaDataProvider`: `SUITE_QL` fails on missing field or record permission; `STATIC`
  succeeds and leaves out the data the user may not see.

**Returns:** `query.PagedData`

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2018.1

- Sort on something unique and unambiguous; otherwise rows may be repeated or lost across pages.

#### Query.runPaged.promise(options)
<!-- id: N/query.Query#runPaged.promise -->

Promise form of [`Query.runPaged`](#queryrunpagedoptions), declared with the same parameters and
errors (listed in the spec only on `runPaged`, repeated here). The Promise resolves to a
[`query.PagedData`](#pageddata-object).

> [!SOURCE-ERROR] The source's module summary types this method's return as `query.PagedData`. Like
> every promise variant it returns a Promise, which then resolves to the `PagedData`.
<!-- source-error: N/query.Query#runPaged.promise | - | module_row_types_runpaged_promise_as_pageddata -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.pageSize` | string | No | `50` |
| `options.customScriptId` | string | No | |
| `options.metaDataProvider` | string | No | `SUITE_QL` |

- `options.pageSize`: typed as a string by the source; see the note under `runPaged` and pass a
  number.

**Returns:** `Promise` that resolves to a `query.PagedData`.

**Errors:** the same as `Query.runPaged`, for which the source states none.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2018.1

- Needs a unique, unambiguous sort order, or pages can contain duplicates or miss rows.

#### Query.toSuiteQL()
<!-- id: N/query.Query#toSuiteQL -->

Converts the definition into an equivalent [`SuiteQL`](#suiteql-object) object that returns the same
results. Column aliases do not appear in the generated SQL text (see [`Column.alias`](#columnalias)).
See also [SuiteQL in the query definition](../topics/query_definition.suiteql.md).

**Returns:** `query.SuiteQL`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.1

#### Query.type
<!-- id: N/query.Query#type -->

`string`, read-only. The initial query type, as set by [`query.create`](#querycreateoptions). Since
2018.1.

#### Query.columns
<!-- id: N/query.Query#columns -->

`query.Column[]`. The columns the results will have. Since 2018.1.

> [!SOURCE-ERROR] The source's description of this property says `createColumn` creates conditions;
> it creates columns. Fill this array with `Column` objects from `createColumn`.
<!-- source-error: N/query.Query#columns | - | says_createcolumn_creates_conditions -->

#### Query.condition
<!-- id: N/query.Query#condition -->

`query.Condition`. The query's filter: either a single condition or a nested one built with
`and` / `or` / `not`. Since 2018.1.

#### Query.sort
<!-- id: N/query.Query#sort -->

`query.Sort[]`. The sorts applied to the results. Since 2018.1. (See the module-level note about the
source typing this as a column array.)

#### Query.root
<!-- id: N/query.Query#root -->

`query.Component`, read-only. The root component, created automatically together with the query and
typed with the query's initial type. Since 2018.1.

#### Query.child
<!-- id: N/query.Query#child -->

`Object`. Maps child names to the [`Component`](#component-object) objects joined into the query,
following the parent/child hierarchy. Since 2018.1.

> [!SOURCE-ERROR] The source describes this property as the children "of this component", wording
> that belongs to `Component.child`. For a `Query`, read it as the components joined below the query.
<!-- source-error: N/query.Query#child | - | describes_children_of_this_component -->

#### Query.id
<!-- id: N/query.Query#id -->

`number`, read-only. Only set when the query was loaded with [`query.load`](#queryloadoptions); for a
query created in the script and not saved it is `null`. Since 2018.1.

#### Query.name
<!-- id: N/query.Query#name -->

`string`, read-only. Only set for a query obtained with [`query.load`](#queryloadoptions); `null` for
a query created and not saved. Since 2018.1.

### Component (object)
<!-- id: N/query.Component -->

One building block of a query definition: a query type at one point in the join tree. The root
component comes with the query; every join adds a new component whose parent is the component you
joined from. The last component in a chain stands for the relationship to all of its parents.
Available in client and server scripts since 2018.1.

#### Component.autoJoin(options)
<!-- id: N/query.Component#autoJoin -->

Adds a further join below this component (use [`Query.autoJoin`](#queryautojoinoptions) for the
first one).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

- `options.fieldId`: take it from the Records Catalog, not the Records Browser.

**Returns:** `query.Component`

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: this join relationship is already in the query.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Component.join(options)
<!-- id: N/query.Component#join -->

An alias of [`Component.autoJoin`](#componentautojoinoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |

- `options.fieldId`: decides both the columns used for the join and the type of the new component.
  Use Records Catalog IDs, not Records Browser IDs.

**Returns:** `query.Component`

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: the join relationship already exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Component.joinFrom(options)
<!-- id: N/query.Component#joinFrom -->

Adds an inverse join, from a child type back to this component (child to parent). Sets the new
component's [`source`](#componentsource).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.source` | string | Yes | |

- `options.source`: the query type the new component comes from.

**Returns:** `query.Component`

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: the join relationship already exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Component.joinTo(options)
<!-- id: N/query.Component#joinTo -->

Adds a forward join, from this component to a target type (parent to child). Sets the new component's
[`target`](#componenttarget).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | Yes | |
| `options.target` | string | Yes | |

- `options.target`: the query type of the new component.

**Returns:** `query.Component`

**Errors:**

- `RELATIONSHIP_ALREADY_USED`: the join relationship already exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Component.createColumn(options)
<!-- id: N/query.Component#createColumn -->

Creates a column on this component's type, from a field ID or from a formula with an optional return
type. Assign it into [`Query.columns`](#querycolumns). Field IDs are listed in the Records Catalog.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.fieldId` | string | No | |
| `options.formula` | string | No | |
| `options.type` | string | No | |
| `options.aggregate` | string | No | |
| `options.groupBy` | boolean | No | `false` |
| `options.context` | Object | No | |
| `options.context.name` | string | No | |
| `options.context.params` | Object | No | |
| `options.context.params.currencyId` | number | No | |
| `options.context.params.date` | query.RelativeDate \| Date | No | |
| `options.label` | string | No | |

- `options.fieldId` / `options.formula`: one of the two is needed.
- `options.type`: a [`query.ReturnType`](#queryreturntype) value; required with a formula.
- `options.aggregate`: a [`query.Aggregate`](#queryaggregate) value.
- `options.groupBy`: `true` groups the results by this column.
- `options.context`: either a [`query.FieldContext`](#queryfieldcontext) value, or an object with
  `name` (a `FieldContext` value) and `params`. Only the `CONVERTED` context uses `params`, which has
  two members: `currencyId` (internal ID of the currency to convert to) and `date` (the date whose
  exchange rate is used). Both are required when the context is `CONVERTED`.
- `options.label`: a label for when the query is the data source for printing.
- The source does not list an `alias` option for this method (only `Query.createColumn` has one), so
  it does not state whether columns created here can be given an alias.

**Returns:** `query.Column`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Component.createCondition(options)
<!-- id: N/query.Component#createCondition -->

Creates a condition on this component's type, from a field ID with operator and values, or from a
formula with an optional return type. Combine conditions with `Query.and`, `Query.or` and `Query.not`
and assign the result to [`Query.condition`](#querycondition).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fieldId` | string | No | |
| `options.operator` | string | Yes | |
| `options.values` | string[] \| number[] \| Date[] \| query.RelativeDate[] \| query.Period[] | No | |
| `options.formula` | string | No | |
| `options.type` | string | No | |
| `options.aggregate` | string | No | |
| `options.caseSensitive` | boolean | No | `false` |

- `options.fieldId` / `options.formula`: one of the two (each conditionally required).
- `options.operator`: a [`query.Operator`](#queryoperator) value.
- `options.values`: not needed with `EMPTY` or `EMPTY_NOT`. Unlike the `Query` version, this method
  takes only arrays (of strings, numbers, dates, relative dates or periods), not single values.
- `options.type`: a [`query.ReturnType`](#queryreturntype) value; required with a formula.
- `options.aggregate`: a [`query.Aggregate`](#queryaggregate) value.
- `options.caseSensitive`: for text fields; with number or date values it can raise errors. It may
  improve performance.

**Returns:** `query.Condition`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Component.createSort(options)
<!-- id: N/query.Component#createSort -->

Creates a sort on a column; comparable to [`Query.createSort`](#querycreatesortoptions). Put the
result in [`Query.sort`](#querysort).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.column` | query.Column | Yes | |
| `options.ascending` | boolean | No | `true` |
| `options.caseSensitive` | boolean | No | `false` |
| `options.locale` | string | No | |
| `options.nullsLast` | boolean | No | |

- `options.ascending`: `true` for ascending order.
- `options.caseSensitive`: `true` sorts uppercase ahead of lowercase when ascending.
- `options.locale`: a [`query.SortLocale`](#querysortlocale) value for locale-specific ordering.
- `options.nullsLast`: defaults to whatever `ascending` is.

**Returns:** `query.Sort`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### Component.type
<!-- id: N/query.Component#type -->

`string`, read-only. The query type of this component. Since 2018.1.

#### Component.parent
<!-- id: N/query.Component#parent -->

Read-only. The parent component. Since 2018.1. The spec gives its type as `string`.

> [!SOURCE-ERROR] The source's type row says `string`, but the property refers to a parent component.
> Expect a `query.Component` and check the actual value before relying on either.
<!-- source-error: N/query.Component#parent | - | type_row_says_string_for_component_reference -->

#### Component.child
<!-- id: N/query.Component#child -->

`Object`, read-only. Maps child names to the components joined below this one, following the
parent/child hierarchy. Since 2018.1.

#### Component.target
<!-- id: N/query.Component#target -->

`string`, read-only. For a forward join, the target query type; set by `joinTo` on a query or
component. Since 2018.1.

#### Component.source
<!-- id: N/query.Component#source -->

`string`, read-only. For an inverse join, the source query type; set by `joinFrom` on a query or
component. Since 2018.1.

### Column (object)
<!-- id: N/query.Column -->

A result column, comparable to an `N/search` column. Created by `Query.createColumn` or
`Component.createColumn`. Even though `columns` is optional in `query.create`, a query definition
needs at least one column. All properties below are set through `createColumn`. Available in client
and server scripts since 2018.1.

#### Column.fieldId
<!-- id: N/query.Column#fieldId -->

`string`, read-only. The field the column shows. A column has either this or a `formula`, not both.
Since 2018.1.

#### Column.formula
<!-- id: N/query.Column#formula -->

`string`, read-only. The formula the column computes; excludes `fieldId`. Since 2018.1.

#### Column.type
<!-- id: N/query.Column#type -->

`string`, read-only. The formula's return type; `null` when the column has no formula. Since 2018.1.

#### Column.component
<!-- id: N/query.Column#component -->

`query.Component`, read-only. The component the column belongs to. Since 2018.1.

#### Column.aggregate
<!-- id: N/query.Column#aggregate -->

`string`, read-only. A [`query.Aggregate`](#queryaggregate) value. Since 2018.1.

#### Column.groupBy
<!-- id: N/query.Column#groupBy -->

Read-only. When true, results are grouped by this column. Since 2018.1. The spec's type is `any`,
meaning it has no usable type for this property; the `createColumn` parameter of the same name is a
boolean.

> [!SOURCE-ERROR] The source leaves this property's type row blank. Treat it as a boolean, matching
> the `groupBy` parameter of `createColumn`, but confirm before depending on it.
<!-- source-error: N/query.Column#groupBy | - | type_row_blank -->

#### Column.context
<!-- id: N/query.Column#context -->

`Object`, read-only. How the column's values are presented: raw IDs, consolidated or converted
amounts, display names and so on. It uses a [`query.FieldContext`](#queryfieldcontext) value, and
only the `CONVERTED` context takes parameters. Since 2019.1.

#### Column.alias
<!-- id: N/query.Column#alias -->

`string`. The key this column gets in mapped results ([`Result.asMap`](#resultasmap),
[`ResultSet.asMappedResults`](#resultsetasmappedresults)). For mapped results you must set it when the
column is a formula, or when two columns share a field ID (for instance an `entity` field taken from
two joined types). Set through `createColumn`. Since 2019.2.

#### Column.label
<!-- id: N/query.Column#label -->

`string`, read-only. Use it when the query serves as a data source for printing through
`TemplateRenderer.addQuery` in [N/render](N_render.md). Since 2019.2.

### Condition (object)
<!-- id: N/query.Condition -->

A filter on the results, comparable to an `N/search` filter except that a condition can contain other
conditions. Combine conditions with `Query.and`, `Query.or` and `Query.not`. Available in client and
server scripts since 2018.1.

A parent condition produced by `and` / `or` / `not` has only `children`; the other properties below
(set by `createCondition`) do not apply to such parent conditions.

#### Condition.children
<!-- id: N/query.Condition#children -->

`query.Condition[]`, read-only. The child conditions of a parent condition built with
`and` / `or` / `not`. Since 2018.1.

#### Condition.fieldId
<!-- id: N/query.Condition#fieldId -->

`string`, read-only. The field the condition tests. Since 2018.1.

#### Condition.operator
<!-- id: N/query.Condition#operator -->

`string`, read-only. The name of the operator used. Since 2018.1.

#### Condition.values
<!-- id: N/query.Condition#values -->

`string[] | number[] | Date[] | query.RelativeDate[] | query.Period`, read-only. The values given to
the operator. Since 2018.1.

> [!SOURCE-ERROR] The source types the last member as a single `query.Period`, while both
> `createCondition` methods take `query.Period[]`. Expect an array, as you passed it, and check the
> value before relying on either form.
<!-- source-error: N/query.Condition#values | - | type_query.Period_vs_createcondition_query.Period_array -->

#### Condition.formula
<!-- id: N/query.Condition#formula -->

`string`, read-only. The formula the condition evaluates. Since 2018.1.

#### Condition.type
<!-- id: N/query.Condition#type -->

`string`, read-only. The formula's return type. Since 2018.1.

#### Condition.aggregate
<!-- id: N/query.Condition#aggregate -->

`string`, read-only. A [`query.Aggregate`](#queryaggregate) value. Since 2018.1.

#### Condition.component
<!-- id: N/query.Condition#component -->

`query.Component`, read-only. The component the condition was created on. Since 2018.1.

### Sort (object)
<!-- id: N/query.Sort -->

A sort on a result column. Created by `Query.createSort` or `Component.createSort`. Available in
client and server scripts since 2018.1.

#### Sort.column
<!-- id: N/query.Sort#column -->

`query.Column`, read-only. The column the results are sorted by. Since 2018.1.

#### Sort.ascending
<!-- id: N/query.Sort#ascending -->

`boolean`. `true` (the default) means ascending order. Since 2018.2.

#### Sort.caseSensitive
<!-- id: N/query.Sort#caseSensitive -->

`boolean`. When `true`, uppercase values come before lowercase ones in an ascending sort. Defaults to
`false`. Since 2018.2.

#### Sort.locale
<!-- id: N/query.Sort#locale -->

`string`. A [`query.SortLocale`](#querysortlocale) value, applying that locale's sort rules. Since
2018.2.

#### Sort.nullsLast
<!-- id: N/query.Sort#nullsLast -->

`boolean`. When `true`, empty values come last. If not set, it follows `ascending`. Since 2018.2.

### ResultSet (object)
<!-- id: N/query.ResultSet -->

The rows of a run. The spec names `Query.run` and `Query.run.promise` as its creators; by their return
types, [`query.runSuiteQL`](#queryrunsuiteqloptions), [`SuiteQL.run`](#suiteqlrunoptions) and
`Dataset.run` in [N/dataset](N_dataset.md) also hand back a `ResultSet`. It holds at most 5,000 rows;
use `Query.runPaged` when there may be more. Available in client and server scripts since 2018.1.

#### ResultSet.iterator()
<!-- id: N/query.ResultSet#iterator -->

Returns an iterator over the results (see the global `Iterator` in [globals](../globals.md)).

**Returns:** `Iterator`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### ResultSet.asMappedResults()
<!-- id: N/query.ResultSet#asMappedResults -->

Returns every row as a plain object, by calling [`Result.asMap`](#resultasmap) on each. Keys are field
IDs or column aliases (see the [SuiteQL topic](../topics/suiteql.md) on the letter case of keys).

**Returns:** `Object[]`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### ResultSet.results
<!-- id: N/query.ResultSet#results -->

`query.Result[]`, read-only. The rows, as [`Result`](#result-object) objects. Since 2018.1.

#### ResultSet.types
<!-- id: N/query.ResultSet#types -->

`string[]`, read-only. The return type of each column, in the same order as `columns`. Since 2018.1.

#### ResultSet.columns
<!-- id: N/query.ResultSet#columns -->

`query.Column[]`, read-only. The result columns, in the same order as `types`. Since 2018.1.

### Result (object)
<!-- id: N/query.Result -->

One row of results. Available in client and server scripts since 2018.1.

#### Result.getValue(options)
<!-- id: N/query.Result#getValue -->

Returns the value at a given position in [`Result.values`](#resultvalues). Its type matches the
corresponding entry in [`ResultSet.types`](#resultsettypes).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | number | Yes | |

- The spec names this parameter `options` but types it as a number: the column index.

**Returns:** `string | number | Date | null`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Result.asMap()
<!-- id: N/query.Result#asMap -->

Returns the row as an object keyed by field ID or, where set, by column alias.

**Returns:** `Object`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### Result.values
<!-- id: N/query.Result#values -->

`Array<string | number | null>`, read-only. The row's values, in the order of
[`ResultSet.columns`](#resultsetcolumns), with types matching [`ResultSet.types`](#resultsettypes).
Since 2018.1.

> [!SOURCE-ERROR] The source's type cell for this property has an empty member in its type union, so
> the list of types is incomplete. `getValue` on the same row can return a `Date`, and
> `query.ReturnType` includes `BOOLEAN`, so expect dates and possibly booleans as well.
<!-- source-error: N/query.Result#values | - | type_cell_has_empty_union_member -->
<!-- source-error: N/query.Result#values | - | source_type_list_incomplete -->

### PagedData (object)
<!-- id: N/query.PagedData -->

A handle to paged results. The spec names `Query.runPaged` and `Query.runPaged.promise` as its
creators; [`query.runSuiteQLPaged`](#queryrunsuiteqlpagedoptions) (and its promise form, once
resolved), [`SuiteQL.runPaged`](#suiteqlrunpagedoptions) and `Dataset.runPaged` in
[N/dataset](N_dataset.md) also return a `query.PagedData`. Pages hold between 5 and 1,000 rows; the
last page may hold fewer. Available in client and server scripts since 2018.1.

#### PagedData.fetch(options)
<!-- id: N/query.PagedData#fetch -->

Fetches one page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.index` | number | Yes | |

- `options.index`: the page number, counting from 0.

**Returns:** `query.Page`

**Errors:**

- `INVALID_PAGE_INDEX`: the index is not a number.
- `INVALID_PAGE_RANGE`: the index is negative or past the last page.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### PagedData.fetch.promise(options)
<!-- id: N/query.PagedData#fetch.promise -->

Promise form of [`PagedData.fetch`](#pageddatafetchoptions), declared with the same parameters and
errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.index` | number | Yes | |

- `options.index`: zero-based page index.

**Returns:** `Promise`

**Errors:**

- `INVALID_PAGE_INDEX`: the index has the wrong type.
- `INVALID_PAGE_RANGE`: the index is out of range.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.1

#### PagedData.iterator()
<!-- id: N/query.PagedData#iterator -->

Returns an iterator over the pages (see the global `Iterator` in [globals](../globals.md)).

**Returns:** `Iterator`

**Governance:** 10 units. Another source gives 0; the spec does not say which is right.

> [!SOURCE-ERROR] The sources disagree on this method's cost: one gives 10 units, another 0. Budget
> for 10 until you have measured it.
<!-- source-error: N/query.PagedData#iterator | - | governance_sources_disagree -->

**Availability:** client and server scripts.

**Since:** 2018.1

#### PagedData.count
<!-- id: N/query.PagedData#count -->

`number`, read-only. The total number of result rows. Since 2018.1.

#### PagedData.pageSize
<!-- id: N/query.PagedData#pageSize -->

`number`, read-only. Rows per page, between 5 and 1,000. Since 2018.1.

#### PagedData.pageRanges
<!-- id: N/query.PagedData#pageRanges -->

`query.PageRange[]`. One entry per page, giving its index and size. Since 2018.1.

### Page (object)
<!-- id: N/query.Page -->

One page of paged results. Available in client and server scripts since 2018.1.

#### Page.data
<!-- id: N/query.Page#data -->

`query.ResultSet`, read-only. The rows on this page. Since 2018.1.

#### Page.pageRange
<!-- id: N/query.Page#pageRange -->

`query.PageRange`, read-only. The range of results this page covers. Since 2018.1.

#### Page.pagedData
<!-- id: N/query.Page#pagedData -->

`query.PagedData`, read-only. The paged results this page belongs to. Since 2018.1.

#### Page.isFirst
<!-- id: N/query.Page#isFirst -->

`boolean`, read-only. `true` on the first page. Since 2018.1.

#### Page.isLast
<!-- id: N/query.Page#isLast -->

`boolean`, read-only. `true` on the last page. Since 2018.1.

### PageRange (object)
<!-- id: N/query.PageRange -->

Describes the range of results on one page. Available in client and server scripts since 2018.1.

#### PageRange.index
<!-- id: N/query.PageRange#index -->

`number`, read-only. The index of the page range. Since 2018.1.

#### PageRange.size
<!-- id: N/query.PageRange#size -->

`number`, read-only. How many rows the range holds. Since 2018.1.

### SuiteQL (object)
<!-- id: N/query.SuiteQL -->

A SuiteQL statement (a query language based on SQL-92) together with its parameters, produced by
[`Query.toSuiteQL`](#querytosuiteql). See the [SuiteQL topic](../topics/suiteql.md). Available in
client and server scripts since 2020.1.

#### SuiteQL.run(options)
<!-- id: N/query.SuiteQL#run -->

Runs the statement and returns the result set; equivalent to
[`query.runSuiteQL`](#queryrunsuiteqloptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.customScriptId` | string | No | |

- `options.customScriptId`: identifies the query for performance fixes; keep it unique, because a fix
  applies to every query with the same ID.

**Returns:** `query.ResultSet`

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.1

- The spec gives this method a ceiling of 100,000 rows when SuiteAnalytics Connect is not enabled,
  while `runSuiteQL`, which it calls equivalent, returns at most 5,000 (see the note there). The spec
  does not reconcile the two figures.

#### SuiteQL.runPaged(options)
<!-- id: N/query.SuiteQL#runPaged -->

Runs the statement page by page. Unlike `query.runSuiteQLPaged`, you keep access to the column
definitions through this object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.pageSize` | number | No | `50` |
| `options.customScriptId` | string | No | |

- `options.pageSize`: 5 to 1,000 rows per page.
- `options.customScriptId`: unique identifier for performance fixes; shared IDs mean shared fixes.

**Returns:** `query.PagedData`

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.1

- At most 1,000 pages. In total, 100,000 rows when SuiteAnalytics Connect is not enabled, with no
  ceiling when it is enabled.
- The statement must have a unique, unambiguous sort order, so order by columns that identify each
  row; otherwise rows may be duplicated or skipped across pages.

> [!SOURCE-ERROR] The ordering rule appears only on the source's REST web services page, not on the
> SuiteScript pages. It is likely to apply here as well, so keep an `ORDER BY` on unique columns, but
> the spec marks it for human review.
<!-- source-error: N/query.SuiteQL#runPaged | - | paging_order_rule_stated_on_rest_page_only -->

#### SuiteQL.columns
<!-- id: N/query.SuiteQL#columns -->

`query.Column[]`, read-only. The same columns as the query it was converted from. Column aliases are
not written into the SQL text. Since 2020.1.

#### SuiteQL.params
<!-- id: N/query.SuiteQL#params -->

`Array<string | number | boolean>`, read-only. Values for the `?` placeholders in the statement, in
the order the placeholders appear. For example, a statement filtering on `creditlimit <= ?` would
carry `50000` here. Since 2020.1.

> [!SOURCE-ERROR] The source's type cell and error row give two types for these values, while a
> syntax sample shows a boolean. The spec lists string, number and boolean; confirm booleans before
> relying on them.
<!-- source-error: N/query.SuiteQL#params | - | type_cell_and_error_row_omit_third_type -->
<!-- source-error: N/query.SuiteQL#params | - | source_type_list_incomplete -->

#### SuiteQL.query
<!-- id: N/query.SuiteQL#query -->

`string`, read-only. The SQL text. It can contain SQL clauses, table names, fields in dot notation,
operators, formatting characters, and comments that record field-context metadata.
`Query.toSuiteQL` may add such comments (for example a note that `entityid` is in the RAW context);
they change neither the query nor its results. Since 2020.1.

#### SuiteQL.type
<!-- id: N/query.SuiteQL#type -->

`string`, read-only. A [`query.Type`](#querytype-1) value, the same as the original query's type.
Since 2020.1.

### RelativeDate (object)
<!-- id: N/query.RelativeDate -->

A moment (or range) defined relative to today, used as a condition value. Created by
[`query.createRelativeDate`](#querycreaterelativedateoptions); the values of
[`query.RelativeDateRange`](#queryrelativedaterange) are ranges of this kind. Available in client and
server scripts since 2019.1.

#### RelativeDate.dateId
<!-- id: N/query.RelativeDate#dateId -->

`string`, read-only. A [`query.DateId`](#querydateid) value. Always present on `RelativeDateRange`
values. Since 2019.1.

#### RelativeDate.start
<!-- id: N/query.RelativeDate#start -->

`Object`, read-only. The start of the relative date. Always present on `RelativeDateRange` values.
Since 2019.1.

#### RelativeDate.end
<!-- id: N/query.RelativeDate#end -->

`Object`, read-only. The end of the relative date. Always present on `RelativeDateRange` values.
Since 2019.1.

#### RelativeDate.interval
<!-- id: N/query.RelativeDate#interval -->

`Object`, read-only. The interval that the `WITHIN` operators use. It is for inspection only; don't
use it in script logic. Since 2019.1.

#### RelativeDate.value
<!-- id: N/query.RelativeDate#value -->

`number`, read-only. The value passed to `createRelativeDate`; `undefined` on `RelativeDateRange`
values. Since 2019.1.

#### RelativeDate.isRange
<!-- id: N/query.RelativeDate#isRange -->

`boolean`, read-only. `true` for a range taken from `RelativeDateRange`, `false` for a single moment.
Since 2019.1.

### Period (object)
<!-- id: N/query.Period -->

A time period used as a condition value with `Query.createCondition` or `Component.createCondition`.
Created by [`query.createPeriod`](#querycreateperiodoptions). Available in client and server scripts
since 2020.1.

#### Period.code
<!-- id: N/query.Period#code -->

`string`, read-only. A [`query.PeriodCode`](#queryperiodcode) value. Since 2020.1.

#### Period.adjustment
<!-- id: N/query.Period#adjustment -->

`string`, read-only. A [`query.PeriodAdjustment`](#queryperiodadjustment) value; `NOT_LAST` unless
set otherwise. Since 2020.1.

#### Period.type
<!-- id: N/query.Period#type -->

`string`, read-only. A [`query.PeriodType`](#queryperiodtype) value; `START` unless set otherwise.
Since 2020.1.

## Enums

### query.Type
<!-- id: N/query.Type -->

The query types accepted by [`query.create`](#querycreateoptions). They are not the same as the record
types in `N/record` (`record.Type`). Which types you can use depends on the account, the role and the
enabled features. Custom record types are not in this enum. Since 2018.1.

| Value | String |
|---|---|
| `ACCOUNT` | `account` |
| `ACCOUNTING_BOOK` | `accountingbook` |
| `ACCOUNTING_CONTEXT` | `accountingcontext` |
| `ACCOUNTING_PERIOD` | `accountingperiod` |
| `ACTIVITY` | `activity` |
| `ADDRESS_BOOK` | `addressbook` |
| `ADVANCED_NUMBERING_LOG` | `advancednumberinglog` |
| `ADVANCED_PDF_TEMPLATE` | `advancedpdftemplate` |
| `ALL_PARSER_PLUGIN` | `allparserplugin` |
| `ALLOCATION_METHOD` | `allocationmethod` |
| `AMORTIZATION_SCHEDULE` | `amortizationschedule` |
| `AMORTIZATION_TEMPLATE` | `amortizationtemplate` |
| `AUTHORIZATION_CONSENT` | `authorizationconsent` |
| `AUTOMATED_CLEARING_HOUSE` | `automatedclearinghouse` |
| `BALANCING_SEGMENTS_PREFERENCE` | `balancingsegmentspreference` |
| `BILLING_CLASS` | `billingclass` |
| `BILLING_RATE_CARD` | `billingratecard` |
| `BILLING_SCHEDULE` | `billingschedule` |
| `BILL_OF_DISTRIBUTION` | `billofdistribution` |
| `BILL_RUN` | `billrun` |
| `BILL_RUN_SCHEDULE` | `billrunschedule` |
| `BIN` | `bin` |
| `BOM` | `bom` |
| `BOM_REVISION` | `bomrevision` |
| `BOM_REVISION_COMPONENT` | `bomrevisioncomponent` |
| `BUDGET_EXCHANGE_RATE` | `budgetexchangerate` |
| `BUDGET_LEGACY` | `budgetlegacy` |
| `BUDGETCATEGORY` | `budgetcategory` |
| `BUDGETIMPORT` | `budgetimport` |
| `BUDGETS` | `budgets` |
| `BULK_PROC_SUBMISSION` | `bulkprocsubmission` |
| `BUNDLE_INSTALLATION_SCRIPT` | `bundleinstallationscript` |
| `BUNDLE_INSTALLATION_SCRIPT_DEPLOYMENT` | `bundleinstallationscriptdeployment` |
| `BUSINESS_EVENTS_PROCESSING_HISTORY` | `businesseventsprocessinghistory` |
| `CALENDAR_EVENT` | `calendarevent` |
| `CAMPAIGN_AUDIENCE` | `campaignaudience` |
| `CAMPAIGN_CATEGORY` | `campaigncategory` |
| `CAMPAIGN_CHANNEL` | `campaignchannel` |
| `CAMPAIGN_EMAIL_ADDRESS` | `campaignemailaddress` |
| `CAMPAIGN_EVENT` | `campaignevent` |
| `CAMPAIGN_FAMILY` | `campaignfamily` |
| `CAMPAIGN_OFFER` | `campaignoffer` |
| `CAMPAIGN_RESPONSE` | `campaignresponse` |
| `CAMPAIGN_SEARCH_ENGINE` | `campaignsearchengine` |
| `CAMPAIGN_SUBSCRIPTION` | `campaignsubscription` |
| `CAMPAIGN_TEMPLATE` | `campaigntemplate` |
| `CAMPAIGN_VERTICAL` | `campaignvertical` |
| `CARDHOLDER_AUTHENTICATION` | `cardholderauthentication` |
| `CARDHOLDER_AUTHENTICATION_EVENT` | `cardholderauthenticationevent` |
| `CATEGORY1099MISC` | `category1099misc` |
| `CHARGE` | `charge` |
| `CHARGE_RULE` | `chargerule` |
| `CHARGE_RUN` | `chargerun` |
| `CHARGE_TYPE` | `chargetype` |
| `CLASSIFICATION` | `classification` |
| `CLIENT_SCRIPT` | `clientscript` |
| `CLIENT_SCRIPT_DEPLOYMENT` | `clientscriptdeployment` |
| `COMPANY_FEATURE_SETUP` | `companyfeaturesetup` |
| `COMPETITOR` | `competitor` |
| `CONSOLIDATED_EXCHANGE_RATE` | `consolidatedexchangerat` [sic] |
| `CONSOLIDATED_RATE_ADJUSTOR_PLUGIN` | `consolidatedrateadjustorplugin` |
| `CONTACT` | `contact` |
| `CONTACT_CATEGORY` | `contactcategory` |
| `CONTACT_ROLE` | `contactrole` |
| `CONTACT_SUBSIDIARY_RELATIONSHIP` | `contactsubsidiaryrelationship` |
| `COST_CATEGORY` | `costcategory` |
| `COUPON_CODE` | `couponcode` |
| `CURRENCY` | `currency` |
| `CURRENCY_RATE` | `currencyrate` |
| `CURRENCY_RATE_TYPE` | `currencyratetype` |
| `CUSTOM_SEGMENT_FIELD` | `customsegmentfield` |
| `CUSTOMER` | `customer` |
| `CUSTOMER_CATEGORY` | `customercategory` |
| `CUSTOMER_MESSAGE` | `customermessage` |
| `CUSTOMER_SEGMENT` | `customersegment` |
| `CUSTOMER_SUBSIDIARY_RELATIONSHIP` | `customersubsidiaryrelationship` |
| `CUSTOM_FIELD` | `customfield` |
| `CUSTOM_FIELD_2` | `customfield2` |
| `CUSTOM_GL_PLUGIN` | `customglplugin` |
| `CUSTOM_LIST` | `customlist` |
| `CUSTOM_RECORD_ACTION_SCRIPT` | `customrecordactionscript` |
| `CUSTOM_RECORD_TYPE` | `customrecordtype` |
| `CUSTOM_SEGMENT` | `customsegment` |
| `CUSTOM_TRANSACTION_TYPE` | `customtransactiontype` |
| `DATASET_BUILDER_PLUGIN` | `datasetbuilderplugin` |
| `DELETED_RECORD` | `deletedrecord` |
| `DELETED_RECORD_IN_CONNECT` | `deletedrecordinconnect` |
| `DEPARTMENT` | `department` |
| `DEVICE_ID` | `deviceid` |
| `DISTRIBUTION_CATEGORY` | `distributioncategory` |
| `DISTRIBUTION_NETWORK` | `distributionnetwork` |
| `DOMAIN` | `domain` |
| `DUAL` | `dual` |
| `EMAIL_CAPTURE_PLUGIN` | `emailcaptureplugin` |
| `EMAIL_TEMPLATE` | `emailtemplate` |
| `EMPLOYEE` | `employee` |
| `EMPLOYEE_EXPENSE_SOURCE_TYPE` | `employeeexpensesourcetype` |
| `EMPLOYEE_LIST` | `employeelist` |
| `EMPLOYEE_STATUS` | `employeestatus` |
| `EMPLOYEE_SUBSIDIARY_RELATIONSHIP` | `employeesubsidiaryrelationship` |
| `EMPLOYEE_TYPE` | `employeetype` |
| `ENTITY` | `entity` |
| `ENTITY_GROUP` | `entitygroup` |
| `ENTITY_SUBSIDIARY_RELATIONSHIP` | `entitysubsidiaryrelationship` |
| `EXPENSE_CATEGORY` | `expensecategory` |
| `EXPENSE_REPORT_POLICY` | `expensereportpolicy` |
| `FAX_TEMPLATE` | `faxtemplate` |
| `FILE` | `file` |
| `FISCAL_CALENDAR` | `fiscalcalendar` |
| `FI_CONNECTIVITY_PLUGIN` | `ficonnectivityplugin` |
| `FORECAST` | `forecast` |
| `FORMAT_PROFILE` | `formatprofile` |
| `FULFILLMENT_EXCEPTION_REASON` | `fulfillmentexceptionreason` |
| `FULFILLMENT_REQUEST` | `fulfillmentrequest` |
| `F_I_PARSER_PLUGIN` | `fiparserplugin` |
| `GATEWAY_NOTIFICATION` | `gatewaynotification` |
| `GENERAL_ALLOCATION_SCHEDULE` | `generalallocationschedule` |
| `GENERAL_TOKEN` | `generaltoken` |
| `GENERALIZED_ITEM` | `generalizeditem` |
| `GENERIC_RESOURCE` | `genericresource` |
| `GENERIC_RESOURCE_SUBISIDIARY_RELATIONSHIP` [sic] | `genericresourcesubsidiaryrelationship` |
| `GIFT_CERTIFICATE` | `giftcertificate` |
| `GLOBAL_ACCOUNT_MAPPING` | `globalaccountmapping` |
| `GLOBAL_INVENTORY_RELATIONSHIP` | `globalinventoryrelationship` |
| `GL_LINES_AUDIT_LOG` | `gllinesauditlog` |
| `GL_LINES_PLUGIN_REVISION` | `gllinespluginrevision` |
| `G_L_NUMBERING_SEQUENCE` | `glnumberingsequence` |
| `IMPORTED_EMPLOYEE_EXPENSE` | `importedemployeeexpense` |
| `INBOUND_SHIPMENT` | `inboundshipment` |
| `INCOTERM` | `incoterm` |
| `INVENTORY_COST_TEMPLATE` | `inventorycosttemplate` |
| `INVENTORY_NUMBER` | `inventorynumber` |
| `INVENTORY_STATUS` | `inventorystatus` |
| `INVOICE_GROUP` | `invoicegroup` |
| `INVT_ITEM_PRICE_HISTORY` | `invtitempricehistory` |
| `ISSUE` | `issue` |
| `ISSUE_PRIORITY` | `issuepriority` |
| `ISSUE_SEVERITY` | `issueseverity` |
| `ISSUE_STATUS` | `issuestatus` |
| `ITEM` | `item` |
| `ITEM_ACCOUNT_MAPPING` | `itemaccountmapping` |
| `ITEM_COLLECTION` | `itemcollection` |
| `ITEM_DEMAND_PLAN` | `itemdemandplan` |
| `ITEM_LOCATION_CONFIGURATION` | `itemlocationconfiguration` |
| `ITEM_PROCESS_FAMILY` | `itemprocessfamily` |
| `ITEM_PROCESS_GROUP` | `itemprocessgroup` |
| `ITEM_SEGMENT_CUSTOMER_SEGMENT_MAP` | `itemsegmentcustomersegmentmap` |
| `ITEM_SEGMENT_INCLUDING_SYNTHETIC` | `itemsegmentincludingsynthetic` |
| `ITEM_SUPPLY_PLAN` | `itemsupplyplan` |
| `I_P_RESTRICTIONS` | `iprestrictions` |
| `JOB` | `job` |
| `JOB_RESOURCE_ROLE` | `jobresourcerole` |
| `JOB_STATUS` | `jobstatus` |
| `JOB_TYPE` | `jobtype` |
| `KNOWLEDGE_BASE` | `knowledgebase` |
| `LOCATION` | `location` |
| `LOCATION_COSTING_GROUP` | `locationcostinggroup` |
| `LOGIN_AUDIT` | `loginaudit` |
| `MAIL_TEMPLATE` | `mailtemplate` |
| `MANUFACTURING_COMPONENT` | `manufacturingcomponent` |
| `MANUFACTURING_COST_TEMPLATE` | `manufacturingcosttemplate` |
| `MANUFACTURING_OPERATION_TASK` | `manufacturingoperationtask` |
| `MANUFACTURING_ROUTING` | `manufacturingrouting` |
| `MANUFACTURING_TRANSACTION` | `manufacturingtransaction` |
| `MAP_REDUCE_SCRIPT` | `mapreducescript` |
| `MAP_REDUCE_SCRIPT_DEPLOYMENT` | `mapreducescriptdeployment` |
| `MASS_UPDATE_SCRIPT` | `massupdatescript` |
| `MASS_UPDATE_SCRIPT_DEPLOYMENT` | `massupdatescriptdeployment` |
| `MEDIA_ITEM_FOLDER` | `mediaitemfolder` |
| `MEM_DOC` | `memdoc` |
| `MEM_DOC_TRANSACTION_TEMPLATE` | `memdoctransactiontemplate` |
| `MERCHANDISE_HIERARCHY_LEVEL` | `merchandisehierarchylevel` |
| `MERCHANDISE_HIERARCHY_NODE` | `merchandisehierarchynode` |
| `MERCHANDISE_HIERARCHY_VERSION` | `merchandisehierarchyversion` |
| `MESSAGE` | `message` |
| `MFG_PLANNED_TIME` | `mfgplannedtime` |
| `NETTING_STATEMENT` | `nettingstatement` |
| `NEXUS` | `nexus` |
| `NOTE` | `note` |
| `O_AUTH2_CLIENT_CREDENTIALS` | `oauth22clientcredentials` [sic] |
| `OCR_IMPORT_JOB` | `ocrimportjob` |
| `OCR_IMPORT_JOB_REVIEW` | `ocrimportjobreview` |
| `OCR_PLUGIN` | `ocrplugin` |
| `ONLINE_CASE_FORM` | `onlinecaseform` |
| `ONLINE_FORM_TEMPLATE` | `onlineformtemplate` |
| `ONLINE_LEAD_FORM` | `onlineleadform` |
| `ORDER_ALLOCATION_STRATEGY` | `orderallocationstrategy` |
| `ORDER_RELEASE_LIN` [sic] | `orderreleaseline` |
| `ORDER_RESERVATION` | `orderreservation` |
| `ORDER_TYPE_RECORD` | `ordertyperecord` |
| `OTHER_NAME` | `othername` |
| `OTHER_NAME_CATEGORY` | `othernamecategory` |
| `OTHER_NAME_SUBSIDIARY_RELATIONSHIP` | `othernamesubsidiaryrelationship` |
| `OUTBOUND_REQUEST` | `outboundrequest` |
| `O_AUTH_TOKEN` | `oauthtoken` |
| `PARTNER` | `partner` |
| `PARTNER_SUBSIDIARY_RELATIONSHIP` | `partnersubsidiaryrelationship` |
| `PAYCHECK` | `paycheck` |
| `PAYMENT_CARD` | `paymentcard` |
| `PAYMENT_CARD_SEARCH_RECORD` | `paymentcardsearchrecord` |
| `PAYMENT_CARD_TOKEN` | `paymentcardtoken` |
| `PAYMENT_EVENT` | `paymentevent` |
| `PAYMENT_GATEWAY_PLUGIN` | `paymentgatewayplugin` |
| `PAYMENT_INSTRUMENT` | `paymentinstrument` |
| `PAYMENT_METHOD` | `paymentmethod` |
| `PAYMENT_PROCESSING_PROFILE` | `paymentprocessingprofile` |
| `PAYMENT_RESULT_PREVIEW` | `aymentresultpreview` [sic] |
| `PAYROLL_BATCH` | `payrollbatch` |
| `PAYROLL_ITEM` | `payrollitem` |
| `PAYROLL_ITEM_GROUP` | `payrollitemgroup` |
| `PDF_TEMPLATE` | `pdftemplate` |
| `PERFORMANCE_REVIEW_SCHEDULE_TALENT_DATASET` | `performancereviewscheduletalentdataset` |
| `PHONE_CALL` | `phonecall` |
| `PICK_STRATEGY` | `pickstrategy` |
| `PICK_TASK` | `picktask` |
| `PICK_TASK_INVENTORY_BALANCE` | `picktaskinventorybalance` |
| `PLANNED_ORDER` | `plannedorder` |
| `PLANNED_STANDARD_COST` | `plannedstandardcost` |
| `PLANNING_ITEM_CATEGORY` | `planningitemcategory` |
| `PLANNING_ITEM_GROUP` | `planningitemgroup` |
| `PLANNING_ITEM_GROUP_SOURCE` | `planningitemgroupsource` |
| `PLANNING_RULE_GROUP` | `planningrulegroup` |
| `PLANNING_VIEW` | `planningview` |
| `PLATFORM_EXTENSION_PLUGIN` | `platformextensionplugin` |
| `PLUG_IN_TYPE` | `plugintype` |
| `PLUG_IN_TYPE_IMPL` | `plugintypeimpl` |
| `PORTLET` | `portlet` |
| `PORTLET_DEPLOYMENT` | `portletdeployment` |
| `POSTING_ACCOUNT_ACTIVITY` | `postingaccountactivity` |
| `PREDICTED_RISK_TRAIN_EVAL_HISTORY` | `predictedrisktrainevalhistory` |
| `PRICE_LEVEL` | `pricelevel` |
| `PRICING` | `pricing` |
| `PRICING_GROUP` | `pricinggroup` |
| `PRICING_WITH_CUSTOMERS` | `pricingwithcustomers` |
| `PROJECT_BUDGET` | `projectbudget` |
| `PROJECT_EXPENSE_TYPE` | `projectexpensetype` |
| `PROJECT_FINANCIALS` | `projectfinancials` |
| `PROJECT_IC_CHARGE_REQUEST` | `projecticchargerequest` |
| `PROJECT_SUBSIDIARY_RELATIONSHIP` | `projectsubsidiaryrelationship` |
| `PROJECT_TASK` | `projecttask` |
| `PROJECT_TEMPLATE` | `projecttemplate` |
| `PROJECT_TEMPLATE_SUBSIDIARY_RELATIONSHIP` | `projectemplatesubsidiaryrelationship` [sic] |
| `PROMOTIONS_PLUGIN` | `promotionsplugin` |
| `PROMOTION_CODE` | `promotioncode` |
| `PROMPT` | `prompt` |
| `PUBLISHED_SAVED_SEARCH` | `publishedsavedsearch` |
| `QUANTITY_PRICING_SCHEDULE` | `quantitypricingschedule` |
| `QUOTA` | `quota` |
| `RECENT_RECORD` | `recentrecord` |
| `RECORD_ACTION_SCRIPT_DEPLOYMENT` | `recordactionscriptdeployment` |
| `REC_SYS_ALGORITHM` | `recsysalgorithm` |
| `REC_SYS_ANALYTICS_REPORT` | `recsysanalyticsreport` |
| `REC_SYS_ANALYTICS_REPORT_AGG` | `recsysanalyticsreportagg` |
| `REC_SYS_BLOCKLIST` | `recsysblocklist` |
| `REC_SYS_CONVERSION` | `recsysconversion` |
| `REC_SYS_ELIGIBILITY` | `recsyseligibility` |
| `REC_SYS_SCENARIO` | `recsysscenario` |
| `REDIRECT` | `redirect` |
| `RESOURCE_ALLOCATION` | `resourceallocation` |
| `RESOURCE_GROUP` | `resourcegroup` |
| `RESTLET` | `restlet` |
| `RESTLET_DEPLOYMENT` | `restletdeployment` |
| `RETIREMENT_PLAN` | `retirementplan` |
| `REV_REC_SCHEDULE` | `revrecschedule` |
| `REV_REC_TEMPLATE` | `revrectemplate` |
| `REVENUE_ELEMENT` | `revenueelement` |
| `ROLE` | `role` |
| `SALES_CHANNEL` | `saleschannel` |
| `SALES_ROLE` | `salesrole` |
| `SALES_INVOICED` | `salesinvoiced` |
| `SALES_ORDERED` | `salesordered` |
| `SALES_TAX_ITEM` | `salestaxitem` |
| `SCHEDULED_SCRIPT` | `scheduledscript` |
| `SCHEDULED_SCRIPT_DEPLOYMENT` | `scheduledscriptdeployment` |
| `SCHEDULED_SCRIPT_INSTANCE` | `scheduledscriptinstance` |
| `SCRIPT` | `script` |
| `SCRIPT_CUSTOM_RECORD_TYPE` | `scriptcustomrecordtype` |
| `SCRIPT_DEPLOYMENT` | `scriptdeployment` |
| `SCRIPT_NOTE` | `scriptnote` |
| `SCRIPT_RECORD_TYPE` | `scriptrecordtype` |
| `SEARCH_CAMPAIGN` | `searchcampaign` |
| `SENT_EMAIL` | `sentemail` |
| `SHIPPING_PACKAGE` | `shippingpackage` |
| `SHIPPING_PARTNER_REGISTRATION` | `shippingpartnerregistration` |
| `SHIPPING_PARTNERS_PLUGIN` | `shippingpartnersplugin` |
| `SHIP_ITEM` | `shipitem` |
| `SHOPPING_CART` | `shoppingcart` |
| `SITE_CATEGORY` | `sitecategory` |
| `SITE_THEME` | `sitetheme` |
| `SOLUTION` | `solution` |
| `STANDARD_COST_VERSION` | `standardcostversion` |
| `STATE` | `state` |
| `STATISTICAL_JOURNAL_ENTRY` | `statisticaljournalentry` |
| `STATISTICAL_SCHEDULE` | `statisticalschedule` |
| `STORE_PICKUP_FULFILLMENT` | `storepickupfulfillment` |
| `STORE_TAB` | `storetab` |
| `SUBLIST` | `sublist` |
| `SUBSIDIARY` | `subsidiary` |
| `SUBSIDIARY_SETTINGS` | `subsidiarysettings` |
| `SUITELET` | `suitelet` |
| `SUITELET_DEPLOYMENT` | `suiteletdeployment` |
| `SUITE_SCRIPT_DETAIL` | `suitescriptdetail` |
| `SUPPLY_CHAIN_SNAPSHOT` | `supplychainsnapshot` |
| `SUPPLY_CHAIN_SNAPSHOT_SIMULATION` | `supplychainsnapshotsimulation` |
| `SUPPLY_CHANGE_ORDER` | `supplychangeorder` |
| `SUPPLY_PLAN_DEFINITION` | `supplyplandefinition` |
| `SUPPORT_CASE` | `supportcase` |
| `SUPPORT_CASE_PRIORITY` | `supportcasepriority` |
| `SUPPORT_CASE_STATUS` | `supportcasestatus` |
| `SYSTEM_EMAIL_TEMPLATE` | `systememailtemplate` |
| `SYSTEM_NOTE` | `systemnote` |
| `SYSTEM_NOTE2` | `systemnote2` |
| `SYSTEM_NOTE_FIELD` | `systemnotefield` |
| `SYSTEM_NOTE_TYPE` | `systemnotetype` |
| `TASK` | `task` |
| `TASK_ITEM_STATUS` | `taskitemstatus` |
| `TAX_CALCULATION_PLUGIN` | `taxcalculationplugin` |
| `TAX_ITEM_TAX_GROUP` | `taxitemtaxgroup` |
| `TAX_TYPE` | `taxtype` |
| `TERM` | `term` |
| `TEST_PLUGIN` | `testplugin` |
| `TEXT_ENHANCE_ACTION` | `textenhanceaction` |
| `TIME_BILL` | `timebill` |
| `TIME_MODIFICATION_REQUEST` | `timemodificationrequest` |
| `TIME_SHEET` | `timesheet` |
| `TOPIC` | `topic` |
| `TRACKING_NUMBER` | `trackingnumber` |
| `TRANSACTION` | `transaction` |
| `TRANSACTION_ADDRESSBOOK` | `transactionaddressbook` |
| `TRANSACTION_APPLIED_RULES_LOG` | `transactionappliedruleslog` |
| `TRANSACTION_BILLING` | `transactionbilling` |
| `TRANSACTION_BILLING_ADDRESSBOOK` | `transactionbillingaddressbook` |
| `TRANSACTION_DELETION_REASON` | `transactiondeletionreason` |
| `TRANSACTION_HISTORY` | `transactionhistory` |
| `TRANSACTION_NUMBERING_AUDIT_LOG` | `transactionnumberingauditlog` |
| `TRANSACTION_PAYEE_ADDRESSBOOK` | `transactionpayeeaddressbook` |
| `TRANSACTION_RETURN_ADDRESSBOOK` | `transactionreturnaddressbook` |
| `TRANSACTION_SHIPPING_ADDRESSBOOK` | `transactionshippingaddressbook` |
| `TRANSACTION_STATUS` | `transactionstatus` |
| `U_S_R_SNAPSHOT` | `usrsnapshot` |
| `UMD_FIELD` | `umdfield` |
| `UNDELIVERED_EMAIL` | `undeliveredemail` |
| `UNITS_TYPE` | `unitstype` |
| `UNLOCKED_TIME_PERIOD` | `unlockedtimeperiod` |
| `USER_AUTHORIZATION_CONSENT` | `userauthorizationconsent` |
| `USER_EVENT_SCRIPT` | `usereventscript` |
| `USER_EVENT_SCRIPT_DEPLOYMENT` | `usereventscriptdeployment` |
| `USER_O_AUTH_TOKEN` | `useroauthtoken` |
| `USRSAVEDSEARCH` | `usrsavedsearch` |
| `USR_AUDIT_LOG` | `usrauditlog` |
| `USR_DS_AUDIT_LOG` | `usrdsauditlog` |
| `USR_DS_EXECUTION_LOG` | `usrdsexecutionlog` |
| `USR_EXECUTION_LOG` | `usrexecutionlog` |
| `VENDOR` | `vendor` |
| `VENDOR_CATEGORY` | `vendorcategory` |
| `VENDOR_SUBSIDIARY_RELATIONSHIP` | `vendorsubsidiaryrelationship` |
| `WEBAPP` | `webapp` |
| `WEB_SITE` | `website` |
| `WORKBOOK_BUILDER_PLUGIN` | `workbookbuilderplugin` |
| `WORKFLOW_ACTION_SCRIPT` | `workflowactionscript` |
| `WORKFLOW_ACTION_SCRIPT_DEPLOYMENT` | `workflowactionscriptdeployment` |
| `WORKPLACE` | `workplace` |
| `WORK_CALENDAR` | `workcalendar` |
| `WBS` | `wbs` |
| `ZONE` | `zone` |

> [!SOURCE-ERROR] Four value strings look damaged in the source: `consolidatedexchangerat` [sic] for
> `CONSOLIDATED_EXCHANGE_RATE` seems truncated, `aymentresultpreview` [sic] for
> `PAYMENT_RESULT_PREVIEW` lacks its first letter, `projectemplatesubsidiaryrelationship` [sic] for
> `PROJECT_TEMPLATE_SUBSIDIARY_RELATIONSHIP` lacks a letter, and `oauth22clientcredentials` [sic] for
> `O_AUTH2_CLIENT_CREDENTIALS` has a doubled digit. Use the enum constants rather than these literal
> strings, and verify the strings in your account if you must pass them directly.
<!-- source-error: N/query.Type | CONSOLIDATED_EXCHANGE_RATE | value_string_looks_truncated -->
<!-- source-error: N/query.Type | PAYMENT_RESULT_PREVIEW | value_string_missing_leading_letter -->
<!-- source-error: N/query.Type | PROJECT_TEMPLATE_SUBSIDIARY_RELATIONSHIP | value_string_missing_letter -->
<!-- source-error: N/query.Type | O_AUTH2_CLIENT_CREDENTIALS | value_string_has_doubled_digit -->

> [!SOURCE-ERROR] Two key names look wrong in the source: `ORDER_RELEASE_LIN` [sic] seems truncated
> (its string is `orderreleaseline`), and `GENERIC_RESOURCE_SUBISIDIARY_RELATIONSHIP` [sic] seems
> misspelt (its string spells "subsidiary" correctly). The keys are shown as the source has them;
> check which spelling the enum really exposes before using them, or pass the strings instead.
<!-- source-error: N/query.Type | ORDER_RELEASE_LIN | key_name_looks_truncated -->
<!-- source-error: N/query.Type | GENERIC_RESOURCE_SUBISIDIARY_RELATIONSHIP | key_name_looks_misspelled -->

### query.Operator
<!-- id: N/query.Operator -->

Operators for `Query.createCondition` and `Component.createCondition`. The same operator set is shared
across the SuiteScript analytic APIs, and it applies to [N/dataset](N_dataset.md) and
[N/workbook](N_workbook.md) as well. You can pass the mapped strings in the right-hand column
directly, without loading `N/query`. The include/exclude family treats its list of values with an
implicit AND, while the any-of family treats its list with an implicit OR. The spec's illustration
uses `EXCLUDE_ALL` to drop rows linked to all of a set of web application and web store contexts.
Since 2018.1.

| Value | Mapped string |
|---|---|
| `AFTER` | `AFTER` |
| `AFTER_NOT` | `AFTER_NOT` |
| `ANY_OF` | `ANY_OF` |
| `ANY_OF_NOT` | `ANY_OF_NOT` |
| `BEFORE` | `BEFORE` |
| `BEFORE_NOT` | `BEFORE_NOT` |
| `BETWEEN` | `BETWEEN` |
| `BETWEEN_NOT` | `BETWEEN_NOT` |
| `CONTAIN` | `CONTAIN` |
| `CONTAIN_NOT` | `CONTAIN_NOT` |
| `EMPTY` | `EMPTY` |
| `EMPTY_NOT` | `EMPTY_NOT` |
| `ENDWITH` | `ENDWITH` |
| `ENDWITH_NOT` | `ENDWITH_NOT` |
| `EQUAL` | `EQUAL` |
| `EQUAL_NOT` | `EQUAL_NOT` |
| `EXCLUDE_ALL` | `MN_EXCLUDE` |
| `EXCLUDE_ANY` | `MN_EXCLUDE_ALL` |
| `EXCLUDE_EXACTLY` | `MN_EXCLUDE_EXACTLY` |
| `GREATER` | `GREATER` |
| `GREATER_NOT` | `GREATER_NOT` |
| `GREATER_OR_EQUAL` | `GREATER_OR_EQUAL` |
| `GREATER_OR_EQUAL_NOT` | `GREATER_OR_EQUAL_NOT` |
| `INCLUDE_ALL` | `MN_INCLUDE_ALL` |
| `INCLUDE_ANY` | `MN_INCLUDE` |
| `INCLUDE_EXACTLY` | `MN_INCLUDE_EXACTLY` |
| `IS` | `IS` |
| `IS_NOT` | `IS_NOT` |
| `LESS` | `LESS` |
| `LESS_NOT` | `LESS_NOT` |
| `LESS_OR_EQUAL` | `LESS_OR_EQUAL` |
| `LESS_OR_EQUAL_NOT` | `LESS_OR_EQUAL_NOT` |
| `ON` | `ON` |
| `ON_NOT` | `ON_NOT` |
| `ON_OR_AFTER` | `ON_OR_AFTER` |
| `ON_OR_AFTER_NOT` | `ON_OR_AFTER_NOT` |
| `ON_OR_BEFORE` | `ON_OR_BEFORE` |
| `ON_OR_BEFORE_NOT` | `ON_OR_BEFORE_NOT` |
| `START_WITH` | `START_WITH` |
| `START_WITH_NOT` | `START_WITH_NOT` |
| `WITHIN` | `WITHIN` |
| `WITHIN_NOT` | `WITHIN_NOT` |

> [!SOURCE-ERROR] The mapped strings for `EXCLUDE_ALL` (`MN_EXCLUDE`) and `EXCLUDE_ANY`
> (`MN_EXCLUDE_ALL`) look swapped compared with the include pair (`INCLUDE_ALL` maps to
> `MN_INCLUDE_ALL`). Use the enum constants rather than the raw strings, and test the behaviour if you
> must pass strings.
<!-- source-error: N/query.Operator | - | exclude_and_include_mapped_strings_look_swapped -->

### query.Aggregate
<!-- id: N/query.Aggregate -->

Aggregate functions for [`Column.aggregate`](#columnaggregate) and
[`Condition.aggregate`](#conditionaggregate). Every function except `MEDIAN` comes in two forms: the
plain one uses all instances of a value, and the `_DISTINCT` one counts duplicate values only once. On
dates, the maximum is the most recent date and the minimum is the earliest. Since 2018.1.

> [!SOURCE-ERROR] The source's descriptions of the distinct and non-distinct forms are swapped. Go by
> the names: `_DISTINCT` collapses duplicates, and the plain form keeps them.
<!-- source-error: N/query.Aggregate | - | distinct_and_nondistinct_descriptions_inverted -->

| Value | Meaning |
|---|---|
| `AVERAGE` | Average over all values |
| `AVERAGE_DISTINCT` | Average over distinct values |
| `COUNT` | Count of all values |
| `COUNT_DISTINCT` | Count of distinct values |
| `MAXIMUM` | Largest value (latest date) |
| `MAXIMUM_DISTINCT` | Largest of the distinct values |
| `MEDIAN` | Median (no distinct form) |
| `MINIMUM` | Smallest value (earliest date) |
| `MINIMUM_DISTINCT` | Smallest of the distinct values |
| `SUM` | Sum of all values |
| `SUM_DISTINCT` | Sum of distinct values |

### query.ReturnType
<!-- id: N/query.ReturnType -->

Return types for formulas, used by the `type` option of `createColumn` and `createCondition` (on both
`Query` and `Component`). Since 2018.1.

| Value |
|---|
| `ANY` |
| `BOOLEAN` |
| `CLOBTEXT` |
| `CURRENCY` |
| `DATE` |
| `DATETIME` |
| `DURATION` |
| `FLOAT` |
| `INTEGER` |
| `KEY` |
| `PERCENT` |
| `RELATIONSHIP` |
| `STRING` |
| `UNKNOWN` |

### query.FieldContext
<!-- id: N/query.FieldContext -->

How a column's values are presented, set through [`Column.context`](#columncontext). Since 2019.1. For
example, `HIERARCHY` shows a readable path such as a parent company followed by a subsidiary, while
`HIERARCHY_IDENTIFIER` shows the same path as internal IDs (such as 1 and 5).

| Value | Meaning |
|---|---|
| `CONVERTED` | Amounts converted at the exchange rate of a given date (takes `currencyId` and `date` params) |
| `CURRENCY_CONSOLIDATED` | Amounts consolidated into the base currency |
| `DISPLAY` | User-friendly display values |
| `HIERARCHY` | User-friendly hierarchical values |
| `HIERARCHY_IDENTIFIER` | Raw hierarchical values |
| `RAW` | Raw values such as internal IDs (the default for columns) |
| `SIGN_CONSOLIDATED` | Not described by the source |

> [!SOURCE-ERROR] The source lists `SIGN_CONSOLIDATED` without any description. Its meaning is not
> documented; test its output before using it.
<!-- source-error: N/query.FieldContext | - | sign_consolidated_has_no_description -->

### query.SortLocale
<!-- id: N/query.SortLocale -->

Locale collations for the `locale` option of `Query.createSort` / `Component.createSort` and for
[`Sort.locale`](#sortlocale). The locale must be enabled in the company settings. Since 2018.2.

| Value |
|---|
| `ARABIC` |
| `ARABIC_ABJ_MATCH` |
| `ARABIC_ABJ_MATCH_CI` |
| `ARABIC_ABJ_SORT` |
| `ARABIC_ABJ_SORT_CI` |
| `ARABIC_CI` |
| `ARABIC_MATCH` |
| `ARABIC_MATCH_CI` |
| `ASCII7` |
| `ASCII7_CI` |
| `AZERBAIJANI` |
| `AZERBAIJANI_CI` |
| `BENGALI` |
| `BENGALI_CI` |
| `BIG5` |
| `BIG5_CI` |
| `BINARY` |
| `BINARY_CI` |
| `BULGARIAN` |
| `BULGARIAN_CI` |
| `CANADIAN_M` |
| `CATALAN` |
| `CATALAN_CI` |
| `CROATIAN` |
| `CROATIAN_CI` |
| `CS_CZ` |
| `CZECH` |
| `CZECH_CI` |
| `CZECH_PUNCTUATION` |
| `CZECH_PUNCTUATION_CI` |
| `DANISH` |
| `DANISH_CI` |
| `DANISH_M` |
| `DA_DK` |
| `DE_DE` |
| `DUTCH` |
| `DUTCH_CI` |
| `EBCDIC` |
| `EBCDIC_CI` |
| `EEC_EURO` |
| `EEC_EUROPA3` |
| `EEC_EUROPA3_CI` |
| `EEC_EURO_CI` |
| `EN` |
| `EN_AU` |
| `EN_CA` |
| `EN_GB` |
| `EN_US` |
| `ESTONIAN` |
| `ESTONIAN_CI` |
| `ES_AR` |
| `ES_ES` |
| `FINNISH` |
| `FINNISH_CI` |
| `FI_FI` |
| `FRENCH` |
| `FRENCH_AI` |
| `FRENCH_CI` |
| `FRENCH_M` |
| `FR_CA` |
| `FR_FR` |
| `GBK` |
| `GBK_AI` |
| `GBK_CI` |
| `GENERIC_M` |
| `GERMAN` |
| `GERMAN_AI` |
| `GERMAN_CI` |
| `GERMAN_DIN` |
| `GERMAN_DIN_AI` |
| `GERMAN_DIN_CI` |
| `GREEK` |
| `GREEK_AI` |
| `GREEK_CI` |
| `HEBREW` |
| `HEBREW_AI` |
| `HEBREW_CI` |
| `HE_IL` |
| `HKSCS` |
| `HKSCS_AI` |
| `HKSCS_CI` |
| `HUNGARIAN` |
| `HUNGARIAN_AI` |
| `HUNGARIAN_CI` |
| `ICELANDIC` |
| `ICELANDIC_AI` |
| `ICELANDIC_CI` |
| `ID_ID` |
| `INDONESIAN` |
| `INDONESIAN_AI` |
| `INDONESIAN_CI` |
| `ITALIAN` |
| `ITALIAN_AI` |
| `ITALIAN_CI` |
| `IT_IT` |
| `JAPANESE_M` |
| `JA_JP` |
| `KOREAN_M` |
| `KO_KR` |
| `LATIN` |
| `LATIN_AI` |
| `LATIN_CI` |
| `LATVIAN` |
| `LATVIAN_AI` |
| `LATVIAN_CI` |
| `LITHUANIAN` |
| `LITHUANIAN_AI` |
| `LITHUANIAN_CI` |
| `MALAY` |
| `MALAY_AI` |
| `MALAY_CI` |
| `NL_NL` |
| `NO_NO` |
| `NORWEGIAN` |
| `NORWEGIAN_AI` |
| `NORWEGIAN_CI` |
| `POLISH` |
| `POLISH_AI` |
| `POLISH_CI` |
| `PT_BR` |
| `PUNCTUATION` |
| `PUNCTUATION_AI` |
| `PUNCTUATION_CI` |
| `ROMANIAN` |
| `ROMANIAN_AI` |
| `ROMANIAN_CI` |
| `RUSSIAN` |
| `RUSSIAN_AI` |
| `RUSSIAN_CI` |
| `RU_RU` |
| `SCHINESE_PINYIN_M` |
| `SCHINESE_RADICAL_M` |
| `SCHINESE_STROKE_M` |
| `SLOVAK` |
| `SLOVAK_AI` |
| `SLOVAK_CI` |
| `SLOVENIAN` |
| `SLOVENIAN_AI` |
| `SLOVENIAN_CI` |
| `SPANISH` |
| `SPANISH_AI` |
| `SPANISH_CI` |
| `SPANISH_M` |
| `SV_SE` |
| `SWEDISH` |
| `SWEDISH_AI` |
| `SWEDISH_CI` |
| `SWISS` |
| `SWISS_AI` |
| `SWISS_CI` |
| `TCHINESE_RADICAL_M` |
| `TCHINESE_STROKE_M` |
| `THAI_M` |
| `TH_TH` |
| `TR_TR` |
| `TURKISH` |
| `TURKISH_AI` |
| `TURKISH_CI` |
| `UKRAINIAN` |
| `UKRAINIAN_AI` |
| `UKRAINIAN_CI` |
| `UNICODE_BINARY` |
| `UNICODE_BINARY_AI` |
| `UNICODE_BINARY_CI` |
| `VIETNAMESE` |
| `VIETNAMESE_AI` |
| `VIETNAMESE_CI` |
| `VI_VN` |
| `WEST_EUROPEAN` |
| `WEST_EUROPEAN_AI` |
| `WEST_EUROPEAN_CI` |
| `ZH_CN` |
| `ZH_TW` |

### query.DateId
<!-- id: N/query.DateId -->

The unit and direction for [`query.createRelativeDate`](#querycreaterelativedateoptions), also
reported by [`RelativeDate.dateId`](#relativedatedateid). The code in the right-hand column is the
value that appears in the `dateId` property. The difference from `RelativeDateRange`: a `DateId` is
combined with a number to build a relative date, while a `RelativeDateRange` value is a ready-made
range you use directly. Since 2019.1.

| Value | Code |
|---|---|
| `DAYS_AGO` | `dago` |
| `DAYS_FROM_NOW` | `dfn` |
| `HOURS_AGO` | `hago` |
| `HOURS_FROM_NOW` | `hfn` |
| `MINUTES_AGO` | `nago` |
| `MINUTES_FROM_NOW` | `nfn` |
| `MONTHS_AGO` | `mago` |
| `MONTHS_FROM_NOW` | `mfn` |
| `QUARTERS_AGO` | `qago` |
| `QUARTERS_FROM_NOW` | `qfn` |
| `SECONDS_AGO` | `sago` |
| `SECONDS_FROM_NOW` | `sfn` |
| `WEEKS_AGO` | `wago` |
| `WEEKS_FROM_NOW` | `wfn` |
| `YEARS_AGO` | `yago` |
| `YEARS_FROM_NOW` | `yfn` |

### query.RelativeDateRange
<!-- id: N/query.RelativeDateRange -->

Ready-made date ranges relative to today, used as condition values with `Query.createCondition`. Each
value is a [`RelativeDate`](#relativedate-object) range whose `dateId` is a short code such as `LM`,
`LYTD` or `TODAY`. How the range compares depends on the operator: `TODAY` with `BEFORE` matches dates
in the past, and `LAST_YEAR` with `AFTER` matches dates after last year. Since 2019.1.

> [!SOURCE-ERROR] The source lists `RelativeDate.value` among the places this enum is used, yet that
> property is `undefined` on range values. Use these values as condition values; do not expect a
> number in `RelativeDate.value` for them.
<!-- source-error: N/query.RelativeDateRange | - | used_as_relativedate_value_vs_value_undefined_for_ranges -->

| Value | Code |
|---|---|
| `FISCAL_HALF_BEFORE_LAST` | `FHBL` |
| `FISCAL_HALF_BEFORE_LAST_TO_DATE` | `FHBLTD` |
| `FISCAL_QUARTER_BEFORE_LAST` | `FQBL` |
| `FISCAL_QUARTER_BEFORE_LAST_TO_DATE` | `FQBLTD` |
| `FISCAL_YEAR_BEFORE_LAST` | `FYBL` |
| `FISCAL_YEAR_BEFORE_LAST_TO_DATE` | `FYBLTD` |
| `FIVE_DAYS_AGO` | `DAGO5` |
| `FIVE_DAYS_FROM_NOW` | `DFN5` |
| `FOUR_DAYS_AGO` | `DAGO4` |
| `FOUR_DAYS_FROM_NOW` | `DFN4` |
| `FOUR_WEEKS_STARTING_THIS_WEEK` | `TWN3W` |
| `LAST_BUSINESS_WEEK` | `LBW` |
| `LAST_FISCAL_HALF` | `LFH` |
| `LAST_FISCAL_HALF_ONE_FISCAL_YEAR_AGO` | `LFHLFY` |
| `LAST_FISCAL_HALF_TO_DATE` | `LFHTD` |
| `LAST_FISCAL_QUARTER` | `LFQ` |
| `LAST_FISCAL_QUARTER_ONE_FISCAL_YEAR_AGO` | `LFQLFY` |
| `LAST_FISCAL_QUARTER_TO_DATE` | `LFQTD` |
| `LAST_FISCAL_QUARTER_TWO_FISCAL_YEARS_AGO` | `LFQFYBL` |
| `LAST_FISCAL_YEAR` | `LFY` |
| `LAST_FISCAL_YEAR_TO_DATE` | `LFYTD` |
| `LAST_MONTH` | `LM` |
| `LAST_MONTH_ONE_FISCAL_QUARTER_AGO` | `LMLFQ` |
| `LAST_MONTH_ONE_FISCAL_YEAR_AGO` | `LMLFY` |
| `LAST_MONTH_TO_DATE` | `LMTD` |
| `LAST_MONTH_TWO_FISCAL_QUARTERS_AGO` | `LMFQBL` |
| `LAST_MONTH_TWO_FISCAL_YEARS_AGO` | `LMFYBL` |
| `LAST_ROLLING_HALF` | `LRH` |
| `LAST_ROLLING_QUARTER` | `LRQ` |
| `LAST_ROLLING_YEAR` | `LRY` |
| `LAST_WEEK` | `LW` |
| `LAST_WEEK_TO_DATE` | `LWTD` |
| `LAST_YEAR` | `LY` |
| `LAST_YEAR_TO_DATE` | `LYTD` |
| `MONTH_AFTER_NEXT` | `MAN` |
| `MONTH_AFTER_NEXT_TO_DATE` | `MANTD` |
| `MONTH_BEFORE_LAST` | `MBL` |
| `MONTH_BEFORE_LAST_TO_DATE` | `MBLTD` |
| `NEXT_BUSINESS_WEEK` | `NBW` |
| `NEXT_FISCAL_HALF` | `NFH` |
| `NEXT_FISCAL_QUARTER` | `NFQ` |
| `NEXT_FISCAL_YEAR` | `NFY` |
| `NEXT_FOUR_WEEKS` | `N4W` |
| `NEXT_MONTH` | `NM` |
| `NEXT_ONE_HALF` | `NOH` |
| `NEXT_ONE_MONTH` | `NOM` |
| `NEXT_ONE_QUARTER` | `NOQ` |
| `NEXT_ONE_WEEK` | `NOW` |
| `NEXT_ONE_YEAR` | `NOY` |
| `NEXT_WEEK` | `NW` |
| `NINETY_DAYS_AGO` | `DAGO90` |
| `NINETY_DAYS_FROM_NOW` | `DFN90` |
| `ONE_YEAR_BEFORE_LAST` | `OYBL` |
| `PREVIOUS_FISCAL_QUARTERS_LAST_FISCAL_YEAR` | `PQLFY` |
| `PREVIOUS_FISCAL_QUARTERS_THIS_FISCAL_YEAR` | `PQTFY` |
| `PREVIOUS_MONTHS_LAST_FISCAL_HALF` | `PMLFH` |
| `PREVIOUS_MONTHS_LAST_FISCAL_QUARTER` | `PMLFQ` |
| `PREVIOUS_MONTHS_LAST_FISCAL_YEAR` | `PMLFY` |
| `PREVIOUS_MONTHS_SAME_FISCAL_HALF_LAST_FISCAL_YEAR` | `PMSFHLFY` |
| `PREVIOUS_MONTHS_SAME_FISCAL_QUARTER_LAST_FISCAL_YEAR` | `PMSFQLFY` |
| `PREVIOUS_MONTHS_THIS_FISCAL_HALF` | `PMTFH` |
| `PREVIOUS_MONTHS_THIS_FISCAL_QUARTER` | `PMTFQ` |
| `PREVIOUS_MONTHS_THIS_FISCAL_YEAR` | `PMTFY` |
| `PREVIOUS_ONE_DAY` | `OD` |
| `PREVIOUS_ONE_HALF` | `OH` |
| `PREVIOUS_ONE_MONTH` | `OM` |
| `PREVIOUS_ONE_QUARTER` | `OQ` |
| `PREVIOUS_ONE_WEEK` | `OW` |
| `PREVIOUS_ONE_YEAR` | `OY` |
| `PREVIOUS_ROLLING_HALF` | `PRH` |
| `PREVIOUS_ROLLING_QUARTER` | `PRQ` |
| `PREVIOUS_ROLLING_YEAR` | `PRY` |
| `SAME_DAY_FISCAL_QUARTER_BEFORE_LAST` | `SDFQBL` |
| `SAME_DAY_FISCAL_YEAR_BEFORE_LAST` | `SDFYBL` |
| `SAME_DAY_LAST_FISCAL_QUARTER` | `SDLFQ` |
| `SAME_DAY_LAST_FISCAL_YEAR` | `SDLFY` |
| `SAME_DAY_LAST_MONTH` | `SDLM` |
| `SAME_DAY_LAST_WEEK` | `SDLW` |
| `SAME_DAY_MONTH_BEFORE_LAST` | `SDMBL` |
| `SAME_DAY_WEEK_BEFORE_LAST` | `SDWBL` |
| `SAME_FISCAL_HALF_LAST_FISCAL_YEAR` | `SFHLFY` |
| `SAME_FISCAL_HALF_LAST_FISCAL_YEAR_TO_DATE` | `SFHLFYTD` |
| `SAME_FISCAL_QUARTER_FISCAL_YEAR_BEFORE_LAST` | `SFQFYBL` |
| `SAME_FISCAL_QUARTER_LAST_FISCAL_YEAR` | `SFQLFY` |
| `SAME_FISCAL_QUARTER_LAST_FISCAL_YEAR_TO_DATE` | `SFQLFYTD` |
| `SAME_MONTH_FISCAL_QUARTER_BEFORE_LAST` | `SMFQBL` |
| `SAME_MONTH_FISCAL_YEAR_BEFORE_LAST` | `SMFYBL` |
| `SAME_MONTH_LAST_FISCAL_QUARTER` | `SMLFQ` |
| `SAME_MONTH_LAST_FISCAL_QUARTER_TO_DATE` | `SMLFQTD` |
| `SAME_MONTH_LAST_FISCAL_YEAR` | `SMLFY` |
| `SAME_MONTH_LAST_FISCAL_YEAR_TO_DATE` | `SMLFYTD` |
| `SAME_WEEK_FISCAL_YEAR_BEFORE_LAST` | `SWFYBL` |
| `SAME_WEEK_LAST_FISCAL_YEAR` | `SWLFY` |
| `SIXTY_DAYS_AGO` | `DAGO60` |
| `SIXTY_DAYS_FROM_NOW` | `DFN60` |
| `TEN_DAYS_AGO` | `DAGO10` |
| `TEN_DAYS_FROM_NOW` | `DFN10` |
| `THIRTY_DAYS_AGO` | `DAGO30` |
| `THIRTY_DAYS_FROM_NOW` | `DFN30` |
| `THIS_BUSINESS_WEEK` | `TBW` |
| `THIS_FISCAL_HALF` | `TFH` |
| `THIS_FISCAL_HALF_TO_DATE` | `TFHTD` |
| `THIS_FISCAL_QUARTER` | `TFQ` |
| `THIS_FISCAL_QUARTER_TO_DATE` | `TFQTD` |
| `THIS_FISCAL_YEAR` | `TFY` |
| `THIS_FISCAL_YEAR_TO_DATE` | `TFYTD` |
| `THIS_MONTH` | `TM` |
| `THIS_MONTH_TO_DATE` | `TMTD` |
| `THIS_ROLLING_HALF` | `TRH` |
| `THIS_ROLLING_QUARTER` | `TRQ` |
| `THIS_ROLLING_YEAR` | `TRY` |
| `THIS_WEEK` | `TW` |
| `THIS_WEEK_TO_DATE` | `TWTD` |
| `THIS_YEAR` | `TY` |
| `THIS_YEAR_TO_DATE` | `TYTD` |
| `THREE_DAYS_AGO` | `DAGO3` |
| `THREE_DAYS_FROM_NOW` | `DFN3` |
| `THREE_FISCAL_QUARTERS_AGO` | `FQB` |
| `THREE_FISCAL_QUARTERS_AGO_TO_DATE` | `FQBTD` |
| `THREE_FISCAL_YEARS_AGO` | `FYB` |
| `THREE_FISCAL_YEARS_AGO_TO_DATE` | `FYBTD` |
| `THREE_MONTHS_AGO` | `MB` |
| `THREE_MONTHS_AGO_TO_DATE` | `MBTD` |
| `TODAY` | `TODAY` |
| `TODAY_TO_END_OF_THIS_MONTH` | `TODAYTTM` |
| `TOMORROW` | `TOMORROW` |
| `TWO_DAYS_AGO` | `DAGO2` |
| `TWO_DAYS_FROM_NOW` | `DFN2` |
| `WEEK_AFTER_NEXT` | `WAN` |
| `WEEK_AFTER_NEXT_TO_DATE` | `WANTD` |
| `WEEK_BEFORE_LAST` | `WBL` |
| `WEEK_BEFORE_LAST_TO_DATE` | `WBLTD` |
| `YESTERDAY` | `YESTERDAY` |

### query.PeriodCode
<!-- id: N/query.PeriodCode -->

Period codes for the `code` option of [`query.createPeriod`](#querycreateperiodoptions). Since 2020.1.

| Value | Code |
|---|---|
| `FIRST_FISCAL_QUARTER_LAST_FY` | `Q1LFY` |
| `FIRST_FISCAL_QUARTER_THIS_FY` | `Q1TFY` |
| `FISCAL_QUARTER_BEFORE_LAST` | `QBL` |
| `FISCAL_YEAR_BEFORE_LAST` | `FYBL` |
| `FOURTH_FISCAL_QUARTER_LAST_FY` | `Q4LFY` |
| `FOURTH_FISCAL_QUARTER_THIS_FY` | `Q4TFY` |
| `LAST_FISCAL_QUARTER` | `LQ` |
| `LAST_FISCAL_QUARTER_ONE_FISCAL_YEAR_AGO` | `LQOLFY` |
| `LAST_FISCAL_QUARTER_TO_PERIOD` | `LFQTP` |
| `LAST_FISCAL_YEAR` | `LFY` |
| `LAST_FISCAL_YEAR_TO_PERIOD` | `LFYTP` |
| `LAST_PERIOD` | `LP` |
| `LAST_PERIOD_ONE_FISCAL_QUARTER_AGO` | `LPOLQ` |
| `LAST_PERIOD_ONE_FISCAL_YEAR_AGO` | `LPOLFY` |
| `LAST_ROLLING_18_PERIODS` | `LR18FP` |
| `LAST_ROLLING_6_FISCAL_QUARTERS` | `LR6FQ` |
| `PERIOD_BEFORE_LAST` | `PBL` |
| `SAME_FISCAL_QUARTER_LAST_FY` | `TQOLFY` |
| `SAME_FISCAL_QUARTER_LAST_FY_TO_PERIOD` | `TFQOLFYTP` |
| `SAME_PERIOD_LAST_FY` | `TPOLFY` |
| `SAME_PERIOD_LAST_FISCAL_QUARTER` | `TPOLQ` |
| `SECOND_FISCAL_QUARTER_LAST_FY` | `Q2LFY` |
| `SECOND_FISCAL_QUARTER_THIS_FY` | `Q2TFY` |
| `THIRD_FISCAL_QUARTER_LAST_FY` | `Q3LFY` |
| `THIRD_FISCAL_QUARTER_THIS_FY` | `Q3TFY` |
| `THIS_FISCAL_QUARTER` | `TQ` |
| `THIS_FISCAL_QUARTER_TO_PERIOD` | `TFQTP` |
| `THIS_FISCAL_YEAR` | `TFY` |
| `THIS_FISCAL_YEAR_TO_PERIOD` | `TFYTP` |
| `THIS_PERIOD` | `TP` |

### query.PeriodAdjustment
<!-- id: N/query.PeriodAdjustment -->

Values for the `adjustment` option of [`query.createPeriod`](#querycreateperiodoptions) (default
`NOT_LAST`) and for [`Period.adjustment`](#periodadjustment). The source does not state what either
value does to the period. Since 2020.1.

| Value |
|---|
| `ALL` |
| `NOT_LAST` |

### query.PeriodType
<!-- id: N/query.PeriodType -->

Values for the `type` option of [`query.createPeriod`](#querycreateperiodoptions) (default `START`)
and for [`Period.type`](#periodtype). The source does not state what the two values mean beyond their
names. Since 2020.1.

| Value |
|---|
| `END` |
| `START` |

## Examples

The field IDs in these examples (`type`, `trandate`, `entity`, `category`, `upccode`, ...) and the
SuiteQL table and column names are illustrative. Query and SuiteQL names come from the analytics data
source, so confirm each one in the [Records Catalog](../topics/records_catalog.md) for your account.

### Query definition with a join, a combined condition and paged results

A scheduled script that reports invoices from the last 30 days for customers in two categories. It
joins from the transaction to its customer, puts a condition on each side, sorts on the unique `id`
(required for stable paging) and reads the rows page by page.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const WATCHED_CATEGORIES = [3, 7];

  const execute = () => {
    const invoices = query.create({ type: query.Type.TRANSACTION });
    const customer = invoices.autoJoin({ fieldId: 'entity' });

    const isInvoice = invoices.createCondition({
      fieldId: 'type',
      operator: query.Operator.ANY_OF,
      values: ['CustInvc']
    });
    const isRecent = invoices.createCondition({
      fieldId: 'trandate',
      operator: query.Operator.ON_OR_AFTER,
      values: [query.createRelativeDate({ dateId: query.DateId.DAYS_AGO, value: 30 })]
    });
    const inWatchedCategory = customer.createCondition({
      fieldId: 'category',
      operator: query.Operator.ANY_OF,
      values: WATCHED_CATEGORIES
    });
    invoices.condition = invoices.and(isInvoice, isRecent, inWatchedCategory);

    const idColumn = invoices.createColumn({ fieldId: 'id' });
    invoices.columns = [
      idColumn,
      invoices.createColumn({ fieldId: 'tranid' }),
      customer.createColumn({ fieldId: 'companyname' }),
      invoices.createColumn({ fieldId: 'foreigntotal' })
    ];
    invoices.sort = [invoices.createSort({ column: idColumn, ascending: true })];

    // pageSize is passed as a number; see the note under Query.runPaged.
    const paged = invoices.runPaged({ pageSize: 250 });
    log.audit({ title: 'Recent invoices', details: `${paged.count} rows` });

    paged.pageRanges.forEach((range) => {
      const page = paged.fetch({ index: range.index });
      page.data.results.forEach((row) => {
        const [id, tranId, company, total] = row.values;
        log.debug({ title: `Invoice ${tranId}`, details: `${company}: ${total} (internal id ${id})` });
      });
    });
  };

  return { execute };
});
```

### SuiteQL with bound parameters

A RESTlet that returns vendors with large bills. The two `?` placeholders are filled from `params` in
order, so request input never gets spliced into the SQL text. The join is written in Oracle style,
the syntax the [SuiteQL topic](../topics/suiteql.md) recommends. `asMappedResults()` gives keys in
lower case.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/query'], (query) => {
  const LARGE_BILLS_SQL = `
    SELECT v.id, v.companyname, COUNT(t.id) AS billcount, SUM(t.foreigntotal) AS billtotal
    FROM transaction t, vendor v
    WHERE t.entity = v.id
      AND t.type = ?
      AND t.foreigntotal >= ?
    GROUP BY v.id, v.companyname`;

  const get = (requestParams) => {
    const threshold = Number(requestParams.minAmount) || 10000;
    const rows = query.runSuiteQL({
      query: LARGE_BILLS_SQL,
      params: ['VendBill', threshold],
      customScriptId: 'restlet_large_vendor_bills'
    }).asMappedResults();

    return rows.map((row) => ({
      vendorId: row.id,
      vendor: row.companyname,
      bills: row.billcount,
      total: row.billtotal
    }));
  };

  return { get };
});
```

### From a query definition to paged SuiteQL

Items with no UPC code, found with an `EMPTY` condition (which needs no `values`). The definition is
converted with `toSuiteQL()` and paged through `SuiteQL.runPaged`, which keeps the column
definitions. The sort on `id` gives the unique ordering that paging needs.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const execute = () => {
    const items = query.create({ type: query.Type.ITEM });
    const idColumn = items.createColumn({ fieldId: 'id' });
    items.columns = [idColumn, items.createColumn({ fieldId: 'itemid' })];
    items.condition = items.createCondition({
      fieldId: 'upccode',
      operator: query.Operator.EMPTY
    });
    items.sort = [items.createSort({ column: idColumn })];

    const statement = items.toSuiteQL();
    log.debug({ title: 'Generated SuiteQL', details: statement.query });

    const paged = statement.runPaged({ pageSize: 500 });
    paged.pageRanges.forEach((range) => {
      paged.fetch({ index: range.index }).data.asMappedResults().forEach((row) => {
        log.audit({ title: 'Item missing UPC', details: `${row.itemid} (id ${row.id})` });
      });
    });
  };

  return { execute };
});
```
