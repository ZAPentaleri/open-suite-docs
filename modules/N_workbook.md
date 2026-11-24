# N/workbook
<!-- id: N/workbook -->

`N/workbook` lets a server script assemble SuiteAnalytics workbooks: flat table views, pivots and
charts, each sitting on a dataset from [N/dataset](N_dataset.md). You never hand the module one big
configuration object. Instead you build the workbook from small parts, each made by its own
`create*` factory (expressions, dimensions, measures, selectors, sorts, filters and styles), put
those parts into a table, pivot or chart, and pass those to `workbook.create`. You can also load an
existing workbook and run one of its pivots to get the computed cells back.

- **Availability:** server scripts only. The module is not supported in unauthenticated client
  contexts.
- **Permission:** the running user needs the SuiteAnalytics Workbook permission.
- **Feature:** the [Analytic APIs](../topics/analytic_apis.md) as a whole require the feature
  "SuiteAnalytics Workbook" to be enabled.
- **Dataset first:** every workbook needs a dataset. The dataset is set per visualization (each
  table, pivot and chart names its own), not on the workbook, so one workbook can mix
  visualizations over different datasets. A visualization can only use fields that exist in its
  dataset. The module depends on `N/dataset`.
- **Governance:** the factory functions are documented as free (0 units). `workbook.loadWorkbook`,
  `workbook.list` and `workbook.listPaged` cost 10 units each, and `Workbook.runPivot` costs 10
  units for every intersection it returns.
- **Load it as:** `define(['N/workbook'], (workbook) => { ... })`
- Related: [SuiteScript Analytic APIs](../topics/analytic_apis.md),
  [Workbook API workbook concepts](../topics/analytic_apis.workbooks.md),
  [Workbook tutorial](../topics/analytic_apis.tutorial_workbook.md) and its
  [pivot steps](../topics/analytic_apis.tutorial_workbook.pivot.md),
  [Workbook Builder Plug-in](../topics/workbook_builder_plugin.md) and its
  [context](../topics/workbook_builder_plugin.context.md), [N/dataset](N_dataset.md),
  [N/datasetLink](N_datasetLink.md), [N/query](N_query.md) (its `Operator` and `SortLocale` enums are
  used here).

**Where analytic names come from.** The Analytic APIs read the analytics data source, which is not
the same as the search data source. Look up record and field names for datasets in the Records
Catalog; the Records Browser describes the search data source. See
[SuiteScript Analytic APIs](../topics/analytic_apis.md).

**Getting a script-built workbook into the account.** The source documents no method that saves a
`Workbook` object: `workbook.create` returns an object and nothing says it is persisted. The route
the source does document is the [Workbook Builder Plug-in](../topics/workbook_builder_plugin.md):

- Developing one requires the features "Server SuiteScript" and "Workbook API" to be enabled in the
  development account.
- The plug-in script (SuiteScript 2.0 or 2.1, `@NScriptType workbookbuilderplugin`) implements one
  interface function, `createWorkbook(context)`, which returns nothing. It builds a workbook with
  this module and assigns it to `context.workbook`. Leaving that property unassigned raises
  `NO_WORKBOOK_CREATED`; assigning something that is not a `Workbook` raises
  `WRONG_PARAMETER_TYPE`.
- `context.name` and `context.description` (strings) are the name and description entered on the
  plug-in implementation; `context.owner` and `context.role` (numbers) are the internal IDs of the
  user who added it and of that user's role. All four are read-only; assigning one raises
  `READ_ONLY`.
- A plug-in can only reference datasets that have been saved (through the workbook UI or a Dataset
  Builder plug-in). Referencing an unsaved one raises `CAN_REFERENCE_ONLY_PERSISTED_DATASET`.
- The plug-in lets a workbook depend on who is looking at it (user- or role-specific data, custom
  logic, a subset of data). Workbooks made directly with this module are static: they know nothing
  about the runtime user or role.
- A plug-in run is limited to 1,000 usage units, values longer than a field's character limit are
  truncated, and the plug-in code cannot use a JavaScript `switch`/`case` statement.
- A workbook that is invalid because of bad syntax or logic still shows in the workbook list but
  cannot be opened (the UI reports that the workbook does not exist or is no longer available).
  Logic errors in a syntactically valid workbook can also make pivots or charts render wrongly. The
  source recommends running the creation code in the SuiteScript debugger first, and checking the
  logic there by running the visualizations (for pivots, with `Workbook.runPivot`).

**How the pieces fit**

| You want | Build it with |
|---|---|
| A value or formula | `createExpression`, `createConstant`, `createTranslation`, or `Dataset.getExpressionFromColumn` in N/dataset |
| A table view | `createTable` ← `createTableColumn` ← `createSort`, `createConditionalFormat` ← `createConditionalFormatRule` ← `createTableColumnFilter` + `createStyle` |
| A pivot | `createPivot` ← two `createPivotAxis` ← `createDataDimension` / `createSection` ← `createDataDimensionItem`; measures from `createDataMeasure` / `createCalculatedMeasure` |
| Pivot sorting | `createSortDefinition` ← `createSortByDataDimensionItem` / `createSortByMeasure` ← `createSort` |
| Pivot filtering | `createLimitingFilter`, `createConditionalFilter`, plus `filterExpressions` |
| Pivot cell styling | `createReportStyle` ← `createReportStyleRule` + `createMeasureValueSelector` ← `createMeasureSelector` |
| A chart | `createChart` ← `createCategory`, `createLegend`, `createSeries` ← `createAspect`, `createChartAxis` |
| Results | `Workbook.runPivot` → `PivotIntersection` → `DataDimensionValue` / `SectionValue` / `MeasureValue` |

## Members

| Member | Kind | Summary |
|---|---|---|
| [`workbook.create(options)`](#workbookcreateoptions) | function | Assemble a Workbook from tables, pivots and charts |
| [`workbook.loadWorkbook(options)`](#workbookloadworkbookoptions) | function | Load an existing workbook (10 units) |
| [`workbook.list()`](#workbooklist) | function | List all workbooks (10 units) |
| [`workbook.listPaged(options)`](#workbooklistpagedoptions) | function | Paged workbook metadata (10 units) |
| [`workbook.createTable(options)`](#workbookcreatetableoptions) | function | Table view over a dataset |
| [`workbook.createTableColumn(options)`](#workbookcreatetablecolumnoptions) | function | One column of a table view |
| [`workbook.createTableColumnFilter(options)`](#workbookcreatetablecolumnfilteroptions) | function | Operator + values test on a column |
| [`workbook.createTableColumnCondition(options)`](#workbookcreatetablecolumnconditionoptions) | function | Combine column filters under an operator |
| [`workbook.createFieldContext(options)`](#workbookcreatefieldcontextoptions) | function | How a column's field value is presented |
| [`workbook.createSort(options)`](#workbookcreatesortoptions) | function | Sort settings (direction, nulls, case, locale) |
| [`workbook.createConditionalFormat(options)`](#workbookcreateconditionalformatoptions) | function | Group of conditional format rules |
| [`workbook.createConditionalFormatRule(options)`](#workbookcreateconditionalformatruleoptions) | function | Filter + style pair |
| [`workbook.createPivot(options)`](#workbookcreatepivotoptions) | function | Pivot over a dataset |
| [`workbook.createPivotAxis(options)`](#workbookcreatepivotaxisoptions) | function | Row or column axis of a pivot |
| [`workbook.createDataDimension(options)`](#workbookcreatedatadimensionoptions) | function | Grouping dimension |
| [`workbook.createDataDimensionItem(options)`](#workbookcreatedatadimensionitemoptions) | function | One grouping expression in a dimension |
| [`workbook.createSection(options)`](#workbookcreatesectionoptions) | function | Container of dimensions, measures and sections |
| [`workbook.createDataMeasure(options)`](#workbookcreatedatameasureoptions) | function | Aggregated measure |
| [`workbook.createCalculatedMeasure(options)`](#workbookcreatecalculatedmeasureoptions) | function | Measure computed by an expression |
| [`workbook.createMeasureSelector(options)`](#workbookcreatemeasureselectoroptions) | function | Pick measures |
| [`workbook.createMeasureValueSelector(options)`](#workbookcreatemeasurevalueselectoroptions) | function | Pick pivot cells for styling |
| [`workbook.createDimensionSelector(options)`](#workbookcreatedimensionselectoroptions) | function | Point at a dimension or section |
| [`workbook.createPathSelector(options)`](#workbookcreatepathselectoroptions) | function | Path through nested dimensions |
| [`workbook.createSortDefinition(options)`](#workbookcreatesortdefinitionoptions) | function | Sorting for a pivot axis |
| [`workbook.createSortByDataDimensionItem(options)`](#workbookcreatesortbydatadimensionitemoptions) | function | Sort by a dimension item |
| [`workbook.createSortByMeasure(options)`](#workbookcreatesortbymeasureoptions) | function | Sort by a measure |
| [`workbook.createLimitingFilter(options)`](#workbookcreatelimitingfilteroptions) | function | Top-N style filter |
| [`workbook.createConditionalFilter(options)`](#workbookcreateconditionalfilteroptions) | function | Predicate filter on aggregated values |
| [`workbook.createReportStyle(options)`](#workbookcreatereportstyleoptions) | function | Pivot cell styling |
| [`workbook.createReportStyleRule(options)`](#workbookcreatereportstyleruleoptions) | function | Boolean expression + style |
| [`workbook.createChart(options)`](#workbookcreatechartoptions) | function | Chart over a dataset |
| [`workbook.createChartAxis(options)`](#workbookcreatechartaxisoptions) | function | Titled chart axis |
| [`workbook.createCategory(options)`](#workbookcreatecategoryoptions) | function | Chart category (x grouping) |
| [`workbook.createLegend(options)`](#workbookcreatelegendoptions) | function | Chart legend |
| [`workbook.createSeries(options)`](#workbookcreateseriesoptions) | function | Chart series |
| [`workbook.createAspect(options)`](#workbookcreateaspectoptions) | function | How a measure appears in a series |
| [`workbook.createExpression(options)`](#workbookcreateexpressionoptions) | function | Generic expression by function ID |
| [`workbook.createConstant(options)`](#workbookcreateconstantoptions) | function | Constant expression |
| [`workbook.createTranslation(options)`](#workbookcreatetranslationoptions) | function | Translated-string expression |
| [`workbook.createCurrency(options)`](#workbookcreatecurrencyoptions) | function | Currency amount value |
| [`workbook.createDuration(options)`](#workbookcreatedurationoptions) | function | Duration value |
| [`workbook.createRange(options)`](#workbookcreaterangeoptions) | function | Date or date-time range value |
| [`workbook.createSimpleRecordKey(options)`](#workbookcreatesimplerecordkeyoptions) | function | Single-value record key |
| [`workbook.createComplexRecordKey(options)`](#workbookcreatecomplexrecordkeyoptions) | function | Multi-property record key |
| [`workbook.createStyle(options)`](#workbookcreatestyleoptions) | function | Visual style for formatting |
| [`workbook.createColor(options)`](#workbookcreatecoloroptions) | function | RGBA color object |
| [`workbook.createFontSize(options)`](#workbookcreatefontsizeoptions) | function | Font size with a unit |
| [`workbook.createPositionPercent(options)`](#workbookcreatepositionpercentoptions) | function | Background position in percent |
| [`workbook.createPositionUnits(options)`](#workbookcreatepositionunitsoptions) | function | Background position in CSS-like units |
| [`workbook.createPositionValues(options)`](#workbookcreatepositionvaluesoptions) | function | Background position by keywords |
| [`Workbook`](#workbook-object) | object | A workbook; has `runPivot` |
| [`Table`](#table-object), [`TableColumn`](#tablecolumn-object), [`TableColumnFilter`](#tablecolumnfilter-object), [`TableColumnCondition`](#tablecolumncondition-object), [`FieldContext`](#fieldcontext-object), [`Sort`](#sort-object), [`ConditionalFormat`](#conditionalformat-object), [`ConditionalFormatRule`](#conditionalformatrule-object) | objects | Table view parts |
| [`Pivot`](#pivot-object), [`PivotAxis`](#pivotaxis-object), [`DataDimension`](#datadimension-object), [`DataDimensionItem`](#datadimensionitem-object), [`Section`](#section-object), [`DataMeasure`](#datameasure-object), [`CalculatedMeasure`](#calculatedmeasure-object), [`MeasureSelector`](#measureselector-object), [`MeasureValueSelector`](#measurevalueselector-object), [`DimensionSelector`](#dimensionselector-object), [`PathSelector`](#pathselector-object), [`DescendantorSelfNodesSelector`](#descendantorselfnodesselector-object), [`ChildNodesSelector`](#childnodesselector-object), [`SortDefinition`](#sortdefinition-object), [`SortByDataDimensionItem`](#sortbydatadimensionitem-object), [`SortByMeasure`](#sortbymeasure-object), [`LimitingFilter`](#limitingfilter-object), [`ConditionalFilter`](#conditionalfilter-object), [`ReportStyle`](#reportstyle-object), [`ReportStyleRule`](#reportstylerule-object) | objects | Pivot parts |
| [`Chart`](#chart-object), [`ChartAxis`](#chartaxis-object), [`Category`](#category-object), [`Legend`](#legend-object), [`Series`](#series-object), [`Aspect`](#aspect-object) | objects | Chart parts |
| [`Expression`](#expression-object), [`Currency`](#currency-object), [`Duration`](#duration-object), [`Range`](#range-object), [`Record`](#record-object), [`RecordKey`](#recordkey-object) | objects | Expressions and typed values |
| [`Style`](#style-object), [`Color`](#color-object), [`FontSize`](#fontsize-object), [`PositionPercent`](#positionpercent-object), [`PositionUnits`](#positionunits-object), [`PositionValues`](#positionvalues-object) | objects | Styling |
| [`PivotIntersection`](#pivotintersection-object), [`DataDimensionValue`](#datadimensionvalue-object), [`DataDimensionItemValue`](#datadimensionitemvalue-object), [`SectionValue`](#sectionvalue-object), [`MeasureValue`](#measurevalue-object) | objects | Pivot results |
| [`workbook.Aggregation`](#workbookaggregation), [`AspectType`](#workbookaspecttype), [`ChartType`](#workbookcharttype), [`ConstantType`](#workbookconstanttype), [`DateTimeHierarchy`](#workbookdatetimehierarchy), [`DateTimeProperty`](#workbookdatetimeproperty), [`ExpressionType`](#workbookexpressiontype), [`Stacking`](#workbookstacking), [`TotalLine`](#workbooktotalline), [`Color`](#workbookcolor), [`FontSize`](#workbookfontsize), [`FontStyle`](#workbookfontstyle), [`FontWeight`](#workbookfontweight), [`Image`](#workbookimage), [`Position`](#workbookposition), [`TemporalUnit`](#workbooktemporalunit), [`TextAlign`](#workbooktextalign), [`TextDecorationLine`](#workbooktextdecorationline), [`TextDecorationStyle`](#workbooktextdecorationstyle), [`Unit`](#workbookunit) | enums | Fixed value lists |

## Functions

### workbook.create(options)
<!-- id: N/workbook.create -->

Assembles a `Workbook` object. A workbook carries an ID, a name, a description and lists of pivots,
tables and charts. Every workbook is based on a dataset, but `create` takes no dataset option of its
own: each table, pivot and chart you pass in names its dataset. The concept material ties the
`name` and `description` options to the Workbook Builder plug-in case (there, the name shown in the
UI comes from the plug-in implementation record, see [`Workbook.name`](#workbookname)).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.charts` | workbook.Chart[] | No | |
| `options.description` | string | No | |
| `options.name` | string | No | |
| `options.pivots` | workbook.Pivot[] | No | |
| `options.tables` | workbook.Table[] | No | |

- `options.charts`: charts built with [`workbook.createChart`](#workbookcreatechartoptions).

> [!SOURCE-ERROR] The source's description of the `charts` parameter is cut off partway through, so
> its full meaning is missing. Rely on the declared type (an array of `Chart` objects) and verify the
> behaviour in your account.
<!-- source-error: N/workbook.create | - | charts_parameter_description_truncated -->

- `options.pivots`: pivots from [`workbook.createPivot`](#workbookcreatepivotoptions).
- `options.tables`: table views from [`workbook.createTable`](#workbookcreatetableoptions).

**Returns:** `workbook.Workbook`, the assembled workbook.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

The spec says `Workbook.id` is set when the workbook is created, but `create` documents no `id`
option, and the source does not state where the ID comes from. One Workbook Builder plug-in sample
passes an `id` to `create` anyway; the source flags that sample as using an option missing from the
documented parameters, so don't treat it as confirmed. The source also documents no way to save the
returned object; see "Getting a script-built workbook into the account" above.

### workbook.loadWorkbook(options)
<!-- id: N/workbook.loadWorkbook -->

Loads a workbook that already exists, by its ID. Use it to get a `Workbook` whose pivots you want to
run.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `workbook.Workbook`, the existing workbook.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.list()
<!-- id: N/workbook.list -->

Returns every workbook in the account. It takes no parameters.

**Returns:** `Object[]`, one entry per workbook. The source does not state which fields each entry
has.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.listPaged(options)
<!-- id: N/workbook.listPaged -->

Returns workbook metadata a page at a time. It is the paged counterpart of `workbook.list`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.category` | string | No | |
| `options.pageSize` | number | Yes | |

- `options.category`: must be a value of the owner-category enum, otherwise the call fails. The
  source does not list the owner-category values or say which module defines that enum.
- `options.pageSize`: number of entries per page. The source gives no limits.

**Returns:** `PagedInfoData`, paged workbook metadata. The source does not define this type's
members.

**Errors:**

- `INVALID_OWNER_CATEGORY`: `options.category` is not a valid owner category.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createTable(options)
<!-- id: N/workbook.createTable -->

Builds a table view, which is a flat list of dataset columns. Pass the result to
[`workbook.create`](#workbookcreateoptions) in `options.tables`. The concept material names
`columns`, `dataset`, `id` and `name` as the required inputs, matching the table below.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string \| workbook.Expression | Yes | |
| `options.columns` | workbook.TableColumn[] | Yes | |
| `options.dataset` | dataset.Dataset | Yes | |
| `options.id` | string | Yes | |

- `options.name`: a plain string, or an `Expression` (for example a translation from
  [`workbook.createTranslation`](#workbookcreatetranslationoptions)).
- `options.columns`: columns from [`workbook.createTableColumn`](#workbookcreatetablecolumnoptions).
- `options.dataset`: the dataset whose columns the table shows. In a Workbook Builder plug-in it must
  be a saved dataset.

**Returns:** `workbook.Table`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createTableColumn(options)
<!-- id: N/workbook.createTableColumn -->

Defines one column of a table view. The column shows a dataset column, picked by its alias, and can
carry a label, a width, a sort, a filter condition and conditional formats. Used by
[`workbook.createTable`](#workbookcreatetableoptions). The column alias must already be defined in
the dataset. The documented order for conditional formatting is: build a column filter (operator
plus values), build a style, pair them in a conditional format rule, group rules in a conditional
format, then pass that to this function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.alias` | string | No | |
| `options.label` | string \| workbook.Expression | No | |
| `options.width` | number | No | |
| `options.condition` | workbook.ConditionalFilter | No | |
| `options.conditionalFormats` | workbook.ConditionalFormat[] | Yes | |
| `options.datasetColumnAlias` | string | Yes | |
| `options.sort` | workbook.Sort | No | |

- `options.alias`: an alias used to map results back to this column.
- `options.label`: a string or an `Expression`.
- `options.width`: in pixels.
- `options.condition`: a filter from
  [`workbook.createConditionalFilter`](#workbookcreateconditionalfilteroptions).
- `options.conditionalFormats`: formats from
  [`workbook.createConditionalFormat`](#workbookcreateconditionalformatoptions). Marked required;
  the source does not say whether an empty array is accepted.
- `options.datasetColumnAlias`: the alias of the dataset column this table column shows.
- `options.sort`: from [`workbook.createSort`](#workbookcreatesortoptions).

> [!SOURCE-ERROR] The source's own syntax sample for this function sets a field context, yet the
> parameter table has no field-context option; and the same sample leaves out `conditionalFormats`,
> which the table marks required. Follow the parameter table (always pass `conditionalFormats`) and
> verify whether a field context can be attached here.
<!-- source-error: N/workbook.createTableColumn | - | param_table_omits_fieldcontext_used_in_syntax_sample -->
<!-- source-error: N/workbook.createTableColumn | - | required_conditionalformats_omitted_in_syntax_sample -->

**Returns:** `workbook.TableColumn`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

The spec says `FieldContext` objects are used by this function, and `TableColumn` has a
`fieldContext` property, but this function lists no `fieldContext` option. The source does not
state how to attach a field context here. Likewise, the `TableColumn` object has no properties for
the `condition` and `conditionalFormats` you pass in.

### workbook.createTableColumnFilter(options)
<!-- id: N/workbook.createTableColumnFilter -->

Creates a filter that tests a table column's value with an operator and a list of values. It is the
`filter` half of a conditional format rule
([`workbook.createConditionalFormatRule`](#workbookcreateconditionalformatruleoptions)).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.operator` | string | Yes | |
| `options.values` | Array<null \| Object \| number \| string \| boolean \| Date> | No | |

- `options.operator`: an N/query [`Operator`](N_query.md) value, passed as its string value (for
  example `query.Operator.GREATER`). The concept material says the allowed operators are the
  `query.Operator` values or their strings.
- `options.values`: entries can be `null`, objects, numbers, strings, booleans or dates. Which ones
  make sense depends on the column and the operator.

**Returns:** `workbook.TableColumnFilter`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createTableColumnCondition(options)
<!-- id: N/workbook.createTableColumnCondition -->

Combines several table column filters under one operator into a table column condition.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filters` | workbook.TableColumnFilter[] | No | |
| `options.operator` | string | Yes | |

- `options.operator`: the source does not list the operator strings this function accepts (the
  `query.Operator` list is documented for column filters, not for conditions).

**Returns:** declared as `workbook.TableColumnFilter`, though the description says the function
produces a [`TableColumnCondition`](#tablecolumncondition-object).

> [!SOURCE-ERROR] The source gives the return type as `TableColumnFilter`, while its own description
> says a `TableColumnCondition` comes back. Expect a `TableColumnCondition`, and check the object
> you actually receive.
<!-- source-error: N/workbook.createTableColumnCondition | - | return_type_listed_as_table_column_filter -->

**Errors:**

- `INVALID_OPERATOR`: `options.operator` is not a valid operator.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.1

### workbook.createFieldContext(options)
<!-- id: N/workbook.createFieldContext -->

Creates a field context, which controls how a table column's field value is presented. The spec
lists [`workbook.createTableColumn`](#workbookcreatetablecolumnoptions) as its consumer.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.parameters` | Object | No | |
| `options.name` | string | Yes | |

- `options.parameters`: a plain key/value object.
- `options.name`: the context name. See [`FieldContext.name`](#fieldcontextname) for the known
  values.

**Returns:** `workbook.FieldContext`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createSort(options)
<!-- id: N/workbook.createSort -->

Creates sort settings. You pass them to a table column, or to the pivot sorts
([`createSortByDataDimensionItem`](#workbookcreatesortbydatadimensionitemoptions),
[`createSortByMeasure`](#workbookcreatesortbymeasureoptions)).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.ascending` | boolean | No | `true` |
| `options.locale` | query.SortLocale | No | |
| `options.nullsLast` | boolean | No | |
| `options.caseSensitive` | boolean | No | |

- `options.ascending`: `true` sorts ascending and `false` sorts descending.
- `options.locale`: a value from N/query's `SortLocale` enum. Any other value raises
  `INVALID_SORT_LOCALE`.
- `options.nullsLast`: if omitted, it takes the same value as `options.ascending`. Ascending sorts
  therefore put nulls last by default, and descending sorts put them first.
- `options.caseSensitive`: whether letter case is taken into account when ordering.

**Returns:** `workbook.Sort`

**Errors:**

- `INVALID_SORT_LOCALE`: `options.locale` is not a `query.SortLocale` value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

The `Sort` object has an `order` property (since 2022.2), but this function lists no `order`
option. The source does not state how `order` is set.

### workbook.createConditionalFormat(options)
<!-- id: N/workbook.createConditionalFormat -->

Groups conditional format rules so a table column can apply them
(`createTableColumn`'s `options.conditionalFormats`). When several rules match a value, the first
matching rule wins: in the tutorial, a "yellow below 10" rule listed before a "pink at 0 or more"
rule paints the value 5 yellow. Order the rules from most to least specific.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.rules` | workbook.ConditionalFormatRule[] | Yes | |

**Returns:** `workbook.ConditionalFormat`

**Errors:**

- `NO_RULE_DEFINED`: `options.rules` is missing or empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createConditionalFormatRule(options)
<!-- id: N/workbook.createConditionalFormatRule -->

Pairs a filter (which cells match) with a style (how matching cells look, for example cell
background, font size or font style). Used by
[`workbook.createConditionalFormat`](#workbookcreateconditionalformatoptions). The concept material
names `filter` and `style` as the required inputs.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filter` | workbook.TableColumnFilter | Yes | |
| `options.style` | workbook.Style | Yes | |

**Returns:** `workbook.ConditionalFormatRule`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createPivot(options)
<!-- id: N/workbook.createPivot -->

Builds a pivot: a cross-tabulation of a dataset with a row axis and a column axis. It can also
carry aggregation filters, filter expressions and report styles. A pivot holds an ID, a name, both
axes, its filters and its filter expressions. Pass it to `workbook.create` in `options.pivots`, and
run it later with [`Workbook.runPivot`](#workbookrunpivotoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.aggregationFilters` | Array<workbook.ConditionalFilter \| workbook.LimitingFilter> | No | |
| `options.columnAxis` | workbook.PivotAxis | Yes | |
| `options.dataset` | dataset.Dataset | Yes | |
| `options.filterExpressions` | workbook.Expression[] | No | |
| `options.rowAxis` | workbook.PivotAxis | Yes | |
| `options.reportStyles` | workbook.ReportStyle | Yes | |
| `options.id` | string | Yes | |
| `options.name` | string | Yes | |

- `options.aggregationFilters`: limiting or conditional filters. When you give several, they are
  combined.
- `options.columnAxis`: the horizontal (x) axis.
- `options.dataset`: only this dataset's fields can be used in the pivot.
- `options.filterExpressions`: filters that act on the raw, non-aggregated values.
- `options.rowAxis`: the vertical (y) axis.

> [!SOURCE-ERROR] The source's description of `rowAxis` talks about the fields used on the column
> axis, which looks like text copied from `columnAxis`. Treat `rowAxis` as the row (y) axis, as its
> name and the axis data say, and verify the layout of the result.
<!-- source-error: N/workbook.createPivot | options.rowAxis | row_axis_description_says_fields_used_on_column_axis -->

- `options.reportStyles`: declared here as a single `ReportStyle`.

> [!SOURCE-ERROR] This parameter is typed as one `ReportStyle`, while
> [`Pivot.reportStyles`](#pivotreportstyles) is an array of them. The spec does not say which form
> is accepted; passing an array matches the property, but verify.
<!-- source-error: N/workbook.createPivot | options.reportStyles | createPivot_reportStyles_single_vs_property_array -->

> [!SOURCE-ERROR] The concept material says a pivot takes either a `dataset` or a `datasetLink`
> (one or the other, never both), and the `Pivot` object has a `datasetLink` property, but this
> reference lists no `datasetLink` option and makes `dataset` required. The concept material's list
> of required inputs (`columnAxis`, the dataset or dataset link, `id` and `name`) also leaves out
> `rowAxis` and `reportStyles`, which the table above marks required. Test which inputs your account
> really insists on.
<!-- source-error: N/workbook.createPivot | - | datasetLink_missing_vs_concept_page -->
<!-- source-error: N/workbook.createPivot | - | required_rowAxis_reportStyles_vs_concept_page -->

> [!SOURCE-ERROR] The `Pivot` object and the concept material know a portlet name for pivots, but
> this function's parameter list has no `portletName` option. The source does not say whether you
> can pass one here; verify before relying on it.
<!-- source-error: N/workbook.createPivot | - | portletName_missing_vs_concept_page -->

> [!SOURCE-ERROR] The source's syntax samples for this function never pass `reportStyles`, even
> though the parameter is marked required, and one sample is not even valid code (a `name` string is
> never closed). Don't copy those samples; follow the parameter table and check what is enforced.
<!-- source-error: N/workbook.createPivot | - | reportStyles_required_but_omitted_in_all_syntax_samples -->
<!-- source-error: N/workbook.createPivot | - | syntax_sample_has_unclosed_name_string -->

**Returns:** `workbook.Pivot`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createPivotAxis(options)
<!-- id: N/workbook.createPivotAxis -->

Creates one axis of a pivot. The axis is rooted either in a single data dimension or, when the axis
needs several, in a section, and it can have sort definitions. A sort definition can only refer to
items of the root dimension.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.root` | workbook.DataDimension \| workbook.Section | Yes | |
| `options.sortDefinitions` | workbook.SortDefinition[] | No | |

- `options.sortDefinitions`: optional here, but the pivot tutorial says an axis needs both a root
  and a sort definition; that disagreement is flagged on the
  [tutorial page](../topics/analytic_apis.tutorial_workbook.pivot.md). Supplying one is the safe
  choice.

**Returns:** `workbook.PivotAxis`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createDataDimension(options)
<!-- id: N/workbook.createDataDimension -->

Creates a data dimension, the grouping that pivot axes, dimension selectors and sections are built
from. Its items say what to group by (for example transaction type by posting period). Its children
nest further dimensions, sections or measures beneath it. The dimension's label is what the workbook
UI shows. The documented order is: create the dimension items, combine them into a dimension, then
use the dimension in the pivot axes.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.children` | Array<workbook.DataDimension \| workbook.Section \| workbook.CalculatedMeasure \| workbook.DataMeasure> | No | |
| `options.totalLine` | workbook.TotalLine | No | |
| `options.items` | workbook.DataDimensionItem[] | Yes | |

- `options.children`: data dimensions, sections, calculated measures or data measures.
- `options.totalLine`: where the total line goes (first line, last line or hidden). Takes a
  [`workbook.TotalLine`](#workbooktotalline) value.
- `options.items`: at least one item from
  [`workbook.createDataDimensionItem`](#workbookcreatedatadimensionitemoptions).

**Returns:** `workbook.DataDimension`

**Errors:**

- `INVALID_TOTAL_LINE`: `options.totalLine` is not a `TotalLine` value.
- `NO_DIMENSION_ITEM_DEFINED`: `options.items` is missing or empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createDataDimensionItem(options)
<!-- id: N/workbook.createDataDimensionItem -->

Creates one item of a data dimension: an expression to group by, with an optional label. The
expression is usually a column expression from N/dataset's `Dataset.getExpressionFromColumn`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.expression` | workbook.Expression | Yes | |
| `options.label` | string | No | |

**Returns:** `workbook.DataDimensionItem`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createSection(options)
<!-- id: N/workbook.createSection -->

Creates a section, which is a container for data dimensions, measures and other sections. Use it as
an axis root when the axis needs more than one dimension.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.children` | Array<workbook.DataDimension \| workbook.CalculatedMeasure \| workbook.DataMeasure \| workbook.Section> | Yes | |
| `options.totalLine` | workbook.TotalLine | No | |

- `options.children`: data dimensions, sections, calculated measures or data measures.
- `options.totalLine`: a [`workbook.TotalLine`](#workbooktotalline) value.

**Returns:** `workbook.Section`

**Errors:**

- `INVALID_TOTAL_LINE`: `options.totalLine` is not a `TotalLine` value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createDataMeasure(options)
<!-- id: N/workbook.createDataMeasure -->

Creates an aggregated measure, such as a sum or a count of unique values over dataset values. A
measure is built from one expression or from several: give exactly one of `expression` (single
value) or `expressions` (several values); they cannot be used together.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.aggregation` | workbook.Aggregation | Yes | |
| `options.expression` | workbook.Expression | No | |
| `options.expressions` | workbook.Expression[] | No | |
| `options.label` | string \| workbook.Expression | No | |

- `options.aggregation`: a [`workbook.Aggregation`](#workbookaggregation) value.
- `options.expression`: conditionally required: needed for a single-expression measure.
- `options.expressions`: conditionally required: needed for a multi-expression measure.
- `options.label`: a string or an `Expression`.

**Returns:** `workbook.DataMeasure`

**Errors:**

- `AT_LEAST_ONE_EXPRESSION_IS_NEEDED`: neither `options.expression` nor a non-empty
  `options.expressions` was given.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `options.expression` and `options.expressions` were given.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createCalculatedMeasure(options)
<!-- id: N/workbook.createCalculatedMeasure -->

Creates a measure whose value is computed from other values, for example the difference of two
measures. At the top level, the expression must be a `PLUS`, `MINUS`, `DIVIDE` or `MULTIPLY`
expression; other expression types can appear as operands nested inside it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.label` | string \| workbook.Expression | No | |
| `options.expression` | workbook.Expression | Yes | |

**Returns:** `workbook.CalculatedMeasure`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createMeasureSelector(options)
<!-- id: N/workbook.createMeasureSelector -->

Selects one or more measures. Used inside a measure value selector.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.measures` | Array<workbook.CalculatedMeasure \| workbook.DataMeasure> | Yes | |

**Returns:** `workbook.MeasureSelector`

**Errors:**

- `NO_MEASURE_DEFINED`: `options.measures` is missing or empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createMeasureValueSelector(options)
<!-- id: N/workbook.createMeasureValueSelector -->

Picks pivot cells by row position, column position and measure, so that a report style can target
them. A report style needs one. In the tutorial, the row and column selectors are
`DescendantorSelfNodesSelector` [sic] values, combined with a measure selector for the measure.
Used by [`workbook.createReportStyle`](#workbookcreatereportstyleoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.columnSelector` | workbook.DescendantorSelfNodesSelector \| workbook.DimensionSelector \| workbook.PathSelector | Yes | |
| `options.rowSelector` | workbook.DescendantorSelfNodesSelector \| workbook.DimensionSelector \| workbook.PathSelector | Yes | |
| `options.measureSelector` | workbook.MeasureSelector | Yes | |

**Returns:** `workbook.MeasureValueSelector`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createDimensionSelector(options)
<!-- id: N/workbook.createDimensionSelector -->

Creates a selector that points at a data dimension or a section. Filters, sorts and measure value
selectors use it to say which part of an axis they refer to.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.dimension` | workbook.DataDimension \| workbook.Section | Yes | |

**Returns:** `workbook.DimensionSelector`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createPathSelector(options)
<!-- id: N/workbook.createPathSelector -->

Creates a selector that walks a path of dimension selectors, much like an XPath, to reach a nested
position on an axis.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.elements` | workbook.DimensionSelector[] | Yes | |

**Returns:** `workbook.PathSelector`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createSortDefinition(options)
<!-- id: N/workbook.createSortDefinition -->

Says how the part of a pivot axis picked by `selector` is ordered. Pass the result to
[`workbook.createPivotAxis`](#workbookcreatepivotaxisoptions) in `sortDefinitions`. It may only
refer to items of the axis's root dimension.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.selector` | workbook.DimensionSelector \| workbook.PathSelector | Yes | |
| `options.sortBys` | Array<workbook.SortByDataDimensionItem \| workbook.SortByMeasure> | Yes | |

**Returns:** `workbook.SortDefinition`

**Errors:**

- `NO_SORT_BY_DEFINED`: `options.sortBys` is missing or empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

A Workbook Builder plug-in sample passes a `row` option to this function; the source flags that as
an option absent from the documented parameters.

### workbook.createSortByDataDimensionItem(options)
<!-- id: N/workbook.createSortByDataDimensionItem -->

Sorts by the values of one data dimension item.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.item` | workbook.DataDimensionItem | Yes | |
| `options.sort` | workbook.Sort | Yes | |

**Returns:** `workbook.SortByDataDimensionItem`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createSortByMeasure(options)
<!-- id: N/workbook.createSortByMeasure -->

Sorts an axis by the values of a measure.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.measure` | workbook.CalculatedMeasure \| workbook.DataMeasure | Yes | |
| `options.otherAxisSelector` | Array< workbook.DescendantorSelfNodesSelector \| workbook.DimensionSelector \| workbook.PathSelector> | Yes | |
| `options.selector` | Array< workbook.DescendantorSelfNodesSelector \| workbook.DimensionSelector \| workbook.PathSelector> | Yes | |
| `options.sort` | workbook.Sort | Yes | |

- `options.otherAxisSelector`: a selector on the axis that is *not* being sorted.

> [!SOURCE-ERROR] Both selector options are typed as arrays, but the source's own sample passes
> single selectors, [`SortByMeasure.otherAxisSelector`](#sortbymeasureotheraxisselector) is a
> single selector, and the `SortByMeasure` object has no `selector` property at all. The spec does
> not settle which shape is correct; try a single selector first and verify.
<!-- source-error: N/workbook.createSortByMeasure | - | params_typed_array_but_sample_and_property_single -->

**Returns:** `workbook.SortByMeasure`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createLimitingFilter(options)
<!-- id: N/workbook.createLimitingFilter -->

Creates a filter that keeps only a limited number of nodes on one axis, after ordering them by the
given sorts (a "top N" filter). Used in `createPivot`'s `aggregationFilters`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filteredNodesSelector` | workbook.DimensionSelector \| workbook.PathSelector | Yes | |
| `options.row` | boolean | Yes | |
| `options.sortBys` | Array<workbook.SortByDataDimensionItem \| workbook.SortByMeasure> | Yes | |
| `options.limit` | number | Yes | |

- `options.row`: `true` filters the row axis and `false` filters the column axis.

**Returns:** `workbook.LimitingFilter`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createConditionalFilter(options)
<!-- id: N/workbook.createConditionalFilter -->

Creates a filter that keeps the axis nodes for which a predicate expression over a measure holds.
Used in `createPivot`'s `aggregationFilters`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filteredNodesSelector` | workbook.DimensionSelector \| workbook.PathSelector | Yes | |
| `options.measure` | workbook.CalculatedMeasure \| workbook.DataMeasure | Yes | |
| `options.otherAxisSelector` | workbook.DimensionSelector \| workbook.PathSelector | Yes | |
| `options.row` | boolean | Yes | |
| `options.predicate` | workbook.Expression | Yes | |

- `options.row`: `true` filters the row axis and `false` filters the column axis.

**Returns:** `workbook.ConditionalFilter`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

The `ConditionalFilter` object also has `columnSelector` and `rowSelector` properties, but this
function has no options for them, and the source does not state how they get set.

### workbook.createReportStyle(options)
<!-- id: N/workbook.createReportStyle -->

Creates a report style for a pivot. The selectors pick the cells to style, and the rules decide
which style applies (the tutorial's example highlights cells whose measure exceeds a threshold).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.rules` | workbook.ReportStyleRule[] | Yes | |
| `options.selectors` | workbook.MeasureValueSelector[] | Yes | |

**Returns:** `workbook.ReportStyle`

**Errors:**

- `NO_RULE_DEFINED`: `options.rules` is missing or empty.
- `NO_SELECTORS_DEFINED`: `options.selectors` is missing or empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createReportStyleRule(options)
<!-- id: N/workbook.createReportStyleRule -->

Pairs a condition with a style. When the expression evaluates to true for a cell, the style applies.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.expression` | workbook.Expression | Yes | |
| `options.style` | workbook.Style | Yes | |

- `options.expression`: must be a boolean expression. Column expressions from
  `Dataset.getExpressionFromColumn` can be used here.

**Returns:** `workbook.ReportStyleRule`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createChart(options)
<!-- id: N/workbook.createChart -->

Builds a chart over a dataset. The spec names four chart kinds: line, bar, area and column (see
[`workbook.ChartType`](#workbookcharttype)). The chart groups data by a category, splits it by a
legend, and plots one or more series.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.aggregationFilters` | Array<workbook.ConditionalFilter \| workbook.LimitingFilter> | No | |
| `options.dataset` | dataset.Dataset | Yes | |
| `options.filterExpressions` | workbook.Expression[] | No | |
| `options.portletName` | string \| workbook.Expression | Yes | |
| `options.stacking` | workbook.Stacking | No | |
| `options.subTitle` | string \| workbook.Expression | No | |
| `options.title` | string \| workbook.Expression | No | |
| `options.type` | workbook.ChartType | Yes | |
| `options.series` | workbook.Series[] | Yes | |
| `options.category` | workbook.Category | Yes | |
| `options.datasetLink` | datasetLink.DatasetLink | Yes | |
| `options.id` | string | Yes | |
| `options.legend` | workbook.Legend | Yes | |
| `options.name` | string | Yes | |

- `options.aggregationFilters`: when you give several, they are combined.
- `options.dataset`: only this dataset's fields can be used.
- `options.filterExpressions`: filters on raw, non-aggregated values.
- `options.portletName`, `options.subTitle`, `options.title`: each takes a string or an
  `Expression`.
- `options.stacking`: a [`workbook.Stacking`](#workbookstacking) value.
- `options.type`: a [`workbook.ChartType`](#workbookcharttype) value.
- `options.series`: what to plot. A typical series is the sum of customer transaction amounts for
  each month.
- `options.category`: from [`workbook.createCategory`](#workbookcreatecategoryoptions).
- `options.datasetLink`: a dataset link from [N/datasetLink](N_datasetLink.md).
- `options.legend`: from [`workbook.createLegend`](#workbookcreatelegendoptions).

> [!SOURCE-ERROR] The source marks both `dataset` and `datasetLink` as required, which the spec flags
> as suspect. The spec does not say whether a chart really needs both or only one of them, so test
> with your own data source before relying on either reading.
<!-- source-error: N/workbook.createChart | - | dataset_and_datasetlink_both_required -->

**Returns:** `workbook.Chart`

**Errors:**

- `INVALID_CHART_TYPE`: `options.type` is not a `ChartType` value.
- `INVALID_STACKING_TYPE`: `options.stacking` is not a `Stacking` value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

The [Analytic APIs overview](../topics/analytic_apis.md) says charting is not supported, while this
function is documented; the source does not reconcile the two, so test charts in your account
before depending on them. The Workbook Builder plug-in sample does build a chart (category, legend
and a series with an aspect).

### workbook.createChartAxis(options)
<!-- id: N/workbook.createChartAxis -->

Creates a titled chart axis. The spec mentions x and y axes.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | Yes | |

**Returns:** `workbook.ChartAxis`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createCategory(options)
<!-- id: N/workbook.createCategory -->

Creates a chart category: the dimension or section the chart groups along an axis. Used by
[`workbook.createChart`](#workbookcreatechartoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.root` | workbook.DataDimension \| workbook.Section | Yes | |
| `options.axis` | workbook.ChartAxis | Yes | |
| `options.sortDefinitions` | workbook.SortDefinition[] | No | |

**Returns:** `workbook.Category`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createLegend(options)
<!-- id: N/workbook.createLegend -->

Creates a chart legend: the dimension or section the chart splits its data by. Used by
[`workbook.createChart`](#workbookcreatechartoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.root` | workbook.Section \| workbook.DataDimension | Yes | |
| `options.axes` | workbook.ChartAxis[] | Yes | |
| `options.sortDefinitions` | workbook.SortDefinition[] | No | |

**Returns:** `workbook.Legend`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createSeries(options)
<!-- id: N/workbook.createSeries -->

Creates a chart series from one or more aspects. Used by `createChart`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.aspects` | workbook.Aspect[] | Yes | |

**Returns:** `workbook.Series`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createAspect(options)
<!-- id: N/workbook.createAspect -->

Creates an aspect, which ties a measure to the way it shows in a series (as the plotted value or as
a color). Used by [`workbook.createSeries`](#workbookcreateseriesoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.measure` | workbook.CalculatedMeasure \| workbook.DataMeasure | Yes | |
| `options.type` | workbook.AspectType | No | |

- `options.type`: a [`workbook.AspectType`](#workbookaspecttype) value.

**Returns:** `workbook.Aspect`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### workbook.createExpression(options)
<!-- id: N/workbook.createExpression -->

The general way to build an expression. You choose a function from
[`workbook.ExpressionType`](#workbookexpressiontype) and supply its parameters by name. Expressions
feed pivots, dimension items, data measures and conditional filters, among others. There are two
kinds of expression: column expressions, obtained from N/dataset's
`Dataset.getExpressionFromColumn` (which takes either a column alias or a column ID, not both), and
custom expressions built with this function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.functionId` | workbook.ExpressionType | Yes | |
| `options.parameters` | Object | No | |

- `options.functionId`: a [`workbook.ExpressionType`](#workbookexpressiontype) value.
- `options.parameters`: the parameter names for each function are listed under
  [`workbook.ExpressionType`](#workbookexpressiontype). They must be spelled exactly as listed.

**Returns:** `workbook.Expression`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

For expressions that read a dataset field, the spec recommends getting them from
`Dataset.getExpressionFromColumn` instead of building `FIELD` expressions yourself.

### workbook.createConstant(options)
<!-- id: N/workbook.createConstant -->

Wraps a literal value as an expression, for comparisons and calculations.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.constant` | string \| number \| boolean \| Date | Yes | |
| `options.type` | workbook.ConstantType | No | |

- `options.constant`: the literal: a string, number, boolean or date.

> [!SOURCE-ERROR] The source describes `constant` as if it were a function ID, but it is typed and
> used as the constant value itself (string, number, boolean or Date). Treat it as the value.
<!-- source-error: N/workbook.createConstant | options.constant | described_as_function_id -->

- `options.type`: a [`workbook.ConstantType`](#workbookconstanttype) value. A JavaScript number
  doesn't show whether it is a decimal or an integer, so give the type when that distinction
  matters.

**Returns:** `workbook.Expression`, a constant expression.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

Always pass an options object: a Workbook Builder plug-in sample calls this function with a bare
number, and the source flags that sample as wrong. The `Currency` and `RecordKey` objects are said
to be usable here, but `options.constant` does not list them among its accepted types (see the
notes on [`Currency`](#currency-object) and [`RecordKey`](#recordkey-object)).

### workbook.createTranslation(options)
<!-- id: N/workbook.createTranslation -->

Creates an expression that resolves to a translated string, looked up by key in a translation
collection. Use it wherever a label or name accepts an `Expression`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | string | Yes | |
| `options.collection` | string | Yes | |

**Returns:** `workbook.Expression`, a translation expression.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createCurrency(options)
<!-- id: N/workbook.createCurrency -->

Creates a currency amount: a number paired with a currency code such as `USD`, `EUR` or `GBP`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.amount` | number | Yes | |

- `options.id`: a currency code.

**Returns:** `workbook.Currency`

**Errors:**

- `INVALID_CURRENCY`: `options.id` is not a valid currency code.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createDuration(options)
<!-- id: N/workbook.createDuration -->

Creates a duration value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.start` | string | Yes | |
| `options.end` | string | Yes | |

- `options.start`, `options.end`: values from [`workbook.TemporalUnit`](#workbooktemporalunit).

> [!SOURCE-ERROR] The source describes `start` and `end` as dates, yet types them as strings drawn
> from the `TemporalUnit` enum, and the error for a bad value is about temporal units. The spec does
> not settle which reading is right, so verify against a live call.
<!-- source-error: N/workbook.createDuration | options.start | described_as_date -->
<!-- source-error: N/workbook.createDuration | options.end | described_as_date -->

**Returns:** `workbook.Duration`

**Errors:**

- `INVALID_TEMPORAL_UNIT`: `options.start` or `options.end` is not a valid temporal unit.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createRange(options)
<!-- id: N/workbook.createRange -->

Creates a range value. Its bounds can be dates or date-times.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.start` | string | Yes | |
| `options.end` | string | Yes | |

- `options.start`: typed `string`.

> [!SOURCE-ERROR] The source's type cell for `start` carries a stray bracket; the spec normalized
> it to `string`. Pass the start bound as a string, the same as `end`, and verify.
<!-- source-error: N/workbook.createRange | options.start | type_cell_has_stray_bracket -->

**Returns:** `workbook.Range`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createSimpleRecordKey(options)
<!-- id: N/workbook.createSimpleRecordKey -->

Creates a record key from a single value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | string \| number | Yes | |

- `options.key`: a string or a number.

**Returns:** `workbook.RecordKey`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createComplexRecordKey(options)
<!-- id: N/workbook.createComplexRecordKey -->

Creates a record key from an object whose properties together make up the key.

> [!SOURCE-ERROR] The type cells in the source were damaged for this function, and the spec carries
> a repaired return type (`RecordKey`). Verify the result before relying on it.
<!-- source-error: N/workbook.createComplexRecordKey | - | source_type_cell_repaired -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |

- `options`: an object holding the key properties. The source does not state which property names
  are expected.

> [!SOURCE-ERROR] The type cell for `options` was also damaged in the source; the spec repaired it
> to `Object`.
<!-- source-error: N/workbook.createComplexRecordKey | options | source_type_cell_repaired -->

**Returns:** `workbook.RecordKey`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createStyle(options)
<!-- id: N/workbook.createStyle -->

Creates a visual style for conditional formatting: colors, background image and position, font and
text decoration. Use it in
[`createConditionalFormatRule`](#workbookcreateconditionalformatruleoptions) (tables) and
[`createReportStyleRule`](#workbookcreatereportstyleruleoptions) (pivots).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.backgroundColor` | workbook.Color \| string | No | |
| `options.backgroundImage` | string | No | |
| `options.backgroundPosition` | workbook.PositionPercent \| workbook.PositionUnits \| workbook.PositionValues | No | |
| `options.color` | workbook.Color \| string | No | |
| `options.fontSize` | workbook.FontSize | No | |
| `options.fontStyle` | workbook.FontStyle | No | |
| `options.fontWeight` | workbook.FontWeight | No | |
| `options.textAlign` | workbook.TextAlign | No | |
| `options.textDecorationColor` | workbook.Color \| string | No | |
| `options.textDecorationLine` | workbook.TextDecorationLine | No | |
| `options.textDecorationStyle` | workbook.TextDecorationStyle \| string | No | |

- `options.backgroundColor`, `options.color`, `options.textDecorationColor`: either a color object
  from [`workbook.createColor`](#workbookcreatecoloroptions), or a value of the
  [`workbook.Color`](#workbookcolor) enum.
- `options.backgroundImage`: a [`workbook.Image`](#workbookimage) value.
- `options.backgroundPosition`: a position object from `createPositionPercent`,
  `createPositionUnits` or `createPositionValues`.
- `options.fontSize`: either a font-size object from
  [`workbook.createFontSize`](#workbookcreatefontsizeoptions), or a
  [`workbook.FontSize`](#workbookfontsize) enum value.
- `options.fontStyle`, `options.fontWeight`, `options.textAlign`, `options.textDecorationLine`,
  `options.textDecorationStyle`: values of the enums of the same names.

**Returns:** `workbook.Style`

**Errors:**

- `INVALID_COLOR`: `options.backgroundColor`, `options.color` or `options.textDecorationColor` is
  neither a `Color` enum value nor a color object.
- `INVALID_FONT_SIZE`: `options.fontSize` is invalid.
- `INVALID_FONT_STYLE`: `options.fontStyle` is invalid.
- `INVALID_FONT_WEIGHT`: `options.fontWeight` is invalid.
- `INVALID_IMAGE`: `options.backgroundImage` is invalid.
- `INVALID_TEXT_ALIGN`: `options.textAlign` is invalid.
- `INVALID_TEXT_DECORATION_LINE`: `options.textDecorationLine` is invalid.
- `INVALID_TEXT_DECORATION_STYLE`: `options.textDecorationStyle` is invalid.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

The `Style` object's color properties document a different error code, `UNSUPPORTED_COLOR`, for a
bad color string. The source does not say which code you get when.

### workbook.createColor(options)
<!-- id: N/workbook.createColor -->

Creates a custom RGBA color object (see [`Color` (object)](#color-object)) for use in
`createStyle`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.alpha` | number | No | |
| `options.blue` | number | No | |
| `options.green` | number | No | |
| `options.red` | number | No | |

- `options.red`, `options.green`, `options.blue`: 0 to 255.
- `options.alpha`: 0 to 1 according to this function's documentation. Out of range raises
  `INVALID_INVALID_ALPHA_VALUE` [sic].

> [!SOURCE-ERROR] This function gives `alpha` a range of 0 to 1, but the `Color` object's `alpha`
> property says 0 to 255. Its error code also appears with a doubled prefix,
> `INVALID_INVALID_ALPHA_VALUE` [sic], while the property page uses `INVALID_ALPHA_VALUE`. The spec
> does not say which range or code is right: test the value you plan to use, and catch both codes.
<!-- source-error: N/workbook.createColor | options.alpha | property_page_says_0_to_255 -->
<!-- source-error: N/workbook.createColor | options.alpha | error_code_doubled_prefix_invalid_invalid_alpha_value -->

**Returns:** `workbook.Color`, meaning the color object [`Color` (object)](#color-object), not the
[`workbook.Color`](#workbookcolor) enum.

**Errors:**

- `INVALID_COLOR_VALUE`: `options.red`, `options.green` or `options.blue` is outside 0 to 255.
- `INVALID_INVALID_ALPHA_VALUE` [sic]: `options.alpha` is outside its allowed range.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createFontSize(options)
<!-- id: N/workbook.createFontSize -->

Creates a font-size object: a number plus a unit. Use it when the named
[`workbook.FontSize`](#workbookfontsize) values are not precise enough.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.unit` | workbook.Unit | Yes | |
| `options.size` | number | Yes | |

- `options.unit`: a [`workbook.Unit`](#workbookunit) value.

**Returns:** `workbook.FontSize`, meaning the object [`FontSize` (object)](#fontsize-object).

**Errors:**

- `INVALID_UNIT`: `options.unit` is not a `Unit` value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createPositionPercent(options)
<!-- id: N/workbook.createPositionPercent -->

Creates a background position given as percentages along x and y. For `createStyle`'s
`backgroundPosition`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.percentX` | number | Yes | |
| `options.percentY` | number | Yes | |

**Returns:** `workbook.PositionPercent`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createPositionUnits(options)
<!-- id: N/workbook.createPositionUnits -->

Creates a background position given as x and y offsets in a length unit. For `createStyle`'s
`backgroundPosition`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.unit` | workbook.Unit | Yes | |
| `options.x` | number | Yes | |
| `options.y` | number | Yes | |

- `options.unit`: a [`workbook.Unit`](#workbookunit) value.

**Returns:** `workbook.PositionUnits`

**Errors:**

- `INVALID_POSITION`: the resulting position is not valid.
- `INVALID_UNIT`: `options.unit` is not a `Unit` value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### workbook.createPositionValues(options)
<!-- id: N/workbook.createPositionValues -->

Creates a background position from keywords such as `LEFT` or `CENTER`. For `createStyle`'s
`backgroundPosition`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.horizontal` | string | Yes | |
| `options.vertical` | string | Yes | |

- `options.horizontal`, `options.vertical`: values from [`workbook.Position`](#workbookposition).

> [!SOURCE-ERROR] The source's parameter descriptions for this function were copied from
> `createPositionUnits` and don't describe these parameters. Go by the parameter names and the
> `Position` enum instead.
<!-- source-error: N/workbook.createPositionValues | - | param_descriptions_copied_from_position_units -->

**Returns:** `workbook.PositionValues`

**Errors:**

- `INVALID_POSITION`: `options.horizontal` or `options.vertical` is not a `Position` value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

## Objects

All objects below are server-side only.

### Workbook (object)
<!-- id: N/workbook.Workbook -->

A workbook, as built by [`workbook.create`](#workbookcreateoptions) or loaded by
[`workbook.loadWorkbook`](#workbookloadworkbookoptions). In a Workbook Builder plug-in, this is the
object you assign to `context.workbook` (see the
[plug-in context](../topics/workbook_builder_plugin.context.md)). Since 2020.2.

#### Workbook.runPivot(options)
<!-- id: N/workbook.Workbook#runPivot -->

Runs one of the workbook's pivots and returns its intersections, meaning the computed cells. Each
intersection gives a row position, a column position and the measure values there. Running a pivot
is also the documented way to check a plug-in's pivot logic in the debugger.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: the ID of a pivot in this workbook.

**Returns:** `workbook.PivotIntersection[]`

**Errors:**

- `PIVOT_DOES_NOT_EXIST`: this workbook has no pivot with ID `options.id`.

**Governance:** 10 units for each intersection returned. Large pivots get expensive, so narrow them
with filters first.

**Availability:** server scripts.

**Since:** 2021.2

The Analytic APIs overview also claims that table views can be run, but `runPivot` is the only run
method the source documents.

#### Workbook.charts
<!-- id: N/workbook.Workbook#charts -->

`workbook.Chart[]`. The workbook's charts (an array of `Chart` objects). Since 2020.2.

#### Workbook.description
<!-- id: N/workbook.Workbook#description -->

`string`. The description, set when the workbook is created. Since 2020.2.

#### Workbook.id
<!-- id: N/workbook.Workbook#id -->

`string`. The workbook ID, set when the workbook is created. Since 2020.2.

#### Workbook.name
<!-- id: N/workbook.Workbook#name -->

`string`. The workbook name. For a workbook built by a Workbook Builder plug-in, the UI ignores this
value and shows the name from the plug-in implementation record instead (the dataset name set in
code is kept). Since 2020.2.

#### Workbook.pivots
<!-- id: N/workbook.Workbook#pivots -->

`workbook.Pivot[]`. The workbook's pivots (an array of `Pivot` objects). Since 2020.2.

#### Workbook.tables
<!-- id: N/workbook.Workbook#tables -->

`workbook.Table[]`. The workbook's table views. Since 2020.2.

### Table (object)
<!-- id: N/workbook.Table -->

A table view, returned by [`workbook.createTable`](#workbookcreatetableoptions). The object is
listed since 2021.2, although `createTable` itself is since 2020.2.

#### Table.columns
<!-- id: N/workbook.Table#columns -->

`workbook.TableColumn[]`. The table's columns, an array of `TableColumn` objects. Since 2021.2.

#### Table.dataset
<!-- id: N/workbook.Table#dataset -->

`dataset.Dataset`. The dataset the table reads from. Since 2021.2.

#### Table.id
<!-- id: N/workbook.Table#id -->

`string`. The table ID. Since 2021.2.

#### Table.name
<!-- id: N/workbook.Table#name -->

`string | workbook.Expression`. The table name, either plain text or an expression. Since 2021.2.

### TableColumn (object)
<!-- id: N/workbook.TableColumn -->

One column of a table view, returned by
[`workbook.createTableColumn`](#workbookcreatetablecolumnoptions) and used by `createTable`. Since
2020.2.

#### TableColumn.alias
<!-- id: N/workbook.TableColumn#alias -->

`string`. The column's alias. Since 2020.2.

#### TableColumn.datasetColumnAlias
<!-- id: N/workbook.TableColumn#datasetColumnAlias -->

`string`. The alias of the dataset column shown. Since 2020.2.

#### TableColumn.fieldContext
<!-- id: N/workbook.TableColumn#fieldContext -->

`workbook.FieldContext`. How the field value is presented. Since 2020.2.

#### TableColumn.filters
<!-- id: N/workbook.TableColumn#filters -->

`workbook.TableColumnFilter`. Filter on this column. The property name is plural but the type is a
single filter, as the spec records it. Since 2020.2.

#### TableColumn.label
<!-- id: N/workbook.TableColumn#label -->

`string`. The column heading. Since 2020.2.

#### TableColumn.sort
<!-- id: N/workbook.TableColumn#sort -->

`workbook.Sort`. Sort settings for the column. Since 2020.2.

#### TableColumn.width
<!-- id: N/workbook.TableColumn#width -->

`number`. Display width in the UI. Setting a non-number raises `WRONG_PARAMETER_TYPE`. Since 2020.2.

### TableColumnFilter (object)
<!-- id: N/workbook.TableColumnFilter -->

An operator-and-values test on a column, returned by
[`workbook.createTableColumnFilter`](#workbookcreatetablecolumnfilteroptions) and used by
`createConditionalFormatRule`. Since 2021.2.

#### TableColumnFilter.operator
<!-- id: N/workbook.TableColumnFilter#operator -->

`string`. An N/query `Operator` value. Any other string raises `INVALID_OPERATOR`. Since 2021.2.

#### TableColumnFilter.values
<!-- id: N/workbook.TableColumnFilter#values -->

`Array<null | Object | boolean | number | string | Date>`. The comparison values. Entries may be
null, objects, booleans, numbers, strings or dates. Since 2021.2.

### TableColumnCondition (object)
<!-- id: N/workbook.TableColumnCondition -->

Column filters combined under an operator, returned by
[`workbook.createTableColumnCondition`](#workbookcreatetablecolumnconditionoptions). Since 2021.1.

#### TableColumnCondition.filters
<!-- id: N/workbook.TableColumnCondition#filters -->

`workbook.TableColumnFilter[]`. The filters being combined. Assigning a value of the wrong type
raises `WRONG_PARAMETER_TYPE`. Since 2021.1.

> [!SOURCE-ERROR] The source files this property under `TableColumnFilter` instead of
> `TableColumnCondition` in its parent-object row. The spec places it on `TableColumnCondition`;
> confirm against the object you receive.
<!-- source-error: N/workbook.TableColumnCondition#filters | - | parent_object_row_says_tablecolumnfilter -->

#### TableColumnCondition.operator
<!-- id: N/workbook.TableColumnCondition#operator -->

`string`. The operator joining the filters. A value of the wrong type raises `WRONG_PARAMETER_TYPE`.
An operator that is not valid raises `INVALID_OPERATOR`. Since 2021.1.

### FieldContext (object)
<!-- id: N/workbook.FieldContext -->

Presentation context for a field, returned by
[`workbook.createFieldContext`](#workbookcreatefieldcontextoptions). Since 2020.2.

#### FieldContext.name
<!-- id: N/workbook.FieldContext#name -->

`string`. The context name. The spec lists `DISPLAY` and `CONSOLIDATED`. A non-string value raises
`WRONG_PARAMETER_TYPE`. Since 2020.2.

> [!SOURCE-ERROR] The source gives `DISPLAY` and `CONSOLIDATED` as examples, not as a complete list,
> so other names may exist; a Workbook Builder plug-in sample, for instance, uses `RAW` and
> `HIERARCHY` field contexts. The spec does not give the full list.
<!-- source-error: N/workbook.FieldContext#name | - | values_given_as_examples -->

#### FieldContext.parameters
<!-- id: N/workbook.FieldContext#parameters -->

`Object`. Extra settings as a key/value object. Since 2020.2.

### Sort (object)
<!-- id: N/workbook.Sort -->

Sort settings, returned by [`workbook.createSort`](#workbookcreatesortoptions). It is used by
`createSortByDataDimensionItem` and `createTableColumn`. Since 2020.2.

#### Sort.order
<!-- id: N/workbook.Sort#order -->

`number`. The sort order. Since 2022.2. `createSort` has no matching option, and the source does not
state how it is set.

#### Sort.ascending
<!-- id: N/workbook.Sort#ascending -->

`boolean`. Ascending when `true`. Since 2020.2.

#### Sort.caseSensitive
<!-- id: N/workbook.Sort#caseSensitive -->

`boolean`. Whether letter case affects the order. Since 2020.2.

#### Sort.locale
<!-- id: N/workbook.Sort#locale -->

Declared type `query.Operator`. Read-only: it cannot be changed after the sort is created. It
accepts a string. Since 2020.2.

> [!SOURCE-ERROR] The source's type row says `query.Operator`. The property holds a locale string,
> and `createSort`'s `locale` option takes a `query.SortLocale` value. Treat it as a sort locale, and
> verify.
<!-- source-error: N/workbook.Sort#locale | - | type_row_says_query_operator -->

#### Sort.nullsLast
<!-- id: N/workbook.Sort#nullsLast -->

`boolean`. Whether nulls sort after other values. Since 2020.2.

### ConditionalFormat (object)
<!-- id: N/workbook.ConditionalFormat -->

A set of conditional format rules, returned by
[`workbook.createConditionalFormat`](#workbookcreateconditionalformatoptions). Table columns take it
through `createTableColumn`'s `conditionalFormats`. Since 2021.2.

> [!SOURCE-ERROR] The source's object page also lists `createPivot` as a consumer, but `createPivot`
> has no parameter that takes a conditional format (pivots are styled with report styles). Use it
> with table columns only.
<!-- source-error: N/workbook.ConditionalFormat | - | object_page_says_used_by_createpivot_no_such_param -->

#### ConditionalFormat.rules
<!-- id: N/workbook.ConditionalFormat#rules -->

Declared type `workbook.ConditionalFormatRule`, but the property takes an array of rules. An empty
array raises `NO_RULE_DEFINED`. Since 2021.2.

> [!SOURCE-ERROR] The type row names a single rule, while the error rows describe an array. Pass an
> array of `ConditionalFormatRule` objects, as `createConditionalFormat` does.
<!-- source-error: N/workbook.ConditionalFormat#rules | - | rules_type_row_single_but_errors_say_array -->

### ConditionalFormatRule (object)
<!-- id: N/workbook.ConditionalFormatRule -->

A filter-plus-style pair, returned by
[`workbook.createConditionalFormatRule`](#workbookcreateconditionalformatruleoptions) and used by
`createConditionalFormat`. Since 2021.2.

#### ConditionalFormatRule.filter
<!-- id: N/workbook.ConditionalFormatRule#filter -->

`workbook.TableColumnFilter`. Which cells the rule matches. Since 2021.2.

#### ConditionalFormatRule.style
<!-- id: N/workbook.ConditionalFormatRule#style -->

`workbook.Style`. How matched cells look. Since 2021.2.

### Pivot (object)
<!-- id: N/workbook.Pivot -->

A pivot, returned by [`workbook.createPivot`](#workbookcreatepivotoptions) and used by
`workbook.create`. Since 2020.2.

#### Pivot.datasetLink
<!-- id: N/workbook.Pivot#datasetLink -->

`datasetLink.DatasetLink`. A dataset link the pivot is based on (from [N/datasetLink](N_datasetLink.md)).
`createPivot` lists no matching option. Since 2020.2.

#### Pivot.portletName
<!-- id: N/workbook.Pivot#portletName -->

`string | workbook.Expression`. Name used when the pivot is shown in a portlet. `createPivot` lists
no matching option. Since 2021.2.

#### Pivot.reportStyles
<!-- id: N/workbook.Pivot#reportStyles -->

`workbook.ReportStyle[]`. Styles applied to pivot cells. An empty array raises `NO_RULE_DEFINED`.
Since 2021.1.

#### Pivot.aggregationFilters
<!-- id: N/workbook.Pivot#aggregationFilters -->

`Array<workbook.LimitingFilter | workbook.ConditionalFilter>`. Filters on aggregated values.
Since 2020.2.

#### Pivot.columnAxis
<!-- id: N/workbook.Pivot#columnAxis -->

`workbook.PivotAxis`. The column (x) axis. Since 2020.2.

#### Pivot.dataset
<!-- id: N/workbook.Pivot#dataset -->

`dataset.Dataset`. The source dataset. Since 2020.2.

#### Pivot.filterExpressions
<!-- id: N/workbook.Pivot#filterExpressions -->

`workbook.Expression[]`. Filters on non-aggregated values. Since 2020.2.

#### Pivot.id
<!-- id: N/workbook.Pivot#id -->

`string`. The pivot ID. Pass it to `Workbook.runPivot`. Since 2020.2.

#### Pivot.name
<!-- id: N/workbook.Pivot#name -->

`string`. The pivot name. Since 2020.2.

#### Pivot.rowAxis
<!-- id: N/workbook.Pivot#rowAxis -->

`workbook.PivotAxis`. The row (y) axis. Since 2020.2.

### PivotAxis (object)
<!-- id: N/workbook.PivotAxis -->

One pivot axis, returned by [`workbook.createPivotAxis`](#workbookcreatepivotaxisoptions) and used
by `createPivot`. Since 2020.2.

#### PivotAxis.root
<!-- id: N/workbook.PivotAxis#root -->

`workbook.DataDimension | workbook.Section`. What the axis is built from. Since 2020.2.

#### PivotAxis.sortDefinitions
<!-- id: N/workbook.PivotAxis#sortDefinitions -->

`workbook.SortDefinition[]`. How the axis is ordered. Since 2020.2.

### DataDimension (object)
<!-- id: N/workbook.DataDimension -->

A grouping dimension, returned by
[`workbook.createDataDimension`](#workbookcreatedatadimensionoptions). It is used by
`createDimensionSelector`, `createPivotAxis` and `createSection`. Since 2020.2.

#### DataDimension.children
<!-- id: N/workbook.DataDimension#children -->

Declared type `Array`. Nested elements under the dimension. The spec says it accepts data
dimensions and sections, and lists the possible element types as `Section`, `DataDimension`,
`CalculatedMeasure` and `DataMeasure`. Since 2020.2.

> [!SOURCE-ERROR] The source's type row includes the two measure types, while its error row only
> mentions dimensions and sections. Its type cell also ends with a dangling `|`, and the spec marks
> this for human review. `createDataDimension`'s `children` option accepts all four types, but the
> spec does not confirm what this property allows, so verify.
<!-- source-error: N/workbook.DataDimension#children | - | type_row_also_lists_measures_unlike_error_row -->
<!-- source-error: N/workbook.DataDimension#children | - | type_cell_has_trailing_union_bar -->

#### DataDimension.items
<!-- id: N/workbook.DataDimension#items -->

`workbook.DataDimensionItem[]`. What the dimension groups by. Since 2020.2.

#### DataDimension.totalLine
<!-- id: N/workbook.DataDimension#totalLine -->

`workbook.TotalLine`. Where the total line appears. A value outside `TotalLine` raises
`INVALID_TOTAL_LINE`. Since 2020.2.

### DataDimensionItem (object)
<!-- id: N/workbook.DataDimensionItem -->

One grouping expression, returned by
[`workbook.createDataDimensionItem`](#workbookcreatedatadimensionitemoptions). It is used by
`createDataDimension` and `createSortByDataDimensionItem`. Since 2020.2.

#### DataDimensionItem.expression
<!-- id: N/workbook.DataDimensionItem#expression -->

`workbook.Expression`. The value grouped by. Since 2020.2.

#### DataDimensionItem.label
<!-- id: N/workbook.DataDimensionItem#label -->

`string`. Display label. Since 2020.2.

### Section (object)
<!-- id: N/workbook.Section -->

A container of dimensions, measures and sections, returned by
[`workbook.createSection`](#workbookcreatesectionoptions). It is used by `createDataDimension`,
`createPivotAxis` and `createSection` itself. Since 2020.2.

#### Section.children
<!-- id: N/workbook.Section#children -->

`Array<workbook.CalculatedMeasure | workbook.DataDimension | workbook.DataMeasure | workbook.Section>`.
The section's contents. Since 2020.2.

#### Section.totalLine
<!-- id: N/workbook.Section#totalLine -->

`workbook.TotalLine`. Where the total line appears. A value outside `TotalLine` raises
`INVALID_TOTAL_LINE`. Since 2020.2.

### DataMeasure (object)
<!-- id: N/workbook.DataMeasure -->

An aggregated measure, returned by
[`workbook.createDataMeasure`](#workbookcreatedatameasureoptions) and used by
`createMeasureSelector`. Since 2021.2.

#### DataMeasure.aggregation
<!-- id: N/workbook.DataMeasure#aggregation -->

`workbook.Aggregation`, given as a string. A value outside the `Aggregation` enum raises
`INVALID_AGGREGATION`. Since 2021.2.

#### DataMeasure.expression
<!-- id: N/workbook.DataMeasure#expression -->

`workbook.Expression`. Used by single-expression measures. Cannot be combined with
[`DataMeasure.expressions`](#datameasureexpressions). Since 2021.2.

#### DataMeasure.expressions
<!-- id: N/workbook.DataMeasure#expressions -->

`workbook.Expression[]`. Used by multi-expression measures. Cannot be combined with
[`DataMeasure.expression`](#datameasureexpression). Since 2021.2.

#### DataMeasure.label
<!-- id: N/workbook.DataMeasure#label -->

`workbook.Expression | string`. Display label. Since 2021.2.

### CalculatedMeasure (object)
<!-- id: N/workbook.CalculatedMeasure -->

An expression-based measure, returned by
[`workbook.createCalculatedMeasure`](#workbookcreatecalculatedmeasureoptions) and used by
`createMeasureSelector`. Since 2021.2.

#### CalculatedMeasure.expression
<!-- id: N/workbook.CalculatedMeasure#expression -->

`workbook.Expression`. The calculation. Setting it when an expression is already defined raises
`MUTUALLY_EXCLUSIVE_ARGUMENTS`. Since 2021.2.

#### CalculatedMeasure.label
<!-- id: N/workbook.CalculatedMeasure#label -->

`workbook.Expression | string`. Display label. Since 2021.2.

### MeasureSelector (object)
<!-- id: N/workbook.MeasureSelector -->

A set of chosen measures, returned by
[`workbook.createMeasureSelector`](#workbookcreatemeasureselectoroptions) and used by
`createMeasureValueSelector`. Since 2021.2.

#### MeasureSelector.measures
<!-- id: N/workbook.MeasureSelector#measures -->

`workbook.CalculatedMeasure[] | workbook.DataMeasure[]`. The chosen measures. Anything other than a
non-empty array of measures raises `WRONG_PARAMETER_TYPE`. Since 2021.2.

### MeasureValueSelector (object)
<!-- id: N/workbook.MeasureValueSelector -->

A cell selector for report styles, returned by
[`workbook.createMeasureValueSelector`](#workbookcreatemeasurevalueselectoroptions) and used by
`createReportStyle`. Since 2021.2.

#### MeasureValueSelector.columnSelector
<!-- id: N/workbook.MeasureValueSelector#columnSelector -->

`workbook.DimensionSelector | workbook.PathSelector | workbook.DescendantorSelfNodesSelector`.
Which columns to select. Since 2021.2.

#### MeasureValueSelector.measureSelector
<!-- id: N/workbook.MeasureValueSelector#measureSelector -->

`workbook.MeasureSelector[]`. Which measures to select. Since 2021.2.

> [!SOURCE-ERROR] The property is typed as an array of measure selectors, while
> `createMeasureValueSelector` takes a single `MeasureSelector`. Pass a single selector to the
> factory, and check the shape when you read this property back.
<!-- source-error: N/workbook.MeasureValueSelector#measureSelector | - | measureselector_array_on_property_single_on_create -->

#### MeasureValueSelector.rowSelector
<!-- id: N/workbook.MeasureValueSelector#rowSelector -->

`workbook.DimensionSelector | workbook.PathSelector | workbook.DescendantorSelfNodesSelector`.
Which rows to select. Since 2021.2.

### DimensionSelector (object)
<!-- id: N/workbook.DimensionSelector -->

Points at one dimension or section. Returned by
[`workbook.createDimensionSelector`](#workbookcreatedimensionselectoroptions). It is used by
`createConditionalFilter`, `createLimitingFilter`, `createMeasureValueSelector`,
`createPathSelector`, `createSortByMeasure` and `createSortDefinition`. Since 2020.2.

#### DimensionSelector.dimension
<!-- id: N/workbook.DimensionSelector#dimension -->

`workbook.DataDimension | workbook.Section`. The target. Since 2020.2.

### PathSelector (object)
<!-- id: N/workbook.PathSelector -->

A path through nested dimensions, returned by
[`workbook.createPathSelector`](#workbookcreatepathselectoroptions). It is used by
`createConditionalFilter`, `createLimitingFilter`, `createSortDefinition` and
`createSortByMeasure`. Since 2020.2.

#### PathSelector.elements
<!-- id: N/workbook.PathSelector#elements -->

Declared type `workbook.DimensionSelector`. The steps of the path, read like XPath segments.
Since 2020.2.

> [!SOURCE-ERROR] The property is typed as a single dimension selector, while `createPathSelector`
> takes an array of them for the same data. Expect an array of `DimensionSelector` objects, and
> verify.
<!-- source-error: N/workbook.PathSelector#elements | - | elements_typed_single_but_create_takes_array -->

### DescendantorSelfNodesSelector (object)
<!-- id: N/workbook.DescendantorSelfNodesSelector -->

A selector type accepted by `createMeasureValueSelector`, `createSortByMeasure` and the
`ConditionalFilter` row and column selectors. There is no `create*` function for it and it has no
properties: the source's syntax sample reaches it as a module-level constant,
`workbook.DescendantOrSelfNodesSelector`, and passes that straight in as a selector. The name
suggests a match on a node together with all its descendants; the source does not describe its
matching further. Since 2021.2.

> [!SOURCE-ERROR] The object's title is spelled `DescendantorSelfNodesSelector` [sic] (lower-case
> "or"), while the sample code writes `DescendantOrSelfNodesSelector`. The spec keeps the title
> spelling as the ID and flags the mismatch; the sample's spelling is the one shown being used in
> code, but verify which name your account exposes.
<!-- source-error: N/workbook.DescendantorSelfNodesSelector | - | sample_casing_DescendantOrSelf_vs_title_DescendantorSelf -->

### ChildNodesSelector (object)
<!-- id: N/workbook.ChildNodesSelector -->

A selector type with no `create*` function: according to the source it is used directly rather than
built. It has no properties. Where it fits: it is one of the four selector types that
[`ConditionalFilter.columnSelector`](#conditionalfiltercolumnselector) and
[`ConditionalFilter.rowSelector`](#conditionalfilterrowselector) accept (alongside
`DescendantorSelfNodesSelector` [sic], `PathSelector` and `DimensionSelector`); no other property or
parameter lists it. The concept material counts it among the selectors that have no factory method.
Unlike `DescendantorSelfNodesSelector` [sic], the source shows no expression for referencing it
(such as a module-level constant), so verify how your account exposes it. Since 2021.2.

### SortDefinition (object)
<!-- id: N/workbook.SortDefinition -->

Ordering for part of an axis, returned by
[`workbook.createSortDefinition`](#workbookcreatesortdefinitionoptions) and used by
`createPivotAxis`. Since 2020.2.

#### SortDefinition.selector
<!-- id: N/workbook.SortDefinition#selector -->

`workbook.DimensionSelector | workbook.PathSelector`. Which part of the axis is sorted. Since 2020.2.

> [!SOURCE-ERROR] The source's error row says this property expects a boolean, which contradicts
> its selector type. Pass a `DimensionSelector` or `PathSelector`.
<!-- source-error: N/workbook.SortDefinition#selector | - | error_row_says_boolean_for_selector -->

#### SortDefinition.sortBys
<!-- id: N/workbook.SortDefinition#sortBys -->

`Array<workbook.SortByDataDimensionItem | workbook.SortByMeasure>`. The sort keys. Since 2020.2.

### SortByDataDimensionItem (object)
<!-- id: N/workbook.SortByDataDimensionItem -->

A sort key based on a dimension item, returned by
[`workbook.createSortByDataDimensionItem`](#workbookcreatesortbydatadimensionitemoptions). Since
2021.2.

#### SortByDataDimensionItem.item
<!-- id: N/workbook.SortByDataDimensionItem#item -->

`workbook.DataDimensionItem`. The item sorted on. Since 2021.2.

#### SortByDataDimensionItem.sort
<!-- id: N/workbook.SortByDataDimensionItem#sort -->

`workbook.Sort`. Direction and options. Since 2021.2.

### SortByMeasure (object)
<!-- id: N/workbook.SortByMeasure -->

A sort key based on a measure, returned by
[`workbook.createSortByMeasure`](#workbookcreatesortbymeasureoptions). Since 2021.2.

#### SortByMeasure.measure
<!-- id: N/workbook.SortByMeasure#measure -->

`workbook.CalculatedMeasure | workbook.DataMeasure`. The measure sorted on. Since 2021.2.

#### SortByMeasure.otherAxisSelector
<!-- id: N/workbook.SortByMeasure#otherAxisSelector -->

`workbook.DescendantorSelfNodesSelector | workbook.PathSelector | workbook.DimensionSelector`. A
selector for the axis that is not being sorted: it fixes where on that axis the measure values are
read from. Since 2021.2.

#### SortByMeasure.sort
<!-- id: N/workbook.SortByMeasure#sort -->

`workbook.Sort`. Direction and options. Since 2021.2.

### LimitingFilter (object)
<!-- id: N/workbook.LimitingFilter -->

A top-N filter, returned by [`workbook.createLimitingFilter`](#workbookcreatelimitingfilteroptions)
and used by `createPivot`. Since 2020.2.

#### LimitingFilter.filteredNodesSelector
<!-- id: N/workbook.LimitingFilter#filteredNodesSelector -->

`workbook.PathSelector | workbook.DimensionSelector`. The nodes being limited. Since 2020.2.

#### LimitingFilter.limit
<!-- id: N/workbook.LimitingFilter#limit -->

`number`. How many nodes to keep. Since 2020.2.

#### LimitingFilter.row
<!-- id: N/workbook.LimitingFilter#row -->

`boolean`. Whether the filter applies to the row axis (the factory documents `true` as row axis and
`false` as column axis). Since 2020.2.

#### LimitingFilter.sortBys
<!-- id: N/workbook.LimitingFilter#sortBys -->

`Array<workbook.SortByDataDimensionItem | workbook.SortByMeasure>`. The ordering used to decide
which nodes are kept. Since 2020.2.

### ConditionalFilter (object)
<!-- id: N/workbook.ConditionalFilter -->

A predicate filter, returned by
[`workbook.createConditionalFilter`](#workbookcreateconditionalfilteroptions). It is used by
`createPivot` and `createTableColumn`. Since 2020.2.

#### ConditionalFilter.columnSelector
<!-- id: N/workbook.ConditionalFilter#columnSelector -->

Declared type `Object`, which the spec uses because the full union is too long to write out. The
value must be one of `DescendantorSelfNodesSelector` [sic], `PathSelector`, `DimensionSelector` or
[`ChildNodesSelector`](#childnodesselector-object). It selects columns. Since 2020.2.

#### ConditionalFilter.rowSelector
<!-- id: N/workbook.ConditionalFilter#rowSelector -->

Declared type `Object`, for the same reason. The value must be one of
`DescendantorSelfNodesSelector` [sic], `PathSelector`, `DimensionSelector` or
[`ChildNodesSelector`](#childnodesselector-object). It selects rows. Since 2020.2.

#### ConditionalFilter.filteredNodesSelector
<!-- id: N/workbook.ConditionalFilter#filteredNodesSelector -->

`workbook.PathSelector | workbook.DimensionSelector`. The nodes being filtered. Since 2020.2.

#### ConditionalFilter.measure
<!-- id: N/workbook.ConditionalFilter#measure -->

`workbook.CalculatedMeasure | workbook.DataMeasure`. The measure tested. Since 2020.2.

#### ConditionalFilter.otherAxisSelector
<!-- id: N/workbook.ConditionalFilter#otherAxisSelector -->

`workbook.PathSelector | workbook.DimensionSelector`. A position on the other axis. Since 2020.2.

#### ConditionalFilter.predicate
<!-- id: N/workbook.ConditionalFilter#predicate -->

`workbook.Expression`. The condition a node must meet to stay. Since 2020.2.

#### ConditionalFilter.row
<!-- id: N/workbook.ConditionalFilter#row -->

`boolean`. `true` applies the filter to rows and `false` applies it to columns. Since 2020.2.

> [!SOURCE-ERROR] The source's error row says a string is expected, but the property is a boolean.
> Pass `true` or `false`.
<!-- source-error: N/workbook.ConditionalFilter#row | - | error_row_says_string_for_boolean -->

### ReportStyle (object)
<!-- id: N/workbook.ReportStyle -->

Pivot cell styling, returned by [`workbook.createReportStyle`](#workbookcreatereportstyleoptions)
and used by `createPivot`. Since 2021.2.

#### ReportStyle.rules
<!-- id: N/workbook.ReportStyle#rules -->

`workbook.ReportStyleRule[]`. An empty array raises `NO_RULE_DEFINED`. Since 2021.2.

#### ReportStyle.selectors
<!-- id: N/workbook.ReportStyle#selectors -->

`workbook.MeasureValueSelector[]`. An empty array raises `NO_SELECTORS_DEFINED`. Since 2021.2.

### ReportStyleRule (object)
<!-- id: N/workbook.ReportStyleRule -->

A condition-plus-style pair, returned by
[`workbook.createReportStyleRule`](#workbookcreatereportstyleruleoptions) and used by
`createReportStyle`. Since 2021.2.

#### ReportStyleRule.expression
<!-- id: N/workbook.ReportStyleRule#expression -->

`workbook.Expression`, which must be boolean. When it is true for a cell, the style is applied.
Since 2021.2.

#### ReportStyleRule.style
<!-- id: N/workbook.ReportStyleRule#style -->

`workbook.Style`. The style to apply. Since 2021.2.

### Chart (object)
<!-- id: N/workbook.Chart -->

A chart, returned by [`workbook.createChart`](#workbookcreatechartoptions). Since 2020.2.

#### Chart.aggregationFilters
<!-- id: N/workbook.Chart#aggregationFilters -->

`Array<workbook.LimitingFilter | workbook.ConditionalFilter>`. Filters on aggregated values.
Since 2020.2.

#### Chart.category
<!-- id: N/workbook.Chart#category -->

`string`, as the spec records it. Since 2020.2.

> [!SOURCE-ERROR] This property is typed as a string, while `createChart`'s `category` option takes
> a `workbook.Category` object. The spec marks the conflict for review without resolving it; expect
> a `Category` object and check what you read back.
<!-- source-error: N/workbook.Chart#category | - | chart_category_string_vs_createChart_workbook.Category -->

#### Chart.dataset
<!-- id: N/workbook.Chart#dataset -->

`dataset.Dataset`. The source dataset. Since 2020.2.

#### Chart.filterExpressions
<!-- id: N/workbook.Chart#filterExpressions -->

`workbook.Expression[]`. Filters on non-aggregated values. Since 2020.2.

#### Chart.id
<!-- id: N/workbook.Chart#id -->

`string`. The chart ID. Since 2020.2.

> [!SOURCE-ERROR] The wording of the source's error row for this property is inverted: it describes
> the failure condition backwards. Rely on the type (a string), and don't infer the error rule from
> that row.
<!-- source-error: N/workbook.Chart#id | - | error_row_wording_inverted -->

#### Chart.legend
<!-- id: N/workbook.Chart#legend -->

Declared type `TBD`. The spec has no real type for this property. `createChart` takes its `legend`
option as a `workbook.Legend`. Since 2020.2.

> [!SOURCE-ERROR] The source's type row literally says "TBD", and its error row leaves out the
> expected type. The spec doesn't confirm what this property holds; a `Legend` object is the likely
> fit given `createChart`, but verify.
<!-- source-error: N/workbook.Chart#legend | - | error_row_omits_expected_type -->
<!-- source-error: N/workbook.Chart#legend | - | type_row_says_tbd -->

#### Chart.name
<!-- id: N/workbook.Chart#name -->

`string`. The chart name. Since 2020.2.

#### Chart.series
<!-- id: N/workbook.Chart#series -->

Declared type `workbook.Series`. Since 2020.2.

> [!SOURCE-ERROR] The property is typed as a single series, while `createChart` takes an array of
> series. The spec marks this for review; expect an array and verify.
<!-- source-error: N/workbook.Chart#series | - | chart_series_typed_single_but_createchart_takes_array -->

#### Chart.stacking
<!-- id: N/workbook.Chart#stacking -->

`workbook.Stacking`. A value outside `Stacking` raises `INVALID_STACKING_TYPE`. Since 2020.2.

#### Chart.subTitle
<!-- id: N/workbook.Chart#subTitle -->

`string`. The subtitle. Since 2020.2.

> [!SOURCE-ERROR] As with `Chart.id`, the error row for this property is worded backwards. Rely on
> the string type.
<!-- source-error: N/workbook.Chart#subTitle | - | error_row_wording_inverted -->

#### Chart.title
<!-- id: N/workbook.Chart#title -->

`string`. The title. Since 2020.2.

#### Chart.type
<!-- id: N/workbook.Chart#type -->

`workbook.ChartType`. A value outside `ChartType` raises `INVALID_CHART_TYPE`. Since 2020.2.

### ChartAxis (object)
<!-- id: N/workbook.ChartAxis -->

A chart axis, returned by [`workbook.createChartAxis`](#workbookcreatechartaxisoptions). Since
2020.2.

#### ChartAxis.title
<!-- id: N/workbook.ChartAxis#title -->

`string`. The axis title. Since 2020.2.

### Category (object)
<!-- id: N/workbook.Category -->

A chart category, returned by [`workbook.createCategory`](#workbookcreatecategoryoptions). Since
2020.2.

#### Category.axis
<!-- id: N/workbook.Category#axis -->

`workbook.ChartAxis`. The axis the category runs along. Since 2020.2.

#### Category.root
<!-- id: N/workbook.Category#root -->

`workbook.DataDimension | workbook.Section`. What the category groups by. Since 2020.2.

#### Category.sortDefinitions
<!-- id: N/workbook.Category#sortDefinitions -->

`workbook.SortDefinition[]`. Ordering of the categories. Since 2020.2.

### Legend (object)
<!-- id: N/workbook.Legend -->

A chart legend, returned by [`workbook.createLegend`](#workbookcreatelegendoptions). Since 2020.2.

#### Legend.axes
<!-- id: N/workbook.Legend#axes -->

`workbook.ChartAxis[]`. Axes for the legend. Since 2020.2.

#### Legend.root
<!-- id: N/workbook.Legend#root -->

`workbook.DataDimension | workbook.Section`. What the legend splits by. Since 2020.2.

#### Legend.sortDefinitions
<!-- id: N/workbook.Legend#sortDefinitions -->

`workbook.SortDefinition[]`. Ordering of the legend entries. Since 2020.2.

### Series (object)
<!-- id: N/workbook.Series -->

A chart series, returned by [`workbook.createSeries`](#workbookcreateseriesoptions). Since 2020.2.

#### Series.aspects
<!-- id: N/workbook.Series#aspects -->

`workbook.Aspect[]`. The aspects that make up the series. Since 2020.2.

### Aspect (object)
<!-- id: N/workbook.Aspect -->

A measure's role in a series, returned by [`workbook.createAspect`](#workbookcreateaspectoptions).
Since 2020.2.

#### Aspect.measure
<!-- id: N/workbook.Aspect#measure -->

`workbook.CalculatedMeasure | workbook.DataMeasure`. The measure shown. Since 2020.2.

#### Aspect.type
<!-- id: N/workbook.Aspect#type -->

`workbook.AspectType`. A value outside `AspectType` raises `INVALID_ASPECT_TYPE`. Since 2020.2.

### Expression (object)
<!-- id: N/workbook.Expression -->

A formula node, returned by [`workbook.createExpression`](#workbookcreateexpressionoptions) and also
produced by `createConstant`, `createTranslation` and N/dataset's `getExpressionFromColumn`. It is
used by `createCalculatedMeasure`, `createConditionalFilter`, `createDataDimensionItem`,
`createDataMeasure`, `createPivot`, `createReportStyleRule`, `createTable` and `createTableColumn`.
Since 2020.2.

#### Expression.functionId
<!-- id: N/workbook.Expression#functionId -->

`workbook.ExpressionType`, given as a string. Which function the expression applies. Since 2020.2.

#### Expression.parameters
<!-- id: N/workbook.Expression#parameters -->

`Object`. The function's arguments, keyed by the names listed under
[`workbook.ExpressionType`](#workbookexpressiontype). Since 2020.2.

### Currency (object)
<!-- id: N/workbook.Currency -->

A currency amount. You get one from [`workbook.createCurrency`](#workbookcreatecurrencyoptions), and
running a pivot also returns them as cell values. Since 2021.2.

> [!SOURCE-ERROR] The source says this object can be used with `createConstant`, but
> `createConstant`'s `constant` option accepts only strings, numbers, booleans and dates. The spec
> does not say which is right; test before passing a `Currency` there.
<!-- source-error: N/workbook.Currency | - | object_said_usable_as_constant_createconstant_excludes_it -->

#### Currency.amount
<!-- id: N/workbook.Currency#amount -->

`number`. The amount. Read-only once the object exists. Since 2021.2.

#### Currency.id
<!-- id: N/workbook.Currency#id -->

`string`. The currency code, for example `USD`, `EUR` or `GBP`. Read-only: setting it after the
currency is created raises `READ_ONLY_PROPERTY`. Since 2021.2.

### Duration (object)
<!-- id: N/workbook.Duration -->

A duration value. You get one from [`workbook.createDuration`](#workbookcreatedurationoptions), and
it can appear as a pivot cell value. Since 2021.2.

#### Duration.amount
<!-- id: N/workbook.Duration#amount -->

`number`. How many units. Read-only once the object exists. Since 2021.2.

#### Duration.units
<!-- id: N/workbook.Duration#units -->

`string`. The unit. Read-only once the object exists. Since 2021.2.

### Range (object)
<!-- id: N/workbook.Range -->

A date or date-time range. You get one from [`workbook.createRange`](#workbookcreaterangeoptions),
and running a pivot also returns them. Since 2021.2.

#### Range.end
<!-- id: N/workbook.Range#end -->

`string`. The end of the range, formatted according to the user's date preferences. Read-only once
the object exists. Since 2021.2.

#### Range.start
<!-- id: N/workbook.Range#start -->

`string`. The start of the range, formatted according to the user's date preferences. Read-only
once the object exists. Since 2021.2.

### Record (object)
<!-- id: N/workbook.Record -->

A record reference returned as a value when a pivot runs. Since 2021.2.

#### Record.name
<!-- id: N/workbook.Record#name -->

`string`. The record's display name. Read-only. Since 2021.2.

#### Record.primaryKey
<!-- id: N/workbook.Record#primaryKey -->

`number`. The record's key. Read-only. Since 2021.2.

#### Record.properties
<!-- id: N/workbook.Record#properties -->

`Object`. Further properties of the record. Read-only. Since 2021.2.

### RecordKey (object)
<!-- id: N/workbook.RecordKey -->

A record key. You build one with [`createSimpleRecordKey`](#workbookcreatesimplerecordkeyoptions) or
[`createComplexRecordKey`](#workbookcreatecomplexrecordkeyoptions), and running a pivot can also
return them. Since 2021.2.

> [!SOURCE-ERROR] As with `Currency`, the source says a record key can be used with
> `createConstant`, but that function's `constant` option does not accept it. Test before relying on
> it.
<!-- source-error: N/workbook.RecordKey | - | object_said_usable_as_constant_createconstant_excludes_it -->

#### RecordKey.properties
<!-- id: N/workbook.RecordKey#properties -->

`Object`. The key's properties. Read-only. Since 2021.2.

### Style (object)
<!-- id: N/workbook.Style -->

A visual style, returned by [`workbook.createStyle`](#workbookcreatestyleoptions). It holds settings
such as background color, font size, font style and text alignment, and applies through
`createConditionalFormatRule` (table conditional formats) and `createReportStyle` (via report style
rules). Since 2021.2.

#### Style.backgroundColor
<!-- id: N/workbook.Style#backgroundColor -->

`workbook.Color | string`. Either a `Color` enum value or a color object. A string that is not a
`Color` value raises `UNSUPPORTED_COLOR`. Since 2021.2.

#### Style.backgroundImage
<!-- id: N/workbook.Style#backgroundImage -->

`workbook.Image`, given as a string. A value outside `Image` raises `INVALID_IMAGE`. Since 2021.2.

#### Style.backgroundPosition
<!-- id: N/workbook.Style#backgroundPosition -->

`workbook.PositionPercent | workbook.PositionUnits | workbook.PositionValues`. Where the background
image sits. Since 2021.2.

#### Style.color
<!-- id: N/workbook.Style#color -->

`workbook.Color | string`. Text color: a `Color` enum value or a color object. A string that is not
a `Color` value raises `UNSUPPORTED_COLOR`. Since 2021.2.

#### Style.fontSize
<!-- id: N/workbook.Style#fontSize -->

`workbook.FontSize | string`. A `FontSize` enum value or a font-size object. A string that is not a
`FontSize` value raises `INVALID_FONT_SIZE`. Since 2021.2.

#### Style.fontStyle
<!-- id: N/workbook.Style#fontStyle -->

`string`, from [`workbook.FontStyle`](#workbookfontstyle). Any other value raises
`INVALID_FONT_STYLE`. Since 2021.2.

#### Style.fontWeight
<!-- id: N/workbook.Style#fontWeight -->

`string`, from [`workbook.FontWeight`](#workbookfontweight). Any other value raises
`INVALID_FONT_WEIGHT`. Since 2021.2.

#### Style.textAlign
<!-- id: N/workbook.Style#textAlign -->

`string`, from [`workbook.TextAlign`](#workbooktextalign). Any other value raises
`INVALID_TEXT_ALIGN`. Since 2021.2.

#### Style.textDecorationColor
<!-- id: N/workbook.Style#textDecorationColor -->

`workbook.Color | string`. Color of underlines and similar decorations: a `Color` enum value or a
color object. A string that is not a `Color` value raises `UNSUPPORTED_COLOR`. Since 2021.2.

#### Style.textDecorationLine
<!-- id: N/workbook.Style#textDecorationLine -->

`string`, from [`workbook.TextDecorationLine`](#workbooktextdecorationline). Any other value raises
`INVALID_TEXT_DECORATION_LINE`. Since 2021.2.

#### Style.textDecorationStyle
<!-- id: N/workbook.Style#textDecorationStyle -->

`string`, from [`workbook.TextDecorationStyle`](#workbooktextdecorationstyle). Any other value
raises `INVALID_TEXT_DECORATION_STYLE`. Since 2021.2.

### Color (object)
<!-- id: N/workbook.Color$object -->

An RGBA color object, returned by [`workbook.createColor`](#workbookcreatecoloroptions) and used by
`createStyle`. It shares its name with the [`workbook.Color`](#workbookcolor) enum of named colors;
this section is the object. Since 2021.2.

#### Color.alpha
<!-- id: N/workbook.Color$object#alpha -->

`number`. Opacity. This property's documentation gives the range as 0 to 255, while
`createColor` gives 0 to 1 (see the source-error note there). A value outside the documented range
raises `INVALID_ALPHA_VALUE`. Since 2021.2.

#### Color.blue
<!-- id: N/workbook.Color$object#blue -->

`number`, 0 to 255. A value outside that range raises `INVALID_COLOR_VALUE`. Since 2021.2.

#### Color.green
<!-- id: N/workbook.Color$object#green -->

`number`, 0 to 255. A value outside that range raises `INVALID_COLOR_VALUE`. Since 2021.2.

#### Color.red
<!-- id: N/workbook.Color$object#red -->

`number`, 0 to 255. A value outside that range raises `INVALID_COLOR_VALUE`. Since 2021.2.

### FontSize (object)
<!-- id: N/workbook.FontSize$object -->

A font size with an explicit unit, returned by
[`workbook.createFontSize`](#workbookcreatefontsizeoptions) and used by `createStyle`. It shares its
name with the [`workbook.FontSize`](#workbookfontsize) enum of named sizes; this section is the
object. Since 2021.2.

#### FontSize.size
<!-- id: N/workbook.FontSize$object#size -->

`number`. The size. Since 2021.2.

#### FontSize.unit
<!-- id: N/workbook.FontSize$object#unit -->

`string`, from [`workbook.Unit`](#workbookunit). Any other value raises `INVALID_UNIT`. Since 2021.2.

### PositionPercent (object)
<!-- id: N/workbook.PositionPercent -->

A percentage-based position, returned by
[`workbook.createPositionPercent`](#workbookcreatepositionpercentoptions) and used by
`createStyle`. Since 2021.2.

#### PositionPercent.percentX
<!-- id: N/workbook.PositionPercent#percentX -->

`number`. Horizontal position in percent. Since 2021.2.

#### PositionPercent.percentY
<!-- id: N/workbook.PositionPercent#percentY -->

`number`. Vertical position in percent. Since 2021.2.

### PositionUnits (object)
<!-- id: N/workbook.PositionUnits -->

A unit-based position, returned by
[`workbook.createPositionUnits`](#workbookcreatepositionunitsoptions) and used by `createStyle`.
Since 2021.2.

#### PositionUnits.unit
<!-- id: N/workbook.PositionUnits#unit -->

`string`, from [`workbook.Unit`](#workbookunit). Any other value raises `INVALID_UNIT`. Since 2021.2.

#### PositionUnits.x
<!-- id: N/workbook.PositionUnits#x -->

`number`. Horizontal offset. Since 2021.2.

#### PositionUnits.y
<!-- id: N/workbook.PositionUnits#y -->

`number`. Vertical offset. Since 2021.2.

### PositionValues (object)
<!-- id: N/workbook.PositionValues -->

A keyword-based position, using values such as `LEFT` or `CENTER`. Returned by
[`workbook.createPositionValues`](#workbookcreatepositionvaluesoptions) and used by `createStyle`.
Since 2021.2.

#### PositionValues.horizontal
<!-- id: N/workbook.PositionValues#horizontal -->

`workbook.Position`, given as a string. A value outside `Position` raises `INVALID_POSITION`.
Since 2021.2.

#### PositionValues.vertical
<!-- id: N/workbook.PositionValues#vertical -->

`workbook.Position`, given as a string. A value outside `Position` raises `INVALID_POSITION`.
Since 2021.2.

### PivotIntersection (object)
<!-- id: N/workbook.PivotIntersection -->

One computed cell of a pivot, returned (in an array) by
[`Workbook.runPivot`](#workbookrunpivotoptions). All its properties are read-only. Since 2021.2.

#### PivotIntersection.column
<!-- id: N/workbook.PivotIntersection#column -->

`workbook.DataDimensionValue | workbook.SectionValue`. The column position. Read-only. Since 2021.2.

#### PivotIntersection.measureValues
<!-- id: N/workbook.PivotIntersection#measureValues -->

`workbook.MeasureValue[]`. The measure values at this cell. Read-only. Since 2021.2.

#### PivotIntersection.row
<!-- id: N/workbook.PivotIntersection#row -->

`workbook.DataDimensionValue | workbook.SectionValue`. The row position. Read-only. Since 2021.2.

### DataDimensionValue (object)
<!-- id: N/workbook.DataDimensionValue -->

The value of a data dimension at a pivot position. It appears as the `row` or `column` of an
intersection. Since 2021.2.

#### DataDimensionValue.dataDimension
<!-- id: N/workbook.DataDimensionValue#dataDimension -->

`workbook.DataDimension`. Which dimension this value belongs to. Read-only. Since 2021.2.

#### DataDimensionValue.itemValues
<!-- id: N/workbook.DataDimensionValue#itemValues -->

`workbook.DataDimensionItemValue[]`. One value per dimension item. Read-only. Since 2021.2.

> [!SOURCE-ERROR] The source's parent-object row for this property names `CalculatedMeasure`, which
> looks like a copy error. The spec places the property on `DataDimensionValue`.
<!-- source-error: N/workbook.DataDimensionValue#itemValues | - | parent_object_row_says_calculatedmeasure -->

### DataDimensionItemValue (object)
<!-- id: N/workbook.DataDimensionItemValue -->

The value of one dimension item at a pivot position. Since 2021.2.

#### DataDimensionItemValue.item
<!-- id: N/workbook.DataDimensionItemValue#item -->

Declared type `workbook.DataDimension`. Read-only. Since 2021.2.

> [!SOURCE-ERROR] The type row says `DataDimension`, while the member descriptions talk about an
> item, which would be a `DataDimensionItem`. The spec marks this for review; inspect the value at
> runtime rather than relying on either type.
<!-- source-error: N/workbook.DataDimensionItemValue#item | - | item_type_row_says_datadimension_members_say_item -->

#### DataDimensionItemValue.value
<!-- id: N/workbook.DataDimensionItemValue#value -->

`string | number | boolean | workbook.Record | workbook.Currency | workbook.Range | workbook.Duration`.
The actual value. Read-only. Since 2021.2.

### SectionValue (object)
<!-- id: N/workbook.SectionValue -->

The position of a section in pivot results. It appears as the `row` or `column` of an intersection.
Since 2021.2.

#### SectionValue.section
<!-- id: N/workbook.SectionValue#section -->

`workbook.Section`. The section. Read-only. Since 2021.2.

### MeasureValue (object)
<!-- id: N/workbook.MeasureValue -->

One measure's value in a pivot intersection. Since 2021.2.

#### MeasureValue.measure
<!-- id: N/workbook.MeasureValue#measure -->

Declared type `workbook.MeasureValue`. Read-only. Since 2021.2.

> [!SOURCE-ERROR] The source's type row makes this property a `MeasureValue`, the same type as the
> object that holds it. That looks wrong, but the spec does not give the intended type. Inspect the
> value at runtime rather than relying on the declared type.
<!-- source-error: N/workbook.MeasureValue#measure | - | type_row_says_measurevalue -->

#### MeasureValue.value
<!-- id: N/workbook.MeasureValue#value -->

`string | number | boolean | workbook.Record | workbook.Currency | workbook.Range | workbook.Duration`.
The computed value. Read-only. Since 2021.2.

## Enums

The spec gives only the value names for these enums (no underlying strings), except
`AspectType`. Reference them through the module, as in `workbook.ChartType.LINE`.

### workbook.Aggregation
<!-- id: N/workbook.Aggregation -->

How a data measure aggregates. Used by `createDataMeasure`. Since 2020.2.

| Value |
|---|
| `AVG` |
| `COUNT` |
| `COUNT_DISTINCT` |
| `MAX` |
| `MEDIAN` |
| `MIN` |
| `SUM` |

### workbook.AspectType
<!-- id: N/workbook.AspectType -->

How a measure shows in a chart series. Used by `createAspect`. Since 2020.2.

| Value | String |
|---|---|
| `COLOR` | `color` |
| `VALUE` | `value` |

### workbook.ChartType
<!-- id: N/workbook.ChartType -->

Kind of chart. Used by `createChart`. Since 2020.2.

| Value |
|---|
| `AREA` |
| `BAR` |
| `COLUMN` |
| `LINE` |

### workbook.ConstantType
<!-- id: N/workbook.ConstantType -->

Data type of a constant expression. Used by `createConstant`. Since 2020.2.

| Value |
|---|
| `BOOLEAN` |
| `CURRENCY` |
| `DATE` |
| `DATE_TIME` |
| `DECIMAL` |
| `DURATION` |
| `INTEGER` |
| `NUMBER` |
| `RANGE` |
| `RECORD` |
| `TEXT` |

### workbook.DateTimeHierarchy
<!-- id: N/workbook.DateTimeHierarchy -->

A date-time hierarchy: whether periods roll up by months or by weeks. Since 2020.2. The source names
no function or parameter that consumes it. Several expression types take a parameter called
`hierarchy` (`DATE_TIME_PROPERTY`, `CHILD_OF`, `HIERARCHY`), but the source does not say whether
this enum is what those parameters expect.

| Value |
|---|
| `MONTH_BASED` |
| `WEEK_BASED` |

### workbook.DateTimeProperty
<!-- id: N/workbook.DateTimeProperty -->

The part of a date-time to extract. It is used by the `DATE_TIME_PROPERTY` expression type (its
`property` parameter). Since 2020.2.

| Value |
|---|
| `DATE` |
| `DAY_OF_MONTH` |
| `DAY_OF_WEEK` |
| `MONTH` |
| `QUARTER` |
| `WEEK_OF_YEAR` |
| `YEAR` |

### workbook.ExpressionType
<!-- id: N/workbook.ExpressionType -->

The functions available to [`workbook.createExpression`](#workbookcreateexpressionoptions)
(`options.functionId`). Each function expects a specific set of parameter names in
`options.parameters`, spelled exactly as below. A trailing `?` marks a parameter that may be
omitted. Since 2020.2.

Behaviour notes from the spec:

- **Integers vs decimals:** a `CONSTANT` built from a JavaScript number can't tell a decimal from an
  integer by itself. Set the constant's type (see [`workbook.ConstantType`](#workbookconstanttype)).
- **Currency conversion:** if `targetCurrency` and `date` are left out, `CURRENCY_CONVERSION`
  converts to the user's subsidiary currency at today's date.
- **Date selectors:** if no anchor is given, date selector expressions anchor on today.
- **Fields:** rather than building `FIELD` expressions by hand, get field expressions from N/dataset's
  `Dataset.getExpressionFromColumn`.
- **HIERARCHY_TO_TEXT:** joins hierarchy levels into one string. For example, levels `L1`, `L1.1`
  and `L1.1.1` with a `/` delimiter give `L1/L1.1/L1.1.1`.
- **TRUNCATE_DATE_TIME:** cuts a date-time down to the given unit, setting every smaller unit to
  zero.
- **SIMPLE_CONSOLIDATE:** in the pivot tutorial it consolidates amounts using the user's subsidiary
  and the current period.
- **Calculated measures:** a calculated measure's top-level expression must be `PLUS`, `MINUS`,
  `DIVIDE` or `MULTIPLY`; other types can sit inside it as operands.

| Value | Parameters |
|---|---|
| `AND` | `expressions` |
| `ANY_OF` | `expression`, `set` |
| `BETWEEN` | `expression`, `min`, `max` |
| `CHILD_OF` | `child`, `parent`, `hierarchy` |
| `COMPARE` | `comparisonType`, `operand1`, `operand2`. `comparisonType` is one of `EQUAL`, `GREATER`, `GREATER_OR_EQUAL`, `LESS`, `LESS_OR_EQUAL` |
| `CONSTANT` | `type`, `value` |
| `CURRENCY_CONVERSION` | `expression`, `targetCurrency?`, `date?` |
| `DATE_RANGE_SELECTOR_ID` | `expression`, `anchor?` |
| `DATE_SELECTOR_ID` | `expression`, `anchor`, `startExpected` |
| `DATE_TIME_PROPERTY` | `dateTime`, `property`, `hierarchy` |
| `DIVIDE` | `operand1`, `operand2` |
| `EQUALS` | `operand1`, `operand2` |
| `FIELD` | `type`, `fieldId`, `datasetId` |
| `HIERARCHY` | `hierarchy`, `expression` |
| `HIERARCHY_TO_TEXT` | `hierarchy`, `levelToText`, `delimiter` |
| `IN_RANGE` | `value`, `range` |
| `IS_NULL` | `expression` |
| `MEASURE_VALUE` | `measure` |
| `MINUS` | `operand1`, `operand2` |
| `MULTIPLY` | `operand1`, `operand2` |
| `NOT` | `expression` |
| `OR` | `expressions` |
| `PLUS` | `operand1`, `operand2` |
| `RECORD_DISPLAY_VALUE` | `record` |
| `RECORD_KEY` | `record` |
| `SIMPLE_CONSOLIDATE` | `expression` |
| `TRANSLATE` | `key`, `collection` |
| `TRUNCATE_DATE_TIME` | `dateTime`, `unit` |
| `DATASET_COLUMN` | not documented |
| `IF` | not documented |
| `LAMBDA` | not documented |

The Workbook Builder plug-in sample also passes `expression` to `NOT` (inside a "not any of" filter
expression), which agrees with this table.

> [!SOURCE-ERROR] The source lists `DATASET_COLUMN` and `IF` without documenting their parameters
> (and the spec has no parameters for `LAMBDA` either). Don't guess the parameter names; test them
> or avoid these types.
<!-- source-error: N/workbook.ExpressionType | - | dataset_column_and_if_types_undocumented -->

### workbook.Stacking
<!-- id: N/workbook.Stacking -->

How chart series stack. Used by `createChart`. Since 2020.2.

| Value |
|---|
| `DISABLED` |
| `NORMAL` |
| `PERCENT` |

### workbook.TotalLine
<!-- id: N/workbook.TotalLine -->

Where a dimension or section shows its total. Used by `createDataDimension` and `createSection`.
Since 2020.2.

| Value |
|---|
| `FIRST_LINE` |
| `HIDDEN` |
| `LAST_LINE` |

### workbook.Color
<!-- id: N/workbook.Color -->

Named colors for `createStyle`'s color options. For a custom RGBA color, use
[`createColor`](#workbookcreatecoloroptions), which returns the [`Color` object](#color-object).
Since 2021.2.

| Value |
|---|
| `BLACK` |
| `BLUE` |
| `BROWN` |
| `GRAY` |
| `GREEN` |
| `ORANGE` |
| `PINK` |
| `PURPLE` |
| `RED` |
| `WHITE` |
| `YELLOW` |

### workbook.FontSize
<!-- id: N/workbook.FontSize -->

Named font sizes for `createStyle`. For an exact size, use
[`createFontSize`](#workbookcreatefontsizeoptions). Since 2021.2.

| Value |
|---|
| `LARGE` |
| `LARGER` |
| `MEDIUM` |
| `SMALL` |
| `SMALLER` |
| `XX_LARGE` |
| `XX_SMALL` |
| `X_LARGE` |
| `X_SMALL` |

### workbook.FontStyle
<!-- id: N/workbook.FontStyle -->

Used by `createStyle`. Since 2021.2.

| Value |
|---|
| `ITALIC` |
| `NORMAL` |
| `OBLIQUE` |

### workbook.FontWeight
<!-- id: N/workbook.FontWeight -->

Used by `createStyle`. Since 2021.2.

| Value |
|---|
| `BOLD` |
| `NORMAL` |

### workbook.Image
<!-- id: N/workbook.Image -->

Built-in background images, used by `Style.backgroundImage` (`createStyle`'s `backgroundImage`).
Since 2021.2.

| Value |
|---|
| `EXCLAMATION` |
| `QUESTION` |
| `SMILE` |

### workbook.Position
<!-- id: N/workbook.Position -->

Position keywords for `PositionValues.horizontal` and `PositionValues.vertical`. Since 2021.2.

| Value |
|---|
| `BOTTOM` |
| `CENTER` |
| `LEFT` |
| `RIGHT` |
| `TOP` |

### workbook.TemporalUnit
<!-- id: N/workbook.TemporalUnit -->

Time units, used by `createDuration`. Since 2021.2.

| Value |
|---|
| `CENTURIES` |
| `DAYS` |
| `DECADES` |
| `ERA` |
| `HALF_DAYS` |
| `HOURS` |
| `MICROS` |
| `MILLENIA` |
| `MILLIS` |
| `MINUTES` |
| `MONTHS` |
| `NANOS` |
| `SECONDS` |
| `WEEKS` |
| `YEARS` |

### workbook.TextAlign
<!-- id: N/workbook.TextAlign -->

Used by `createStyle`. Since 2021.2.

| Value |
|---|
| `CENTER` |
| `JUSTIFY` |
| `LEFT` |
| `RIGHT` |

### workbook.TextDecorationLine
<!-- id: N/workbook.TextDecorationLine -->

The decoration line drawn on text. You set it through `createStyle`'s `textDecorationLine` option.
Since 2021.2.

| Value |
|---|
| `LINE_THROUGH` |
| `NONE` |
| `OVERLINE` |
| `UNDERLINE` |

### workbook.TextDecorationStyle
<!-- id: N/workbook.TextDecorationStyle -->

The style of the decoration line. Used by `createStyle`'s `textDecorationStyle` option. Since
2021.2.

> [!SOURCE-ERROR] In one place the source spells the option as `textDecoractionStyle` [sic]. The spec
> uses `textDecorationStyle` for `createStyle`. Use that spelling, and verify if the option seems
> ignored.
<!-- source-error: N/workbook.TextDecorationStyle | - | parameter_name_misspelled_textdecoractionstyle -->

| Value |
|---|
| `DASHED` |
| `DOTTED` |
| `DOUBLE` |
| `SOLID` |
| `WAVY` |

### workbook.Unit
<!-- id: N/workbook.Unit -->

CSS-style length units, used by `createPositionUnits`, `createFontSize`, `FontSize.unit` and
`PositionUnits.unit`. Since 2020.2.

| Value |
|---|
| `CH` |
| `CM` |
| `EM` |
| `EX` |
| `IN` |
| `MM` |
| `PC` |
| `PT` |
| `PX` |
| `REM` |
| `VH` |
| `VMAX` |
| `VMIN` |
| `VW` |

## Examples

**Building a workbook with a table view and a pivot.** This custom module builds both views over one
saved dataset of support-case time entries. The table highlights rows with heavy hours. The pivot
puts assignees on rows and case status on columns, and turns big totals red. The dataset ID and
column aliases are invented. Use the aliases defined in your own dataset.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/workbook', 'N/dataset', 'N/query'], (workbook, dataset, query) => {
  const buildCaseHoursWorkbook = () => {
    const caseHours = dataset.load({ id: 'custdataset_case_time_entries' });

    const assigneeExpr = caseHours.getExpressionFromColumn({ alias: 'assignee' });
    const statusExpr = caseHours.getExpressionFromColumn({ alias: 'casestatus' });
    const hoursExpr = caseHours.getExpressionFromColumn({ alias: 'hours' });

    // ---- Table view: yellow background on entries above 40 hours ----
    const heavyEntry = workbook.createConditionalFormat({
      rules: [
        workbook.createConditionalFormatRule({
          filter: workbook.createTableColumnFilter({
            operator: query.Operator.GREATER,
            values: [40]
          }),
          style: workbook.createStyle({
            backgroundColor: workbook.Color.YELLOW,
            fontWeight: workbook.FontWeight.BOLD
          })
        })
      ]
    });

    const entryTable = workbook.createTable({
      id: 'case_time_table',
      name: 'Time entries',
      dataset: caseHours,
      columns: [
        workbook.createTableColumn({
          datasetColumnAlias: 'assignee',
          label: 'Assignee',
          conditionalFormats: [], // the spec marks this option as required
          sort: workbook.createSort({ ascending: true })
        }),
        workbook.createTableColumn({
          datasetColumnAlias: 'hours',
          label: 'Hours',
          width: 90,
          conditionalFormats: [heavyEntry]
        })
      ]
    });

    // ---- Pivot: total hours, assignee (rows) x status (columns) ----
    const assigneeItem = workbook.createDataDimensionItem({ label: 'Assignee', expression: assigneeExpr });
    const assigneeDim = workbook.createDataDimension({
      items: [assigneeItem],
      totalLine: workbook.TotalLine.LAST_LINE
    });

    const totalHours = workbook.createDataMeasure({
      label: 'Total hours',
      aggregation: workbook.Aggregation.SUM,
      expression: hoursExpr
    });
    const statusItem = workbook.createDataDimensionItem({ label: 'Status', expression: statusExpr });
    const statusDim = workbook.createDataDimension({
      items: [statusItem],
      children: [totalHours]
    });

    const over100 = workbook.createExpression({
      functionId: workbook.ExpressionType.COMPARE,
      parameters: {
        comparisonType: 'GREATER',
        operand1: workbook.createExpression({
          functionId: workbook.ExpressionType.MEASURE_VALUE,
          parameters: { measure: totalHours }
        }),
        operand2: workbook.createConstant({ constant: 100, type: workbook.ConstantType.INTEGER })
      }
    });

    const redTotals = workbook.createReportStyle({
      selectors: [
        workbook.createMeasureValueSelector({
          rowSelector: workbook.createDimensionSelector({ dimension: assigneeDim }),
          columnSelector: workbook.createDimensionSelector({ dimension: statusDim }),
          measureSelector: workbook.createMeasureSelector({ measures: [totalHours] })
        })
      ],
      rules: [
        workbook.createReportStyleRule({
          expression: over100,
          style: workbook.createStyle({ color: workbook.Color.RED })
        })
      ]
    });

    const sortAxisBy = (dimension, item, ascending) =>
      workbook.createSortDefinition({
        selector: workbook.createDimensionSelector({ dimension }),
        sortBys: [
          workbook.createSortByDataDimensionItem({
            item,
            sort: workbook.createSort({ ascending })
          })
        ]
      });

    const hoursPivot = workbook.createPivot({
      id: 'case_hours_pivot',
      name: 'Hours by assignee and status',
      dataset: caseHours,
      rowAxis: workbook.createPivotAxis({
        root: assigneeDim,
        sortDefinitions: [sortAxisBy(assigneeDim, assigneeItem, true)]
      }),
      columnAxis: workbook.createPivotAxis({
        root: statusDim,
        sortDefinitions: [sortAxisBy(statusDim, statusItem, true)]
      }),
      // Spec types this option as a single ReportStyle; Pivot.reportStyles is an array.
      reportStyles: [redTotals]
    });

    return workbook.create({
      name: 'Support case hours',
      description: 'Assembled by script from the case time-entry dataset',
      tables: [entryTable],
      pivots: [hoursPivot]
    });
  };

  return { buildCaseHoursWorkbook };
});
```

**A Workbook Builder plug-in.** The plug-in builds a table of open quotes when the workbook list is
opened and hands it over through `context.workbook`. It adds a red "stale" highlight only when the
implementation was added by a user in one particular role, showing how a plug-in workbook can depend
on its context. It avoids `switch`, which plug-ins cannot use, and returns its entry function in a
frozen object. The dataset must already be saved; its ID and aliases are invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType workbookbuilderplugin
 */
define(['N/workbook', 'N/dataset', 'N/query'], (workbook, dataset, query) => {
  const createWorkbook = (context) => {
    const openQuotes = dataset.load({ id: 'custdataset_open_quotes' });

    const staleHighlight = workbook.createConditionalFormat({
      rules: [
        workbook.createConditionalFormatRule({
          filter: workbook.createTableColumnFilter({
            operator: query.Operator.GREATER,
            values: [30]
          }),
          style: workbook.createStyle({ color: workbook.Color.RED })
        })
      ]
    });

    // context.role is the internal ID of the role of the user who added the
    // implementation (read-only). 1042 stands for an invented sales-manager role.
    const SALES_MANAGER_ROLE = 1042;
    const ageFormats = context.role === SALES_MANAGER_ROLE ? [staleHighlight] : [];

    const quoteTable = workbook.createTable({
      id: 'open_quote_table',
      name: 'Open quotes',
      dataset: openQuotes,
      columns: [
        workbook.createTableColumn({
          datasetColumnAlias: 'customer_name',
          label: 'Customer',
          conditionalFormats: []
        }),
        workbook.createTableColumn({
          datasetColumnAlias: 'days_open',
          label: 'Days open',
          conditionalFormats: ageFormats,
          sort: workbook.createSort({ ascending: false })
        })
      ]
    });

    // The UI shows the implementation record's name, so reuse it here for consistency.
    context.workbook = workbook.create({
      name: context.name,
      description: context.description,
      tables: [quoteTable]
    });
  };

  return Object.freeze({ createWorkbook });
});
```

**Reading pivot results.** A scheduled script loads a saved workbook, runs one pivot and logs each
cell. A cell value can be a plain value or one of the wrapper objects (`Currency`, `Range`,
`Record`, `Duration`), so the helper checks which properties are present before it formats the
value. Each returned intersection costs 10 units.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/workbook', 'N/log'], (workbook, log) => {
  const asText = (value) => {
    if (value === null || typeof value !== 'object') {
      return String(value);
    }
    if ('amount' in value && 'id' in value) {
      return `${value.amount} ${value.id}`; // Currency
    }
    if ('start' in value && 'end' in value) {
      return `${value.start} .. ${value.end}`; // Range
    }
    if ('primaryKey' in value) {
      return `${value.name} [${value.primaryKey}]`; // Record
    }
    if ('units' in value) {
      return `${value.amount} ${value.units}`; // Duration
    }
    return JSON.stringify(value);
  };

  const positionLabel = (position) =>
    position.itemValues
      ? position.itemValues.map((iv) => asText(iv.value)).join(' / ')
      : '(section)';

  const execute = () => {
    const caseBook = workbook.loadWorkbook({ id: 'custworkbook_case_hours' });
    const cells = caseBook.runPivot({ id: 'case_hours_pivot' });

    cells.forEach((cell) => {
      const where = `${positionLabel(cell.row)} | ${positionLabel(cell.column)}`;
      cell.measureValues.forEach((mv) => {
        log.debug({ title: where, details: asText(mv.value) });
      });
    });

    log.audit({ title: 'Pivot cells read', details: String(cells.length) });
  };

  return { execute };
});
```
