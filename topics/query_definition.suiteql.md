# SuiteQL in N/query
<!-- id: topic:query_definition.suiteql -->

Part of: [Query definitions and components](query_definition.md)

See also: [SuiteQL](suiteql.md), [SuiteQL built-in functions](suiteql.builtin_functions.md),
[Records Catalog](records_catalog.md), [N/query](../modules/N_query.md).

[N/query](../modules/N_query.md) runs SuiteQL statements as well as query definitions. This page
covers the SuiteScript side; the language itself is described on the [SuiteQL](suiteql.md) page.

## What SuiteQL is based on

SuiteQL is a query language built on SQL. The [SuiteQL](suiteql.md) page gives its basis as two
dialects, SQL-92 and Oracle SQL: a statement may use either syntax, but only one of them per query,
and Oracle syntax is the recommended choice. The material behind this page mentions only SQL-92, so
read that as one of the two supported syntaxes, not as a restriction.

## Why use SuiteQL from a script

Compared with a query definition, a SuiteQL statement is more powerful and flexible, and in many
cases it performs best. The cost is that you must know the exact record type (table) and field
(column) names yourself. Those names come from the [Records Catalog](records_catalog.md) of the
account, not from the N/search IDs on this reference's record pages (`out/gen/records/`).

## Two ways to get a statement

1. **Convert a definition.** Build a query definition and call
   [`Query.toSuiteQL()`](../modules/N_query.md#querytosuiteql) to get an equivalent
   [`query.SuiteQL`](../modules/N_query.md#suiteql-object) object, then run that. Column aliases set
   through [`Column.alias`](../modules/N_query.md#columnalias) are not carried into the converted
   statement, so do not look results up by alias afterwards.
2. **Run an arbitrary statement** that you write yourself.

## The SuiteQL object

A [`query.SuiteQL`](../modules/N_query.md#suiteql-object) object exposes:

- [`SuiteQL.query`](../modules/N_query.md#suiteqlquery): the statement text;
- [`SuiteQL.params`](../modules/N_query.md#suiteqlparams): the parameter values, bound in order to the
  question marks (`?`) in the statement;
- [`SuiteQL.columns`](../modules/N_query.md#suiteqlcolumns) and
  [`SuiteQL.type`](../modules/N_query.md#suiteqltype).

## Running a statement

| Call | Result |
|---|---|
| [`SuiteQL.run()`](../modules/N_query.md#suiteqlrunoptions) | All rows in one result set |
| [`SuiteQL.runPaged()`](../modules/N_query.md#suiteqlrunpagedoptions) | Paged data |
| [`query.runSuiteQL()`](../modules/N_query.md#queryrunsuiteqloptions) | All rows in one result set, without building an object first |
| [`query.runSuiteQLPaged()`](../modules/N_query.md#queryrunsuiteqlpagedoptions) | Paged data, without building an object first |

- The two module-level functions take an arbitrary statement in any of three forms: a plain string,
  a `query.SuiteQL` object, or an object holding a `query` string and, optionally, a `params` array.
- For paged runs the page size defaults to 50 rows; it can be set from 5 up to 1,000.

## Typical uses

The scenarios the spec illustrates are: converting the query of a loaded workbook to SuiteQL, running
a hand-written query string, receiving a query as an argument to a RESTlet, reading a large result
page by page, and using a SuiteQL query as the input of a map/reduce script.

## Examples

### Converting a definition and inspecting the statement

Build a small definition over vendors, convert it, log the generated statement and its bound values,
then run it. The statement is read by position because aliases do not survive the conversion.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const execute = () => {
    const q = query.create({ type: query.Type.VENDOR });
    q.condition = q.createCondition({
      fieldId: 'isinactive',
      operator: query.Operator.IS,
      values: [false]
    });
    q.columns = [q.createColumn({ fieldId: 'entityid' }), q.createColumn({ fieldId: 'email' })];

    const sql = q.toSuiteQL();
    log.debug({ title: 'Generated SuiteQL', details: sql.query });
    log.debug({ title: 'Bound values', details: JSON.stringify(sql.params) });

    sql.run().results.forEach((row) => {
      log.debug({ title: `Vendor ${row.values[0]}`, details: row.values[1] || 'no email' });
    });
  };

  return { execute };
});
```

### Arbitrary statement read page by page

Read pending-approval purchase orders in pages of 200, binding the transaction type and status
through placeholders. Because transaction status is a composite value, the filter goes through
`BUILTIN.CF` (see [SuiteQL built-in functions](suiteql.builtin_functions.md)). Table, column and
status values are illustrative; confirm them in the [Records Catalog](records_catalog.md) and your
data.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const execute = () => {
    const paged = query.runSuiteQLPaged({
      query: `SELECT t.id, t.tranid, t.foreigntotal
              FROM transaction t
              WHERE t.type = ? AND BUILTIN.CF(t.status) = ?
              ORDER BY t.id`,
      params: ['PurchOrd', 'PurchOrd:A'],
      pageSize: 200
    });

    paged.pageRanges.forEach((range) => {
      const rows = paged.fetch({ index: range.index }).data.asMappedResults();
      rows.forEach((row) => {
        log.debug({ title: `PO ${row.tranid}`, details: `Total ${row.foreigntotal}` });
      });
    });
  };

  return { execute };
});
```
