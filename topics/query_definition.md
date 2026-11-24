# Query definitions and components
<!-- id: topic:query_definition -->

Sub-topics:

- [Query Formulas](query_definition.formulas.md)
- [Query Relative Dates](query_definition.relative_dates.md)
- [SuiteQL in N/query](query_definition.suiteql.md)

See also: [N/query](../modules/N_query.md), [SuiteQL](suiteql.md), [Records Catalog](records_catalog.md).

A query definition is the object you assemble with [N/query](../modules/N_query.md) to describe the
data you want: the record type to start from, the related record types to reach through joins, the
conditions that narrow the rows, the columns to return and their order. You build it piece by piece
and then run it.

## Building a definition step by step

1. Create the definition with [`query.create()`](../modules/N_query.md#querycreateoptions). Its
   starting record type, the query type, is a value of the [`query.Type`](../modules/N_query.md) enum;
   a definition that starts from customers, for instance, uses `query.Type.CUSTOMER`.
2. Add the first join with [`Query.autoJoin()`](../modules/N_query.md#queryautojoinoptions).
3. Add further joins with [`Query.autoJoin()`](../modules/N_query.md#queryautojoinoptions) or with
   [`Component.autoJoin()`](../modules/N_query.md#componentautojoinoptions) on a component you already
   hold.
4. Add conditions, columns and sorts (below), then run the definition.

## Components and joins

The definition is held in a [`Query`](../modules/N_query.md#query-object) object and is made of a
chain of [`Component`](../modules/N_query.md#component-object) objects, each one the child of the one
before it.

- There is one component for the query type and one more for every join you add.
- The first component, the root, is created automatically together with the definition. You reach it
  through [`Query.root`](../modules/N_query.md#queryroot); it is the child of the query object, which
  holds the query type.
- The second component is created by calling `autoJoin()`, `joinFrom()` or `joinTo()` on the `Query`
  object itself. Every component after that is created by calling the same three methods on a
  `Component`.
- The relationship a join expresses is fixed by two things: the field ID you pass to the join method
  and the join direction (`joinTo()` follows a field forward, `joinFrom()` walks it backwards;
  `autoJoin()` derives the relationship from the field ID).
- The last component in the chain represents its relationship to all of its parent components, so a
  column or condition created on it is reached through the whole join trail above it.

## Conditions

A condition is the query counterpart of a [`search.Filter`](../modules/N_search.md) in a saved
search: it narrows the results. The difference is that a [`query.Condition`](../modules/N_query.md#condition-object)
can contain other conditions, so the logic can be nested.

1. Create each condition with `createCondition()` on the query or on a component.
2. Combine conditions with the query's `and()`, `or()` and `not()` methods.
3. Assign the combined condition to the query's `condition` property.

A condition can also be a formula, passed to `createCondition()` through its formula option (see
[Query Formulas](query_definition.formulas.md)).

To compare a date with a point relative to today: create a relative date with
`query.createRelativeDate()` and a date ID, use it in `createCondition()`, and assign the condition
to the query (see [Query Relative Dates](query_definition.relative_dates.md)).

## Columns, aggregates, grouping and sorting

- **Columns.** A query column is the same thing as a search column: one column of the results.
  Create each with `createColumn()` on the query or on a component, then assign the list to the
  query's `columns` property. A column can also be a formula, through the column's formula option.
- **Aggregates.** Aggregate functions can be used in both conditions and columns.
- **Grouping.** Results are grouped through the column's group-by option.
- **Sorts.** Create each with `createSort()` on the query or on a component, then assign the list to
  the query's `sort` property.

## Running the definition and reading results

Run the definition with [`Query.run()`](../modules/N_query.md#queryrunoptions) for one result set,
or with [`Query.runPaged()`](../modules/N_query.md#queryrunpagedoptions) for paged results. After a
plain run the rows are in [`ResultSet.results`](../modules/N_query.md#resultsetresults); after a paged
run, walk the pages with [`PagedData.iterator()`](../modules/N_query.md#pageddataiterator).

## Vocabulary

| Term | Meaning |
|---|---|
| Query definition | The query type plus the joins built on it |
| Query type | The record type being queried, chosen from `query.Type` |
| Component | A building block of a query: the root, or one join |
| Join | A query across a field that two record types share |
| Condition | A criterion that narrows the results |
| Formula | An expression used to build conditions and columns |
| Relative date | A date expressed relative to the current date |
| Column | One column of the query results |
| Aggregate function | A function that computes a single value from the values in a column |
| Group | A set of unique groups formed from a column's values |
| Sort | The order applied to a results column |
| Result | A single row of a result set |
| Result set | The set of results a query returns |
| Paged data | The set of results returned page by page |
| Page | A single page of paged results |
| Page range | A set of pages |

## Example

A scheduled script that lists customers created in the last six months who have a sales rep
assigned, returning the rep's email through a join and sorting by company name. Field IDs for
N/query come from the [Records Catalog](records_catalog.md), not from this reference's N/search record
pages; confirm them in your account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const execute = () => {
    const q = query.create({ type: query.Type.CUSTOMER });
    const rep = q.autoJoin({ fieldId: 'salesrep' });

    const recent = q.createCondition({
      fieldId: 'datecreated',
      operator: query.Operator.ON_OR_AFTER,
      values: [query.createRelativeDate({ dateId: query.DateId.MONTHS_AGO, value: 6 })]
    });
    const hasRep = q.createCondition({ fieldId: 'salesrep', operator: query.Operator.EMPTY_NOT });
    q.condition = q.and(recent, hasRep);

    const nameCol = q.createColumn({ fieldId: 'companyname' });
    q.columns = [nameCol, rep.createColumn({ fieldId: 'email' })];
    q.sort = [q.createSort({ column: nameCol })];

    q.run().results.forEach((row) => {
      const [company, repEmail] = row.values;
      log.debug({ title: company, details: `Rep: ${repEmail}` });
    });
  };

  return { execute };
});
```
