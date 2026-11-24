# N/search
<!-- id: N/search -->

`N/search` is the module a script uses to pull rows of data out of NetSuite. You describe a search
(its type, filters, result columns and optional settings), run it, and read the rows that come back.
The same module loads, saves and deletes saved searches, reads a few body fields of a single record
without loading it (`lookupFields`), runs the scripted equivalent of the global search box, asks the
account's duplicate-detection rules for likely duplicates, and splits large result sets into pages.

The jobs it is meant for: finding a single record by keyword, building and saving searches, loading
and running saved searches that already exist, finding duplicate records, listing the records that
match a set of filters, and paging through large result sets with page-by-page navigation.

- **Availability:** client and server scripts, but not in client-side contexts where no user is
  authenticated.
- **Permissions:** depending on the operation, the role needs permission to perform searches, to
  persist (save) them and to publish them.
- Saved-search script IDs start with `customsearch`.
- The promise variants of `Page.next`, `Page.prev`, `PagedData.fetch` and the `ResultSet` methods can
  only be used in client scripts.
- `Column.label` reads and writes custom labels only.
- **Load it as:** `define(['N/search'], (search) => { ... })`
- Related: [Saved searches](../topics/saved_searches.md), [Search formulas](../topics/search_formulas.md),
  [search formula functions](../gen/catalogs/search_formula_functions.md),
  [search column functions](../gen/catalogs/search_column_functions.md),
  [operators by field type](../gen/catalogs/search_operator_field_types.md),
  [standalone search types](../gen/catalogs/standalone_search_types.md),
  [search date filters](../gen/catalogs/search_date_filters.md),
  [record search filters, columns and joins](../gen/records/index.md), [N/record](N_record.md),
  [N/format](N_format.md), [N/runtime](N_runtime.md).

> [!SOURCE-ERROR] In the vendor source the descriptions of `search.delete` and `search.delete.promise`
> appear to have been swapped. Rely on the signatures on this page: the plain function returns nothing,
> the promise variant returns a Promise.
<!-- source-error: N/search | - | delete_and_delete_promise_descriptions_swapped -->

> [!SOURCE-ERROR] For `Result.getText`, the source's text for the `column` form speaks of stored select
> fields while its text for the `options` form speaks of non-stored ones, and the two do not agree. The
> spec does not say which kinds of select field each form really supports; test it on your field.
<!-- source-error: N/search | - | gettext_column_says_stored_select_options_non_stored -->

## Members

| Member | Kind | Summary |
|---|---|---|
| [`search.create(options)`](#searchcreateoptions) | function | Define a new search in memory |
| [`search.create.promise(options)`](#searchcreatepromiseoptions) | function | Promise form of `create` |
| [`search.load(options)`](#searchloadoptions) | function | Load an existing saved search |
| [`search.load.promise(options)`](#searchloadpromiseoptions) | function | Promise form of `load` |
| [`search.delete(options)`](#searchdeleteoptions) | function | Delete a saved search |
| [`search.delete.promise(options)`](#searchdeletepromiseoptions) | function | Promise form of `delete` |
| [`search.createColumn(options)`](#searchcreatecolumnoptions) | function | Build a result column |
| [`search.createFilter(options)`](#searchcreatefilteroptions) | function | Build a filter condition |
| [`search.createSetting(options)`](#searchcreatesettingoptions) | function | Build a search setting |
| [`search.lookupFields(options)`](#searchlookupfieldsoptions) | function | Read body fields of one record |
| [`search.lookupFields.promise(options)`](#searchlookupfieldspromiseoptions) | function | Promise form of `lookupFields` |
| [`search.duplicates(options)`](#searchduplicatesoptions) | function | Find likely duplicate entity records |
| [`search.duplicates.promise(options)`](#searchduplicatespromiseoptions) | function | Promise form of `duplicates` |
| [`search.global(options)`](#searchglobaloptions) | function | Keyword (global) search |
| [`search.global.promise(options)`](#searchglobalpromiseoptions) | function | Promise form of `global` |
| [`Search`](#search-object) | object | A search definition you can run or save |
| [`ResultSet`](#resultset-object) | object | Rows returned by `Search.run()` |
| [`Result`](#result-object) | object | One result row |
| [`Column`](#column-object) | object | One result column |
| [`Filter`](#filter-object) | object | One filter condition |
| [`Setting`](#setting-object) | object | One search setting |
| [`PagedData`](#pageddata-object) | object | Summary of a paged run |
| [`Page`](#page-object) | object | One page of a paged run |
| [`PageRange`](#pagerange-object) | object | Bounds of one page |
| [`search.Type`](#searchtype) | enum | Search types |
| [`search.Operator`](#searchoperator) | enum | Filter operators |
| [`search.Summary`](#searchsummary) | enum | Summary (grouping and aggregate) types |
| [`search.Sort`](#searchsort) | enum | Column sort direction |

## Functions

### search.create(options)
<!-- id: N/search.create -->

Defines a new search and returns it as a [`Search`](#search-object) object. Nothing is stored at this
point: you can run the definition straight away with `Search.run()` (no save needed), or persist it
with `Search.save()`. A saved search can be loaded again later with `search.load`, and it is also
usable from the UI like any other saved search.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | search.Type | Yes | |
| `options.filters` | search.Filter \| search.Filter[] \| Object \| Object[] \| string \| string[] | No | |
| `options.filterExpression` | Object[] | No | |
| `options.columns` | search.Column \| search.Column[] \| Object \| Object[] \| string \| string[] | No | |
| `options.settings` | search.Setting \| search.Setting[] \| Object \| Object[] \| string \| string[] | No | |
| `options.title` | string | No | |
| `options.id` | string | No | |
| `options.isPublic` | boolean | No | `false` |
| `options.packageId` | string | No | |

- `options.type` (since 2015.2): a [`search.Type`](#searchtype) value.
- `options.filters` (since 2015.2): one `Filter`, an array of them, one filter expression, or an array
  of expressions. Written as a plain object, a filter has `name` and `operator` (both mandatory) and may
  have `join`, `summary`, `formula` and `values`. A filter value of the wrong type is ignored instead of
  rejected; in a server script that also produces a log entry. You can add conditions later through
  [`Search.filters`](#searchfilters) or [`Search.filterExpression`](#searchfilterexpression).
- `options.filterExpression` (since 2016.2): replaces any filter expression set before. The
  recommended place for a filter expression is still `options.filters`, which recognizes expressions on
  its own.
- `options.columns` (since 2015.2): `Column` objects, plain objects or column-name strings, single or
  in arrays. A plain column object has `name` (mandatory) and may have `formula`, `function`, `join`,
  `label`, `sort` and `summary`.
- `options.settings` (since 2018.2): search parameters otherwise reachable only in the UI. As plain
  objects they carry `name` and `value`; see [`search.createSetting`](#searchcreatesettingoptions).
- `options.title` (since 2015.2): must be set before the search can be saved.
- `options.id` (since 2015.2): the saved search's script ID; generated for you if you leave it out.
- `options.isPublic` (since 2016.2): sets [`Search.isPublic`](#searchispublic).
- `options.packageId` (since 2019.2): the application ID of the SuiteApp the search belongs to.

**Returns:** `search.Search`

**Errors:**

- `SSS_INVALID_SRCH_COL`: a value in `options.columns` is invalid.
- `SSS_INVALID_SRCH_FILTER_EXPR`: a value in `options.filters` is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

Points to keep in mind while building the definition:

- **Sort for a stable order.** A single sort on a field with few distinct values gives a row order that
  can change between runs. Sort on a field with many distinct values, or on several fields.
- **List/record fields work by internal ID.** Filters and columns on list/record fields must use the
  internal ID, not the display text. When you need the text, use a formula column or filter named
  `formulatext` (see [Search formulas](../topics/search_formulas.md)).

### search.create.promise(options)
<!-- id: N/search.create.promise -->

The asynchronous form of [`search.create`](#searchcreateoptions). It takes the same parameters and
raises the same errors, but returns a Promise instead of the `Search` object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | search.Type | Yes | |
| `options.filters` | search.Filter \| search.Filter[] \| Object \| Object[] \| string \| string[] | No | |
| `options.filterExpression` | Object[] | No | |
| `options.columns` | search.Column \| search.Column[] \| Object \| Object[] \| string \| string[] | No | |
| `options.settings` | search.Setting \| search.Setting[] \| Object \| Object[] \| string \| string[] | No | |
| `options.title` | string | No | |
| `options.id` | string | No | |
| `options.isPublic` | boolean | No | `false` |
| `options.packageId` | string | No | |

**Returns:** `Promise`

**Errors:** the same as `search.create`.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.load(options)
<!-- id: N/search.load -->

Loads an existing saved search, whether it was built in the UI or saved by a script, and returns it as a
`Search` object that you can run, change or save again.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.type` | string | No | |

- `options.id` (since 2015.2): the saved search's internal ID, or its script ID (the one beginning with
  `customsearch`).
- `options.type` (since 2015.2): conditionally required. Normally the type is worked out from the
  search's record type and you leave it out. For a standalone search type, meaning a search type with
  no record type behind it (see the [standalone search types](../gen/catalogs/standalone_search_types.md)),
  you must pass it. Values come from [`search.Type`](#searchtype).

**Returns:** `search.Search`

**Errors:**

- `INVALID_SEARCH`: no saved search matches `options.id`.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.load.promise(options)
<!-- id: N/search.load.promise -->

The asynchronous form of [`search.load`](#searchloadoptions): same parameters and errors, returns a
Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.type` | string | No | |

**Returns:** `Promise`

**Errors:** the same as `search.load`.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.delete(options)
<!-- id: N/search.delete -->

Deletes a saved search, whether it was created in the UI or by a script. See the note at the top of the
page about the swapped descriptions in the source.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string \| number | Yes | |
| `options.type` | string | No | |

- `options.id` (since 2015.2): the internal ID or the `customsearch...` script ID of the saved search.
- `options.type` (since 2015.2): conditionally required, namely for standalone search types (see the
  [standalone search types](../gen/catalogs/standalone_search_types.md)). Deleting a standalone search
  without it fails with an error. Values come from [`search.Type`](#searchtype).

**Returns:** `void`

**Errors:**

- `INVALID_SEARCH`: no saved search matches `options.id`.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.delete.promise(options)
<!-- id: N/search.delete.promise -->

The asynchronous form of [`search.delete`](#searchdeleteoptions): same parameters and errors, returns a
Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string \| number | Yes | |
| `options.type` | string | No | |

**Returns:** `Promise`

> [!SOURCE-ERROR] The source says this variant returns a `search.Search` object, yet its declared return
> type is a Promise, as with every other promise variant. Code against a Promise.
<!-- source-error: N/search.delete.promise | - | says_returns_search_object_but_returns_promise -->

**Errors:** the same as `search.delete`.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.createColumn(options)
<!-- id: N/search.createColumn -->

Creates a [`Column`](#column-object). A column stands for one record field you want included in the
results. Pass it in `options.columns` of `search.create`, or add it to a loaded search. A typical use is
a column on a customer search that shows the customer's sales rep.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.summary` | string | No | |
| `options.function` | string | No | |
| `options.sort` | string | No | |
| `options.name` | string | Yes | |
| `options.join` | string | No | |
| `options.formula` | string | No | |
| `options.label` | string | No | |

- `options.summary`: a [`search.Summary`](#searchsummary) value.
- `options.function`: one of the IDs in the
  [search column functions](../gen/catalogs/search_column_functions.md).
- `options.sort`: a [`search.Sort`](#searchsort) value. It cannot be changed once the column exists,
  and if you reuse the same column object in another search, it keeps the sort it was created with.
- `options.formula`: for formula column names; see [`Column.formula`](#columnformula).
- All options exist since 2015.2.

**Returns:** `search.Column`

**Errors:**

- `SSS_INVALID_SRCH_COLUMN_SUM`: `options.summary` is not a valid summary type.
- `INVALID_SRCH_FUNCTN`: the function in `options.function` is unknown.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

A list/record field column gives you the internal ID, not the text. When you need the text, use a
`formulatext` column instead.

### search.createFilter(options)
<!-- id: N/search.createFilter -->

Creates a [`Filter`](#filter-object): one condition that every returned row must meet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.operator` | string | Yes | |
| `options.summary` | string | No | |
| `options.name` | string | Yes | |
| `options.join` | string | No | |
| `options.values` | string \| Date \| number \| boolean \| string[] \| Date[] \| number[] | No | |
| `options.formula` | string | No | |

- `options.operator`: a [`search.Operator`](#searchoperator) value.
- `options.summary`: a [`search.Summary`](#searchsummary) value.
- All options exist since 2015.2.

**Returns:** `search.Filter`

**Errors:**

- `SSS_INVALID_SRCH_OPERATOR`: `options.operator` is not a valid operator.
- `SSS_INVALID_SRCH_SUMMARY_TYP`: `options.summary` is not a valid summary type.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

- Filters on list/record fields compare internal IDs, not text; use a `formulatext` filter to match on
  the text.
- On long text and rich text fields, a "contains" style match only looks at roughly the first 500
  characters (the figure is approximate).

### search.createSetting(options)
<!-- id: N/search.createSetting -->

Creates a [`Setting`](#setting-object): a search parameter that you would otherwise only be able to set
in the UI. Assign the result to [`Search.settings`](#searchsettings), or pass it in `options.settings`
of `search.create`. Settings apply to transaction searches in OneWorld accounts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.value` | string | Yes | |

- `options.name` (since 2018.2): sets [`Setting.name`](#settingname). Allowed values:
  `consolidationtype` or `includeperiodendtransactions`.
- `options.value` (since 2018.2): sets [`Setting.value`](#settingvalue); not case-sensitive.
  - With `consolidationtype`: `ACCTTYPE`, `AVERAGE`, `CURRENT`, `HISTORICAL` or `NONE`. When you do not
    set it, `ACCTTYPE` applies.
  - With `includeperiodendtransactions`: `F`, `FALSE`, `T` or `TRUE`. When you do not set it, false
    applies.
  - The source also says that for a joined search the value is a join ID.

> [!SOURCE-ERROR] The remark that the value is a join ID for joined searches does not fit the fixed
> value lists given for both setting names, and the spec marks it for human review. Use the listed
> values; treat the join-ID remark as unconfirmed.
<!-- source-error: N/search.createSetting | options.value | options.value_join_id_note_conflicts_with_enumerated_values -->

**Returns:** `search.Setting`

**Errors:**

- `SSS_INVALID_SRCH_SETTING`: the setting name is not found (unknown name).
- `SSS_INVALID_SRCH_SETTING_VALUE`: the value is not supported for that setting in this context.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

Including period-end transactions depends on an account feature, which this function's page names
Period End Journal Entries.

> [!SOURCE-ERROR] The sources name different features for the period-end setting: this function's page
> names Period End Journal Entries, while the `Setting` object's page names a "show period end
> transactions" feature. The spec keeps this page's wording without settling the question; check which
> feature your account actually has enabled.
<!-- source-error: N/search.createSetting | - | setting_page_names_show_period_end_transactions_feature -->

### search.lookupFields(options)
<!-- id: N/search.lookupFields -->

Reads body fields of one record without loading the record. Fields on a joined record are reached with
the form `joinId.fieldName`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | enum | Yes | |
| `options.columns` | string \| string[] | Yes | |
| `options.id` | string | Yes | |

- `options.type` (since 2015.2): a [`search.Type`](#searchtype) value.
- `options.columns` (since 2015.2): the field IDs to read; joined fields are allowed.
- `options.id` (since 2015.2): the record's internal ID.

**Returns:** `Object | Object[]`. An object keyed by the requested names. A single-value field comes
back as a plain value (an `internalid` of 1234, for instance, is just that scalar). A select or
multi-select field comes back as an array of value/text pairs, one per selected entry; a multi-select
holding two subsidiaries, say a US and an EU one, yields two entries.

> [!SOURCE-ERROR] The source describes the select-field return both as a single object and as an array
> of objects. The spec's return detail says array; write code that copes with both shapes until you
> have checked it on your field.
<!-- source-error: N/search.lookupFields | - | select_return_described_as_object_and_as_array -->

**Errors:**

- `SSS_INVALID_SRCH_COL`: a name in `options.columns` is not a field of that record.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 1 unit.

**Availability:** client and server scripts.

**Since:** 2015.2

Long text and multi-select values are truncated: the source gives 4000 characters, and also 1300
characters when several languages are in use and 3900 for a single language (see the note under
[`Result`](#result-object)). Load the record with `record.load` when you need the full value.

### search.lookupFields.promise(options)
<!-- id: N/search.lookupFields.promise -->

The asynchronous form of [`search.lookupFields`](#searchlookupfieldsoptions): same parameters and
errors, returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | enum | Yes | |
| `options.columns` | string \| string[] | Yes | |
| `options.id` | string | Yes | |

**Returns:** `Promise`

**Errors:** the same as `search.lookupFields`.

**Governance:** 1 unit.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.duplicates(options)
<!-- id: N/search.duplicates -->

Asks the account's duplicate-detection configuration which records look like duplicates. It covers
customers, leads, prospects, contacts, partners and vendors, and returns at most 1000 rows.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | enum | Yes | |
| `options.fields` | Object | No | |
| `options.id` | number | No | |

- `options.type` (since 2015.2): a [`search.Type`](#searchtype) value, limited to types that support
  duplicate detection.
- `options.fields` (since 2015.2): an object mapping field IDs to the values to compare, with fields
  such as `companyname`, `email`, `name`, `phone`, `address1`, `city`, `state` and `zipcode`. Required
  when you check a set of field values; do not combine it with `options.id`.
- `options.id` (since 2015.2): required when you look for duplicates of one specific record. Which of
  its fields are compared is decided by the account's duplicate-detection settings.

**Returns:** `search.Result[]`, or `null` when nothing matches. The declared return type does not
mention `null`, which looks inconsistent; check for both.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.duplicates.promise(options)
<!-- id: N/search.duplicates.promise -->

The asynchronous form of [`search.duplicates`](#searchduplicatesoptions): same parameters and errors,
returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | enum | Yes | |
| `options.fields` | Object | No | |
| `options.id` | number | No | |

**Returns:** `Promise`

**Errors:** the same as `search.duplicates`.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.global(options)
<!-- id: N/search.global -->

Runs a keyword search across the account, comparable to the global search box in the UI. Each result
row carries the columns `name`, `type`, `info1` and `info2`. At most 1000 rows come back.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.keywords` | string | Yes | |

- `options.keywords` (since 2015.2): plain keywords, or an expression with a prefix that narrows the
  kind of record, for example the `cu` prefix for customers. The source illustrates this with the `cu`
  prefix followed by a customer name; the exact separator is not preserved in the spec.

**Returns:** `search.Result[] | null`; `null` when nothing matches.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.keywords` is missing.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### search.global.promise(options)
<!-- id: N/search.global.promise -->

The asynchronous form of [`search.global`](#searchglobaloptions): same parameters and errors, returns a
Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.keywords` | string | Yes | |

**Returns:** `Promise`

**Errors:** the same as `search.global`.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

## Objects

### Search (object)
<!-- id: N/search.Search -->

A search definition, obtained from `search.create` or `search.load`. With it you can create, run and
save a search; it does not have to be saved before it can run. Client and server scripts; since 2015.2.

#### Search.run()
<!-- id: N/search.Search#run -->

Executes the search and returns a [`ResultSet`](#resultset-object). Running saves nothing. Use it on
searches from `search.create` or `search.load`, then walk the rows with
[`ResultSet.each`](#resultseteachcallback). Loading a saved search built in the UI is recommended for
better performance over building the same definition in code. Results are not cached, so if the data
changes while you iterate, the rows you see can change too.

No parameters.

**Returns:** `search.ResultSet`

**Errors:**

- `SSS_INVALID_SRCH_SETTING`: a search parameter (setting) name is unknown.
- `SSS_INVALID_SRCH_SETTING_VALUE`: a setting value is not supported for that parameter in this
  context.

**Governance:** 0 units.

**Availability:** the spec records client scripts only.

**Since:** 2015.2

> [!SOURCE-ERROR] The source's list of supported script types for this method contains only client
> scripts, which the spec flags as an error: the module, `Search` and the `ResultSet` this method
> returns are available on both sides. Treat the client-only listing as unreliable and confirm the
> behaviour in your server script type.
<!-- source-error: N/search.Search#run | - | supported_script_types_lists_only_client_scripts -->

#### Search.runPaged(options)
<!-- id: N/search.Search#runPaged -->

Runs the search in paged mode. The result is not the rows themselves but a
[`PagedData`](#pageddata-object) summary (count, page ranges, page size); fetch the rows page by page
with [`PagedData.fetch`](#pageddatafetch). A paged run covers at most 1000 pages.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.pageSize` | number | No | `50` |

- `options.pageSize`: the maximum number of entries per page, from 5 to 1000.

**Returns:** `search.PagedData`

**Errors:**

- `SSS_INVALID_SRCH_SETTING`: a search setting is not found.
- `SSS_INVALID_SRCH_SETTING_VALUE`: a setting value is not supported in this context.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2016.1

- The search needs a unique, unambiguous sort order; otherwise rows can be duplicated across pages or
  go missing.
- Results are not cached and may change while you page through them.

#### Search.runPaged.promise(options)
<!-- id: N/search.Search#runPaged.promise -->

The asynchronous form of [`Search.runPaged`](#searchrunpagedoptions): same parameters and errors,
returns a Promise. It needs the same unique, unambiguous sort order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.pageSize` | number | No | `50` |

**Returns:** `Promise`

**Errors:** the same as `Search.runPaged`.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2016.1

#### Search.save()
<!-- id: N/search.Search#save -->

Stores the definition as a saved search and returns the saved search's internal ID. A new saved search
needs a [`title`](#searchtitle) first; if no [`id`](#searchid) is set, a script ID is generated. When
you save a search you loaded, neither title nor ID is needed.

No parameters.

**Returns:** `number`, the internal ID of the saved search.

**Errors:**

- `NAME_ALREADY_IN_USE`: a saved search with that name already exists.
- `SSS_DUPLICATE_SEARCH_SCRIPT_ID`: a saved search with that script ID already exists.
- `SSS_MISSING_REQD_ARGUMENT`: a required value is missing.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Search.save.promise()
<!-- id: N/search.Search#save.promise -->

The asynchronous form of [`Search.save`](#searchsave): same behaviour and errors, returns a Promise.

No parameters.

**Returns:** `Promise`

**Errors:** the same as `Search.save`.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Search.columns
<!-- id: N/search.Search#columns -->

`search.Column[] | string[]`. The result columns. You can assign `Column` objects or column names, singly
or as an array. Assigning replaces the columns that were there; assigning `null` removes all of them. A
value that is neither a string nor a `Column` raises `SSS_INVALID_SRCH_COLUMN`. Since 2015.2.

> [!SOURCE-ERROR] The source gives this error as `SSS_INVALID_SRCH_COLUMN` here, but as
> `SSS_INVALID_SRCH_COL` for `search.create` and `search.lookupFields`. The spec does not say which
> spelling is right; when catching it, check the code your account actually throws, or test for both.
<!-- source-error: N/search.Search#columns | - | error_code_SSS_INVALID_SRCH_COLUMN_vs_SSS_INVALID_SRCH_COL -->

#### Search.filters
<!-- id: N/search.Search#filters -->

`search.Filter[]`. The conditions as `Filter` objects; `null` when the search has no filters. Assigning
replaces the existing filters; assigning `null` removes all of them. A value that does not suit the
filter's type raises `SSS_INVALID_SRCH_FILTER`. [`filterExpression`](#searchfilterexpression) is the
alternative way to express the same conditions. Since 2015.2.

#### Search.filterExpression
<!-- id: N/search.Search#filterExpression -->

`Array[]`. The filters written as an expression instead of `Filter` objects, a shortcut for building
filters with `search.createFilter`. The expression is a nested array whose elements are filter terms,
the operators `and` / `or` / `not`, and nested expressions; values inside can be objects, strings,
numbers or booleans. The logical operators follow JavaScript operator precedence, so group with nested
arrays when you mean something else. Since 2015.2.

- Assigning `null` removes all filter expressions.
- To get or set `Filter` objects, use [`filters`](#searchfilters) instead.
- `search.create` recognizes an expression passed in its `options.filters` automatically.
- An expression that cannot be parsed raises `SSS_INVALID_SRCH_FILTER_EXPR`; the source's example of a
  malformed expression has two `and` operators in a row between two terms.

#### Search.id
<!-- id: N/search.Search#id -->

`string`. The saved search's script ID, which must start with `customsearch`. If you leave it unset, one
is generated when the search is saved. It is a script ID, not an internal ID; for the internal ID see
[`searchId`](#searchsearchid). Since 2015.2.

#### Search.searchId
<!-- id: N/search.Search#searchId -->

`number`, read-only. The saved search's internal ID (numbers such as 55, 234 or 87). It is only
available when the search was loaded with `search.load` or after it has been saved. The `customsearch`
prefix is what tells the other identifier, [`id`](#searchid), apart as the script ID. Since 2015.2.

> [!SOURCE-ERROR] The source's description of this property contains a typo (one short word written in
> place of "it"), and the spec marks it for human review. Read the property as the internal ID, as
> described here.
<!-- source-error: N/search.Search#searchId | - | description_typo_is_for_it -->

#### Search.searchType
<!-- id: N/search.Search#searchType -->

`string`, read-only. The ID of the record type the search is based on, for instance `customer` or
`salesorder`. Use it when you have a search in hand but do not know its record type. Since 2015.2.

#### Search.title
<!-- id: N/search.Search#title -->

`string`. The saved search's title. Required before `save()`; it can also be set through `options.title`
of `search.create`. Since 2015.2.

#### Search.isPublic
<!-- id: N/search.Search#isPublic -->

`boolean`. `true` means the search is public. Defaults to `false`, so searches made with `search.create`
start out private. Since 2015.2.

#### Search.packageId
<!-- id: N/search.Search#packageId -->

`string`. The application ID of the SuiteApp this search belongs to, written as
`<publisher_id>.<project_id>`; the source's examples are `com.netsuite.mysuiteapp` and
`org.mycompany.helloworld`. Using it requires the preference that shows the application ID field.
Since 2019.2.

#### Search.settings
<!-- id: N/search.Search#settings -->

`search.Setting[] | string[]`. Search parameters that otherwise exist only in the UI. You can assign an
array of `Setting` objects, a single `Setting`, or strings; build the objects with
[`search.createSetting`](#searchcreatesettingoptions). An unknown parameter name raises
`SSS_INVALID_SRCH_SETTING`; a value the parameter does not support raises
`SSS_INVALID_SRCH_SETTING_VALUE`. Since 2018.2.

### ResultSet (object)
<!-- id: N/search.ResultSet -->

What `Search.run()` returns. A result set gives access to at most 4000 rows; for more, use
`Search.runPaged` or its promise form. Rows are not cached, so data may change while you walk them. Text
columns hold at most 4000 bytes. Client and server scripts; since 2015.2.

#### ResultSet.each(callback)
<!-- id: N/search.ResultSet#each -->

Calls your function once per row, for up to 4000 rows per call. The callback has the shape
`boolean callback(search.Result)`: it receives a [`Result`](#result-object) and returns `true` to go on
or `false` to stop.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `callback` | function | Yes | |

**Returns:** `void`

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

Whatever the callback does is charged to the calling script's governance. A callback running inside a
scheduled script, for example, spends that script's 10,000-unit allowance just as if the work were
written inline.

#### ResultSet.each.promise(callback)
<!-- id: N/search.ResultSet#each.promise -->

The asynchronous form of [`ResultSet.each`](#resultseteachcallback): same parameters and errors, the
same 4000-row ceiling, returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `callback` | function | Yes | |

**Returns:** `Promise`

**Errors:** the same as `ResultSet.each` (none stated in the source).

**Governance:** 10 units.

**Availability:** client scripts only.

**Since:** 2015.2

#### ResultSet.getRange(options)
<!-- id: N/search.ResultSet#getRange -->

Returns a slice of rows. One call returns at most 1000 rows, but by moving the window you can reach any
number of rows in total.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.start` | number | Yes | |
| `options.end` | number | Yes | |

- `options.start`: index of the first row; inclusive.
- `options.end`: index just past the last row; exclusive.

**Returns:** `search.Result[]`. Fewer rows than asked for when fewer exist (asking for 20 to 30 on a
25-row result gives 5), and an empty array when the range holds no results.

**Errors:** the source does not state any errors for this method.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### ResultSet.getRange.promise(options)
<!-- id: N/search.ResultSet#getRange.promise -->

The asynchronous form of [`ResultSet.getRange`](#resultsetgetrangeoptions): same parameters and
errors, returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.start` | number | Yes | |
| `options.end` | number | Yes | |

**Returns:** `Promise`

**Errors:** the same as `ResultSet.getRange` (none stated in the source).

**Governance:** 10 units.

**Availability:** client scripts only.

**Since:** 2015.2

#### ResultSet.columns
<!-- id: N/search.ResultSet#columns -->

`search.Column[]`, read-only. The columns returned in the results. Since 2015.2.

### Result (object)
<!-- id: N/search.Result -->

One row of search results. Client and server scripts; since 2015.2.

Size limits: a text column holds at most 4000 bytes, which means fewer characters when the text uses
multibyte characters. Long text and multi-select values are cut off: the source gives 4000 characters,
and also 1300 characters when the account uses several languages and 3900 for a single language. When
you need the complete long-text value, load the record with `record.load`.

> [!SOURCE-ERROR] The source states the long-text limit both as 4000 and as 3900 characters without
> reconciling them. Do not rely on either exact cut-off; load the record if the full text matters.
<!-- source-error: N/search.Result | - | long_text_limit_stated_as_both_4000_and_3900 -->

#### Result.getValue(options)
<!-- id: N/search.Result#getValue -->

Reads a column's value from the row; it works for standard fields and formula fields. It has two
overloads: `getValue(column)`, which takes the [`Column`](#column-object) object, and
`getValue(options)`, which takes an options object naming the column. You pass exactly one argument; the
table lists both `column` and `options` as required because each is the required argument of its own
overload.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.summary` | string | No | |
| `options.func` | string | No | |
| `options.join` | string | No | |
| `options.name` | string | Yes | |
| `column` | search.Column | Yes | |

- `options.summary`: a [`search.Summary`](#searchsummary) value.
- `options.func`: a function ID from the [search column functions](../gen/catalogs/search_column_functions.md).
- `options.join`: a join ID; the allowed values are the join IDs listed per record (see the search joins
  on the [record pages](../gen/records/index.md)).

**Returns:** `boolean | number | string`:

- a checkbox gives a boolean;
- a list/record, decimal or image field gives a number (for an image field, the ID of the image file);
- a multi-select field gives a string of comma-separated IDs;
- date and date/time fields give a formatted date string (convert it with [N/format](N_format.md));
- other field types give a string.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

In a search that groups rows, every column needs a summary type.

#### Result.getText(options)
<!-- id: N/search.Result#getText -->

Reads the display text of a column rather than its stored value. It is meant for select, image and
document fields. Like `getValue`, it has two overloads: `getText(column)`, which takes the
[`Column`](#column-object) object, and `getText(options)`, which takes an options object naming the
column. Call it with one argument in one of those forms, never both; the table marks `column` and
`options` as required because each is the required argument of its own overload.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.summary` | string | No | |
| `options.name` | string | Yes | |
| `options.join` | string | No | |
| `column` | search.Column | Yes | |

- `options.summary`: a [`search.Summary`](#searchsummary) value.
- `column`: a search column object ([`Column`](#column-object)).

**Returns:** `string`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

See the note at the top of the page about which select fields each form supports.

#### Result.columns
<!-- id: N/search.Result#columns -->

`search.Column[]`, read-only. The columns of this row. Since 2015.2.

#### Result.id
<!-- id: N/search.Result#id -->

`string`, read-only. The internal ID of the record this row belongs to, given as a number in string form
(for example `'237'`). Since 2015.2.

#### Result.recordType
<!-- id: N/search.Result#recordType -->

`search.Type`. The type of the record this row belongs to, one of the [`search.Type`](#searchtype)
values. Since 2015.2.

### Column (object)
<!-- id: N/search.Column -->

One result column. It is created with `search.createColumn`, added to searches through `search.create`
or a loaded search (`search.load`), returned in `ResultSet.columns`, and passed to `Result.getValue` and
`Result.getText`. Client and server scripts; since 2015.2.

#### Column.setWhenOrderedBy(options)
<!-- id: N/search.Column#setWhenOrderedBy -->

Makes this column report its value from the row where another column reaches its minimum or maximum.
Typical uses: the value on the most recent or the earliest date, or on the largest or smallest amount.
The column must use the `MIN` or `MAX` [summary](#searchsummary).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.join` | string | Yes | |

- `options.name` names the column to order by; `options.join` is its join ID. The spec marks the join as
  required; it does not say what to pass when the ordering column is not joined.

**Returns:** `search.Column`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Column.name
<!-- id: N/search.Column#name -->

`string`, read-only. The column name. Since 2015.2.

#### Column.join
<!-- id: N/search.Column#join -->

`string`, read-only. The join ID used to reach the field on a related record. Since 2015.2.

#### Column.summary
<!-- id: N/search.Column#summary -->

`search.Summary`. The summary type, one of the [`search.Summary`](#searchsummary) values. Since 2015.2.

#### Column.formula
<!-- id: N/search.Column#formula -->

`string`. The formula of a formula column. You can only set it when the column's name is one of the
formula types: `formulatext`, `formulanumeric`, `formuladatetime`, `formulapercent` or
`formulacurrency`. Inside the formula, refer to fields by their script IDs; the source's example builds a
customer name out of the first-name and last-name fields. See [Search formulas](../topics/search_formulas.md)
and the [search formula functions](../gen/catalogs/search_formula_functions.md). Since 2015.2.

#### Column.function
<!-- id: N/search.Column#function -->

`string`. A function applied to the column, chosen from the
[search column functions](../gen/catalogs/search_column_functions.md); the catalog's values give the
function's output type. Setting an unknown function raises `INVALID_SRCH_FUNCTN`. Since 2015.2.

#### Column.label
<!-- id: N/search.Column#label -->

`string`. Gets or sets custom labels only. Which label that is depends on the summary type: with `GROUP`
it is the custom label, with any other summary type it is the summary label. A column cannot have a
custom label and a summary label at the same time. Since 2015.2.

#### Column.sort
<!-- id: N/search.Column#sort -->

`search.Sort`. The sort direction, set through `search.createColumn` with a [`search.Sort`](#searchsort)
value. Without a sort, rows come back in no particular order. The sort cannot be changed once the column
is created, and a column reused in another search keeps its original sort. Since 2015.2.

### Filter (object)
<!-- id: N/search.Filter -->

One filter condition, created with `search.createFilter` and added to searches through `search.create`
or a loaded search (`search.load`). Since 2015.2. The source does not state whether the `Filter` object
is available in client scripts, server scripts or both.

- Several `Filter` objects are combined with an implicit AND. Filter expressions are different: there
  you write `and` / `or` explicitly.
- To leave out rows where a field has no value, use `@NONE@` as the value with a "none of" operator.
- For a checkbox, use the `IS` operator with `T` (checked) or `F` (unchecked).

#### Filter.name
<!-- id: N/search.Filter#name -->

`string`, read-only. The search field name or internal ID being filtered. Since 2015.2.

#### Filter.join
<!-- id: N/search.Filter#join -->

`string`, read-only. The join ID used to reach a field on a related record. Since 2015.2.

#### Filter.operator
<!-- id: N/search.Filter#operator -->

`string`, read-only. The operator, one of the [`search.Operator`](#searchoperator) values. Since 2015.2.

#### Filter.summary
<!-- id: N/search.Filter#summary -->

`search.Summary`. The summary type, one of the [`search.Summary`](#searchsummary) values. Setting an
unknown summary type raises `SSS_INVALID_SRCH_FILTER_SUM`. Since 2015.2.

#### Filter.formula
<!-- id: N/search.Filter#formula -->

`string`. The formula of a formula filter (see [Search formulas](../topics/search_formulas.md)).
Since 2015.2.

### Setting (object)
<!-- id: N/search.Setting -->

One search setting. Two settings are supported: the consolidated exchange rate and whether to show
period-end transactions. They apply to transaction searches in OneWorld accounts. This object's page
names the "show period end transactions" feature as the requirement for the period-end setting (see the
note under [`search.createSetting`](#searchcreatesettingoptions), whose page names a different feature).
Create settings with `search.createSetting` and assign them to [`Search.settings`](#searchsettings).
Client and server scripts; since 2018.2.

#### Setting.name
<!-- id: N/search.Setting#name -->

`string`, read-only. `consolidationtype` or `includeperiodendtransactions`; set through
`search.createSetting`. Since 2018.2.

#### Setting.value
<!-- id: N/search.Setting#value -->

`string`, read-only; not case-sensitive. For `consolidationtype`: `ACCTTYPE`, `AVERAGE`, `CURRENT`,
`HISTORICAL` or `NONE`. For `includeperiodendtransactions`: `F`, `FALSE`, `T` or `TRUE`. Since 2018.2.

### PagedData (object)
<!-- id: N/search.PagedData -->

Returned by `Search.runPaged`. It gives you the total count, the page ranges and the page size; the rows
themselves are fetched one page at a time. Page sizes run from 5 to 1000 rows, and the last page may hold
fewer. Results are not cached and may change while you page through them. Client and server scripts.

> [!SOURCE-ERROR] The source dates this object to 2015.1 but every one of its members to 2016.1, and the
> spec marks this for human review. Treat 2016.1 (when `runPaged` arrived) as the safer minimum.
<!-- source-error: N/search.PagedData | - | object_since_2015.1_vs_members_since_2016.1 -->

#### PagedData.fetch(...)
<!-- id: N/search.PagedData#fetch -->

Returns the [`Page`](#page-object) for one page range.

The spec has no structured parameter list for this method. The source documents the input as
`pageRange.index`: a required, zero-based number choosing the range to fetch. Passing a
[`PageRange`](#pagerange-object) object taken from `pageRanges` supplies that index.

> [!SOURCE-ERROR] The source's parameter row is named `pageRange.index`, while its sample passes an
> options object with `index`. The spec marks this for human review. A `PageRange` object, which has an
> `index` property, fits both readings; otherwise pass an object with `index`.
<!-- source-error: N/search.PagedData#fetch | - | param_row_named_pageRange.index_sample_uses_options.index -->

**Returns:** `search.Page`

**Errors:**

- `INVALID_PAGE_RANGE`: the index does not identify a valid page range.

**Governance:** 5 units.

**Availability:** client and server scripts. The asynchronous counterpart is
[`fetch.promise`](#pageddatafetchpromise).

**Since:** 2016.1

#### PagedData.fetch.promise(...)
<!-- id: N/search.PagedData#fetch.promise -->

The asynchronous form of [`PagedData.fetch`](#pageddatafetch): same input and errors, returns a Promise.
As with `fetch`, the spec has no structured parameter list.

**Returns:** `Promise`

**Errors:** the same as `PagedData.fetch`.

**Governance:** 5 units.

**Availability:** client scripts only.

**Since:** 2016.1

#### PagedData.count
<!-- id: N/search.PagedData#count -->

`number`, read-only. The total number of results at the moment `runPaged` was executed. Since 2016.1.

#### PagedData.pageRanges
<!-- id: N/search.PagedData#pageRanges -->

`search.PageRange[]`, read-only. The ranges that divide the result set into pages, indexed from 0.
Since 2016.1.

#### PagedData.pageSize
<!-- id: N/search.PagedData#pageSize -->

`number`, read-only. Rows per page, between 5 and 1000. Since 2016.1.

#### PagedData.searchDefinition
<!-- id: N/search.PagedData#searchDefinition -->

`search.Search`, read-only. The search criteria used to produce this paged data. Since 2016.1.

### Page (object)
<!-- id: N/search.Page -->

One page of paginated search results, produced by `PagedData.fetch`. Client and server scripts.

> [!SOURCE-ERROR] The source dates this object to 2015.1 but its members to 2016.1, and the spec marks
> this for human review. Treat 2016.1 as the safer minimum.
<!-- source-error: N/search.Page | - | object_since_2015.1_vs_members_since_2016.1 -->

#### Page.next()
<!-- id: N/search.Page#next -->

Moves this page object on to the next page range.

No parameters.

**Returns:** `Void`

**Errors:**

- `INVALID_PAGE_RANGE`: the page is in an invalid state for the move, that is, the range is invalid or
  the page is already the last one.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2016.1

#### Page.next.promise()
<!-- id: N/search.Page#next.promise -->

The asynchronous form of [`Page.next`](#pagenext): same errors, returns a Promise that settles once the
next range's data has loaded, or rejects.

No parameters.

**Returns:** `Promise`

**Errors:** the same as `Page.next`.

**Governance:** 5 units.

**Availability:** client scripts only.

**Since:** 2016.1

#### Page.prev()
<!-- id: N/search.Page#prev -->

Moves this page object back to the previous page range.

No parameters.

**Returns:** `Void`

**Errors:**

- `INVALID_PAGE_RANGE`: the page is in an invalid state for the move, that is, the range is invalid or
  the page is already the first one.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2016.1

#### Page.prev.promise()
<!-- id: N/search.Page#prev.promise -->

The asynchronous form of [`Page.prev`](#pageprev): same errors, returns a Promise that settles once the
previous range's data has loaded, or rejects.

No parameters.

**Returns:** `Promise`

**Errors:** the same as `Page.prev`.

**Governance:** 5 units.

**Availability:** client scripts only.

**Since:** 2016.1

#### Page.data
<!-- id: N/search.Page#data -->

`search.Result[]`, read-only. The results on this page. Since 2016.1.

#### Page.pageRange
<!-- id: N/search.Page#pageRange -->

`search.PageRange`, read-only. The range this page covers, with its index and label. Since 2016.1.

#### Page.pagedData
<!-- id: N/search.Page#pagedData -->

`search.PagedData`, read-only. The paged data object that fetched this page. Since 2016.1.

#### Page.isFirst
<!-- id: N/search.Page#isFirst -->

`boolean`, read-only. `true` when this page is in the first range. Since 2016.1.

#### Page.isLast
<!-- id: N/search.Page#isLast -->

`boolean`, read-only. `true` when this page is in the last range. Since 2016.1.

### PageRange (object)
<!-- id: N/search.PageRange -->

The bounds of one page of results. Client and server scripts.

> [!SOURCE-ERROR] The source dates this object to 2015.1 but its members to 2016.1, and the spec marks
> this for human review. Treat 2016.1 as the safer minimum.
<!-- source-error: N/search.PageRange | - | object_since_2015.1_vs_members_since_2016.1 -->

#### PageRange.index
<!-- id: N/search.PageRange#index -->

`number`, read-only. The range's position, counting from 0. Since 2016.1.

#### PageRange.compoundLabel
<!-- id: N/search.PageRange#compoundLabel -->

`string`, read-only. A human-readable label showing where the range starts and ends. Since 2016.1.

## Enums

### search.Type
<!-- id: N/search.Type -->

The search types you pass to `search.create` (and to `load`, `delete`, `lookupFields` and
`duplicates`). A search type is not a record type, so do not assume this list lines up with `record.Type`
in [N/record](N_record.md). Each search type you use must be enabled in the account's settings.
Since 2015.2.

> [!SOURCE-ERROR] The source's value table repeats its header row as if it were the first value, and
> lists `MERCHANDISE_HIERARCHY_LEVEL` [sic] with a stray space inside the name. The spec leaves out the
> repeated header and has normalized the name; if the normalized constant fails at run time, check how
> your account spells it.
<!-- source-error: N/search.Type | - | header_row_repeated_as_first_value_row -->
<!-- source-error: N/search.Type | - | value_listed_with_stray_space -->
<!-- source-error: N/search.Type | MERCHANDISE_HIERARCHY_LEVEL | source_name_has_stray_space -->

| Value |
|---|
| `ACCOUNT` |
| `ACCOUNTING_BOOK` |
| `ACCOUNTING_CONTEXT` |
| `ACCOUNTING_PERIOD` |
| `ACTIVITY` |
| `ADV_INTER_COMPANY_JOURNAL_ENTRY` |
| `AGGR_FIN_DAT` |
| `ALLOC_RECOMMENDATION_DEMAND` |
| `ALLOC_RECOMMENDATION_DETAIL` |
| `AMORTIZATION_SCHEDULE` |
| `AMORTIZATION_TEMPLATE` |
| `ASSEMBLY_BUILD` |
| `ASSEMBLY_ITEM` |
| `ASSEMBLY_UNBUILD` |
| `AUTHENTICATE_DEVICE_INPUT` |
| `BALANCE_TRX_BY_SEGMENTS` |
| `BALANCING_DETAIL` |
| `BALANCING_RESULT` |
| `BALANCING_TRANSACTION` |
| `BILLING_ACCOUNT` |
| `BILLING_ACCOUNT_BILL_CYCLE` |
| `BILLING_ACCOUNT_BILL_REQUEST` |
| `BILLING_CLASS` |
| `BILLING_RATE_CARD` |
| `BILLING_REVENUE_EVENT` |
| `BILLING_SCHEDULE` |
| `BIN` |
| `BIN_ITEM_BALANCE` |
| `BIN_TRANSFER` |
| `BIN_WORKSHEET` |
| `BLANKET_PURCHASE_ORDER` |
| `BOM` |
| `BOM_REVISION` |
| `BONUS` |
| `BONUS_TYPE` |
| `BUDGET_EXCHANGE_RATE` |
| `BULK_OWNERSHIP_TRANSFER` |
| `BUNDLE_INSTALLATION_SCRIPT` |
| `CALENDAR_EVENT` |
| `CAMPAIGN` |
| `CARDHOLDER_AUTHENTICATION` |
| `CARDHOLDER_AUTHENTICATION_EVENT` |
| `CASH_REFUND` |
| `CASH_SALE` |
| `CHALLENGE_SHOPPER_INPUT` |
| `CHARGE` |
| `CHARGE_RULE` |
| `CHECK` |
| `CLASSIFICATION` |
| `CLIENT_SCRIPT` |
| `CMS_CONTENT` |
| `CMS_CONTENT_TYPE` |
| `CMS_PAGE` |
| `COM_SEARCH_BOOST` |
| `COM_SEARCH_BOOST_TYPE` |
| `COM_SEARCH_GROUP_SYN` |
| `COM_SEARCH_ONE_WAY_SYN` |
| `COMM_AN_SESSION` |
| `COMMERCE_CATEGORY` |
| `COMMERCE_SEARCH_ACTIVITY_DATA` |
| `COMPETITOR` |
| `CONSOLIDATED_EXCHANGE_RATE` |
| `CONTACT` |
| `CONTACT_CATEGORY` |
| `CONTACT_ROLE` |
| `COST_CATEGORY` |
| `COUPON_CODE` |
| `CREDIT_CARD_CHARGE` |
| `CREDIT_CARD_REFUND` |
| `CREDIT_MEMO` |
| `CURRENCY` |
| `CURRENCY_EXCHANGE_RATE` |
| `CUSTOMER` |
| `CUSTOMER_CATEGORY` |
| `CUSTOMER_DEPOSIT` |
| `CUSTOMER_MESSAGE` |
| `CUSTOMER_PAYMENT` |
| `CUSTOMER_PAYMENT_AUTHORIZATION` |
| `CUSTOMER_REFUND` |
| `CUSTOMER_STATUS` |
| `CUSTOMER_SUBSIDIARY_RELATIONSHIP` |
| `CUSTOM_PURCHASE` |
| `CUSTOM_RECORD` |
| `CUSTOM_SALE` |
| `CUSTOM_TRANSACTION` |
| `DELETED_RECORD` |
| `DEPARTMENT` |
| `DEPOSIT` |
| `DEPOSIT_APPLICATION` |
| `DESCRIPTION_ITEM` |
| `DISCOUNT_ITEM` |
| `DOWNLOAD_ITEM` |
| `EMAIL_TEMPLATE` |
| `EMPLOYEE` |
| `EMPLOYEE_CHANGE_REQUEST` |
| `EMPLOYEE_CHANGE_REQUEST_TYPE` |
| `EMPLOYEE_CHANGE_TYPE` |
| `EMPLOYEE_PAYROLL_ITEM` |
| `EMPLOYEE_STATUS` |
| `EMPLOYEE_TYPE` |
| `END_TO_END_TIME` |
| `ENTITY` |
| `ENTITY_ACCOUNT_MAPPING` |
| `ESTIMATE` |
| `EXPENSE_AMORTIZATION_EVENT` |
| `EXPENSE_AMORT_PLAN_AND_SCHEDULE` |
| `EXPENSE_CATEGORY` |
| `EXPENSE_PLAN` |
| `EXPENSE_REPORT` |
| `EXPENSE_REPORT_POLICY` |
| `FAIR_VALUE_PRICE` |
| `FINANCIAL_INSTITUTION` |
| `FIXED_AMOUNT_PROJECT_REVENUE_RULE` |
| `FOLDER` |
| `FULFILLMENT_REQUEST` |
| `GATEWAY_NOTIFICATION` |
| `GENERIC_RESOURCE` |
| `GIFT_CERTIFICATE` |
| `GIFT_CERTIFICATE_ITEM` |
| `GLOBAL_ACCOUNT_MAPPING` |
| `GLOBAL_INVENTORY_RELATIONSHIP` |
| `GL_LINES_AUDIT_LOG` |
| `GL_NUMBERING_SEQUENCE` |
| `GOAL` |
| `IMPORTED_EMPLOYEE_EXPENSE` |
| `INBOUND_SHIPMENT` |
| `INSTALLMENT` |
| `INTER_COMPANY_JOURNAL_ENTRY` |
| `INTER_COMPANY_TRANSFER_ORDER` |
| `INVENTORY_ADJUSTMENT` |
| `INVENTORY_BALANCE` |
| `INVENTORY_COST_REVALUATION` |
| `INVENTORY_COUNT` |
| `INVENTORY_DEMAND` |
| `INVENTORY_DETAIL` |
| `INVENTORY_ITEM` |
| `INVENTORY_NUMBER` |
| `INVENTORY_NUMBER_BIN` |
| `INVENTORY_NUMBER_ITEM` |
| `INVENTORY_STATUS` |
| `INVENTORY_STATUS_CHANGE` |
| `INVENTORY_STATUS_LOCATION` |
| `INVENTORY_TRANSFER` |
| `INVOICE` |
| `INVOICE_GROUP` |
| `INVT_NUMBER_ITEM_BALANCE` |
| `ISSUE` |
| `ITEM` |
| `ITEM_ACCOUNT_MAPPING` |
| `ITEM_BIN_NUMBER` |
| `ITEM_COLLECTION` |
| `ITEM_COLLECTION_ITEM_MAP` |
| `ITEM_DEMAND_PLAN` |
| `ITEM_FULFILLMENT` |
| `ITEM_GROUP` |
| `ITEM_LOCATION_CONFIGURATION` |
| `ITEM_PROCESS_FAMILY` |
| `ITEM_PROCESS_GROUP` |
| `ITEM_RECEIPT` |
| `ITEM_REVISION` |
| `ITEM_SUPPLY_PLAN` |
| `JOB` |
| `JOB_STATUS` |
| `JOB_TYPE` |
| `JOURNAL_ENTRY` |
| `KIT_ITEM` |
| `LABOR_BASED_PROJECT_REVENUE_RULE` |
| `LABOR_CATEGORY` |
| `LABOR_COST_CARD` |
| `LABOR_COST_CARD_ITEM` |
| `LABOR_COST_CARD_SEGMENT` |
| `LABOR_COST_ELEMENT` |
| `LEAD` |
| `LOCATION` |
| `LOT_NUMBERED_ASSEMBLY_ITEM` |
| `LOT_NUMBERED_INVENTORY_ITEM` |
| `MANUFACTURING_COST_TEMPLATE` |
| `MANUFACTURING_OPERATION_TASK` |
| `MANUFACTURING_ROUTING` |
| `MAP_REDUCE_SCRIPT` |
| `MARKUP_ITEM` |
| `MASSUPDATE_SCRIPT` |
| `MEM_DOC` |
| `MERCHANDISE_HIERARCHY_LEVEL` [sic] (the source spells it with a stray space; see the note above) |
| `MERCHANDISE_HIERARCHY_NODE` |
| `MERCHANDISE_HIERARCHY_VERSION` |
| `MESSAGE` |
| `MFG_PLANNED_TIME` |
| `NEXUS` |
| `NON_INVENTORY_ITEM` |
| `NOTE` |
| `NOTE_TYPE` |
| `OPPORTUNITY` |
| `ORDER_RESERVATION` |
| `ORDER_TYPE` |
| `OTHER_CHARGE_ITEM` |
| `OTHER_NAME` |
| `OTHER_NAME_CATEGORY` |
| `PARTNER` |
| `PARTNER_CATEGORY` |
| `PAYCHECK` |
| `PAYCHECK_JOURNAL` |
| `PAYMENT_EVENT` |
| `PAYMENT_INSTRUMENT` |
| `PAYMENT_ITEM` |
| `PAYMENT_METHOD` |
| `PAYMENT_OPTION` |
| `PAYMENT_RESULT_PREVIEW` |
| `PAYROLL_SETUP` |
| `PAYROLL_ITEM` |
| `PCT_COMPLETE_PROJECT_REVENUE_RULE` |
| `PERFORMANCE_METRIC` |
| `PERFORMANCE_REVIEW` |
| `PERFORMANCE_REVIEW_SCHEDULE` |
| `PERIOD_END_JOURNAL` |
| `PERMISSION` |
| `PHONE_CALL` |
| `PICK_STRATEGY` |
| `PICK_TASK` |
| `PLANNED_ORDER` |
| `PLANNING_ENGINE_MESSAGE` |
| `PLANNING_ENGINE_PEGGING` |
| `PLANNING_ENGINE_RESULT` |
| `PLANNING_ITEM_CATEGORY` |
| `PLANNING_ITEM_GROUP` |
| `PLANNING_REPOSITORY_ALLOCATION` |
| `PLANNING_REPOSITORY_BOM_EDGE` |
| `PLANNING_REPOSITORY_ITEM_LOCATION` |
| `PLANNING_REPOSITORY_SOURCE` |
| `PLANNING_RULE_GROUP` |
| `PLANNING_VIEW` |
| `PORTLET` |
| `PRICE_BOOK` |
| `PRICE_LEVEL` |
| `PRICE_PLAN` |
| `PRICING` |
| `PRICING_GROUP` |
| `PROJECT_EXPENSE_TYPE` |
| `PROJECT_IC_CHARGE_REQUEST` |
| `PROJECT_TASK` |
| `PROJECT_TEMPLATE` |
| `PROMISING_SETUP` |
| `PROMOTION_CODE` |
| `PROSPECT` |
| `PURCHASE_CONTRACT` |
| `PURCHASE_ORDER` |
| `PURCHASE_REQUISITION` |
| `RECENT_RECORD` |
| `RECEIVED_VENDOR_BILL` |
| `RES_ALLOCATION_TIME_OFF_CONFLICT` |
| `RESOURCE_ALLOCATION` |
| `RESTLET` |
| `RETURN_AUTHORIZATION` |
| `REVENUE_ARRANGEMENT` |
| `REVENUE_COMMITMENT` |
| `REVENUE_COMMITMENT_REVERSAL` |
| `REVENUE_PLAN` |
| `REV_REC_PLAN_AND_SCHEDULE` |
| `REV_REC_SCHEDULE` |
| `REV_REC_TEMPLATE` |
| `ROLE` |
| `S_C_M_PREDICTED_RISKS` |
| `S_C_M_PREDICTION_TRAIN_HISTORY` |
| `S_C_M_PREDICTION_TRAIN_W_Q_STATUS` |
| `SALES_CHANNEL` |
| `SALES_ORDER` |
| `SALES_ROLE` |
| `SALES_TAX_ITEM` |
| `SAVED_SEARCH` |
| `SCHEDULED_SCRIPT` |
| `SCHEDULED_SCRIPT_INSTANCE` |
| `SCRIPT_DEPLOYMENT` |
| `SERIALIZED_ASSEMBLY_ITEM` |
| `SERIALIZED_INVENTORY_ITEM` |
| `SERVICE_ITEM` |
| `SHIP_ITEM` |
| `SHOPPING_CART` |
| `SOLUTION` |
| `STATE` |
| `STATISTICAL_JOURNAL_ENTRY` |
| `STORE_PICKUP_FULFILLMENT` |
| `SUBSCRIPTION` |
| `SUBSCRIPTION_CHANGE_ORDER` |
| `SUBSCRIPTION_LINE` |
| `SUBSCRIPTION_LINE_REVISION` |
| `SUBSCRIPTION_PLAN` |
| `SUBSCRIPTION_RENEWAL_HISTORY` |
| `SUBSCRIPTION_TERM` |
| `SUBSIDIARY` |
| `SUBTOTAL_ITEM` |
| `SUITELET` |
| `SUITE_SCRIPT_DETAIL` |
| `SUPPLY_CHAIN_SNAPSHOT` |
| `SUPPLY_CHAIN_SNAPSHOT_DETAILS` |
| `SUPPLY_CHANGE_ORDER` |
| `SUPPLY_PLAN_DEFINITION` |
| `SUPPORT_CASE` |
| `SYSTEM_NOTE` |
| `TASK` |
| `TAX_DETAIL` |
| `TAX_GROUP` |
| `TAX_PERIOD` |
| `TAX_TYPE` |
| `TERM` |
| `TIMESHEET_APPROVAL` |
| `TIME_APPROVAL` |
| `TIME_BILL` |
| `TIME_ENTRY` |
| `TIME_OFF_CHANGE` |
| `TIME_OFF_PLAN` |
| `TIME_OFF_REQUEST` |
| `TIME_OFF_RULE` |
| `TIME_OFF_TYPE` |
| `TIME_SHEET` |
| `TOPIC` |
| `TRANSACTION` |
| `TRANSFER_ORDER` |
| `UBER` |
| `UNITS_TYPE` |
| `UNLOCKED_TIME_PERIOD` |
| `USAGE` |
| `USEREVENT_SCRIPT` |
| `VENDOR` |
| `VENDOR_BILL` |
| `VENDOR_CATEGORY` |
| `VENDOR_CREDIT` |
| `VENDOR_PAYMENT` |
| `VENDOR_PREPAYMENT` |
| `VENDOR_PREPAYMENT_APPLICATION` |
| `VENDOR_RETURN_AUTHORIZATION` |
| `VENDOR_SUBSIDIARY_RELATIONSHIP` |
| `WAVE` |
| `WBS` |
| `WEBSITE` |
| `WORKFLOW_ACTION_SCRIPT` |
| `WORK_ORDER` |
| `WORK_ORDER_CLOSE` |
| `WORK_ORDER_COMPLETION` |
| `WORK_ORDER_ISSUE` |
| `WORKPLACE` |
| `ZONE` |

### search.Operator
<!-- id: N/search.Operator -->

Operators for filter conditions, used by `search.createFilter` (`options.operator`) and reported by
`Filter.operator`. Each operator only suits certain field types, as the table shows; the source keeps
that support in its search operators table, which is rendered as the
[operators by field type](../gen/catalogs/search_operator_field_types.md) catalog. Since 2015.2.

| Value | Field types it applies to |
|---|---|
| `AFTER` | Date |
| `ALLOF` | Multiple select |
| `ANY` | Currency, decimal number, time of day; text, email, password, percent, phone |
| `ANYOF` | List/record; document, image; multiple select |
| `BEFORE` | Date |
| `BETWEEN` | Currency, decimal number, time of day |
| `CONTAINS` | Text, email, password, percent, phone |
| `DOESNOTCONTAIN` | Text, email, password, percent, phone |
| `DOESNOTSTARTWITH` | Text, email, password, percent, phone |
| `EQUALTO` | Currency, decimal number, time of day; checkbox |
| `GREATERTHAN` | Currency, decimal number, time of day |
| `GREATERTHANOREQUALTO` | Currency, decimal number, time of day |
| `HASKEYWORDS` | Text, email, password, percent, phone |
| `IS` | Checkbox; text, email, password, percent, phone |
| `ISEMPTY` | Currency, decimal number, time of day; date; text, email, password, percent, phone |
| `ISNOT` | Text, email, password, percent, phone |
| `ISNOTEMPTY` | Currency, decimal number, time of day; date; text, email, password, percent, phone |
| `LESSTHAN` | Currency, decimal number, time of day |
| `LESSTHANOREQUALTO` | Currency, decimal number, time of day |
| `NONEOF` | List/record; document, image; multiple select |
| `NOTAFTER` | Date |
| `NOTALLOF` | Multiple select |
| `NOTBEFORE` | Date |
| `NOTBETWEEN` | Currency, decimal number, time of day |
| `NOTEQUALTO` | Currency, decimal number, time of day |
| `NOTGREATERTHAN` | Currency, decimal number, time of day |
| `NOTGREATERTHANOREQUALTO` | Currency, decimal number, time of day |
| `NOTLESSTHAN` | Currency, decimal number, time of day |
| `NOTLESSTHANOREQUALTO` | Currency, decimal number, time of day |
| `NOTON` | Date |
| `NOTONORAFTER` | Date |
| `NOTONORBEFORE` | Date |
| `NOTWITHIN` | Date |
| `ON` | Date |
| `ONORAFTER` | Date |
| `ONORBEFORE` | Date |
| `STARTSWITH` | Text, email, password, percent, phone |
| `WITHIN` | Date |

### search.Summary
<!-- id: N/search.Summary -->

Summary types for searches that group or aggregate rows, used by `Column.summary` and `Filter.summary`
(and the `summary` options of `createColumn`, `createFilter`, `Result.getValue` and `Result.getText`).
The spec lists the values without describing them; the names speak for themselves. Since 2015.2.

| Value | Note |
|---|---|
| `GROUP` | Grouping; with `GROUP`, `Column.label` is the custom label |
| `COUNT` | |
| `SUM` | |
| `AVG` | |
| `MIN` | Required (or `MAX`) by `Column.setWhenOrderedBy` |
| `MAX` | Required (or `MIN`) by `Column.setWhenOrderedBy` |

### search.Sort
<!-- id: N/search.Sort -->

Sort directions for a column, used by `search.createColumn` (`options.sort`) and `Column.sort`.
Since 2015.2.

| Value | Meaning |
|---|---|
| `ASC` | Ascending |
| `DESC` | Descending |
| `NONE` | No sort |

## Examples

**Paging through customers with an overdue balance.** A scheduled script that lists active customers
who owe money, largest overdue amount first. The second sort key (`entityid`) makes the order
unambiguous, which `runPaged` needs to avoid duplicated or missing rows. Each `PageRange` is handed to
`fetch`, which supplies the page index.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const overdue = search.create({
      type: search.Type.CUSTOMER,
      filters: [
        ['isinactive', search.Operator.IS, 'F'],
        'and',
        ['overduebalance', search.Operator.GREATERTHAN, 0]
      ],
      columns: [
        search.createColumn({ name: 'overduebalance', sort: search.Sort.DESC }),
        search.createColumn({ name: 'entityid', sort: search.Sort.ASC }),
        'daysoverdue',
        search.createColumn({ name: 'email', join: 'salesrep' })
      ]
    });

    const paged = overdue.runPaged({ pageSize: 250 });
    log.audit({ title: 'Overdue customers', details: `${paged.count} rows / ${paged.pageRanges.length} pages` });

    paged.pageRanges.forEach((range) => {
      const page = paged.fetch(range);
      page.data.forEach((row) => {
        const owed = Number(row.getValue({ name: 'overduebalance' }));
        const repEmail = row.getValue({ name: 'email', join: 'salesrep' }) || 'unassigned';
        if (owed > 10000) {
          log.debug({ title: `Escalate ${row.getValue({ name: 'entityid' })}`, details: `${owed} owed, rep ${repEmail}` });
        }
      });
    });
  };

  return { execute };
});
```

**Walking a saved search with a governance guard.** Loads a saved search whose script ID comes from a
script parameter, then stops early when the remaining usage runs low. Returning `false` from the
callback ends `each`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/runtime', 'N/log'], (search, runtime, log) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const savedId = script.getParameter({ name: 'custscript_stale_quote_search' });
    const stale = search.load({ id: savedId });

    let handled = 0;
    stale.run().each((result) => {
      handled += 1;
      log.debug({ title: `Row ${handled}`, details: `${result.recordType} ${result.id}` });
      return script.getRemainingUsage() > 200;
    });

    log.audit({ title: 'Stale quote sweep', details: `${handled} rows visited` });
  };

  return { execute };
});
```

**Revenue per sales rep with a summary search.** Groups main-line transactions by sales rep and totals
them; every column carries a summary type because the search groups. `getText(column)` and
`getValue(column)` use the column-object overloads. `@NONE@` with `NONEOF` drops rows that have no rep.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/search'], (search) => {
  const onRequest = (context) => {
    const repColumn = search.createColumn({ name: 'salesrep', summary: search.Summary.GROUP });
    const totalColumn = search.createColumn({ name: 'amount', summary: search.Summary.SUM, sort: search.Sort.DESC });
    const latestColumn = search.createColumn({ name: 'trandate', summary: search.Summary.MAX });

    const byRep = search.create({
      type: search.Type.TRANSACTION,
      filters: [
        ['mainline', search.Operator.IS, 'T'],
        'and',
        ['trandate', search.Operator.WITHIN, 'thisFiscalYear'],
        'and',
        ['salesrep', search.Operator.NONEOF, '@NONE@']
      ],
      columns: [repColumn, totalColumn, latestColumn]
    });

    const lines = byRep.run().getRange({ start: 0, end: 50 }).map((row) => {
      const rep = row.getText(repColumn);
      const total = row.getValue(totalColumn);
      const latest = row.getValue(latestColumn);
      return `${rep}: ${total} (last sale ${latest})`;
    });

    context.response.write({ output: lines.join('\n') });
  };

  return { onRequest };
});
```

**Credit headroom via a formula column and a formula filter.** A RESTlet that returns customers whose
remaining credit (limit minus balance) has dropped under a threshold. The formula refers to fields by
their IDs; see [Search formulas](../topics/search_formulas.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/search'], (search) => {
  const get = (params) => {
    const floor = Number(params.floor || 500);
    const headroom = search.createColumn({
      name: 'formulanumeric',
      formula: '{creditlimit} - {balance}',
      sort: search.Sort.ASC
    });

    const tight = search.create({
      type: search.Type.CUSTOMER,
      filters: [
        search.createFilter({ name: 'creditlimit', operator: search.Operator.GREATERTHAN, values: 0 }),
        search.createFilter({
          name: 'formulanumeric',
          formula: '{creditlimit} - {balance}',
          operator: search.Operator.LESSTHAN,
          values: floor
        })
      ],
      columns: [headroom, search.createColumn({ name: 'entityid', sort: search.Sort.ASC })]
    });

    return tight.run().getRange({ start: 0, end: 100 }).map((row) => ({
      id: row.id,
      customer: row.getValue({ name: 'entityid' }),
      headroom: row.getValue(headroom)
    }));
  };

  return { get };
});
```

**Average-rate consolidation on a transaction search.** In a OneWorld account, a setting built with
`createSetting` asks for the `AVERAGE` consolidated exchange rate, something otherwise chosen in the UI.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const averageRate = search.createSetting({ name: 'consolidationtype', value: 'AVERAGE' });

    const totals = search.create({
      type: search.Type.TRANSACTION,
      filters: [
        ['mainline', search.Operator.IS, 'T'],
        'and',
        ['trandate', search.Operator.WITHIN, 'thisFiscalYear']
      ],
      columns: [search.createColumn({ name: 'amount', summary: search.Summary.SUM })],
      settings: [averageRate]
    });

    totals.run().each((row) => {
      log.audit({ title: 'Transaction total this fiscal year', details: String(row.getValue({ name: 'amount', summary: search.Summary.SUM })) });
      return false;
    });
  };

  return { execute };
});
```

**Reading a few customer fields before saving an order.** `lookupFields` costs 1 unit and avoids loading
the customer. Select fields come back as arrays of value/text pairs; the helper also accepts a single
object, because the source is inconsistent about that shape.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const firstSelected = (fieldValue) => {
    if (Array.isArray(fieldValue)) {
      return fieldValue.length ? fieldValue[0] : null;
    }
    return fieldValue || null;
  };

  const beforeSubmit = (context) => {
    const customerId = context.newRecord.getValue({ fieldId: 'entity' });
    if (!customerId) {
      return;
    }

    const info = search.lookupFields({
      type: search.Type.CUSTOMER,
      id: String(customerId),
      columns: ['daysoverdue', 'overduebalance', 'salesrep']
    });

    const rep = firstSelected(info.salesrep);
    if (Number(info.daysoverdue) > 60) {
      log.audit({
        title: 'Order for late payer',
        details: `Customer ${customerId} is ${info.daysoverdue} days overdue; rep ${rep ? rep.text : 'none'}`
      });
    }
  };

  return { beforeSubmit };
});
```

**Non-blocking lookup in a client script.** In the browser, the promise variant keeps the form
responsive while the lookup runs.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/search'], (search) => {
  const fieldChanged = (context) => {
    if (context.fieldId !== 'entity') {
      return;
    }
    const current = context.currentRecord;
    const customerId = current.getValue({ fieldId: 'entity' });
    if (!customerId) {
      return;
    }

    search.lookupFields.promise({
      type: search.Type.CUSTOMER,
      id: String(customerId),
      columns: ['overduebalance']
    }).then((info) => {
      const owed = Number(info.overduebalance) || 0;
      if (owed > 0) {
        current.setValue({ fieldId: 'memo', value: `Customer has ${owed.toFixed(2)} overdue` });
      }
    }).catch((err) => {
      console.error(`Overdue lookup failed: ${err.message}`);
    });
  };

  return { fieldChanged };
});
```
