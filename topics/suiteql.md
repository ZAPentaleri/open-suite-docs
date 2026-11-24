# SuiteQL
<!-- id: topic:suiteql -->

Part of: [Analytic APIs](analytic_apis.md)

Sub-topics:
- [SuiteQL built-in functions](suiteql.builtin_functions.md)
- [SuiteQL joins](suiteql.joins.md)

See also: [SuiteQL in the query definition](query_definition.suiteql.md), [Records Catalog](records_catalog.md),
[N/query](../modules/N_query.md).

SuiteQL is a SQL dialect for querying NetSuite data. It runs against the analytics data source,
which is the same data that workbooks use.

## Where you can run it

SuiteQL is reachable from three places:

- **SuiteAnalytics Connect** (ODBC/JDBC). Many concurrent Connect sessions slow queries down.
- **SuiteScript**, through [N/query](../modules/N_query.md): build a [`query.SuiteQL`](../modules/N_query.md)
  object, convert a query definition with [`Query.toSuiteQL()`](../modules/N_query.md), or run a
  statement directly with [`query.runSuiteQL()`](../modules/N_query.md) or
  [`query.runSuiteQLPaged()`](../modules/N_query.md).
- **REST web services**: send a POST request to the SuiteQL resource with the statement in the
  request body's `q` property.

> [!SOURCE-ERROR] One source page lists only two of these channels and leaves out REST web services,
> while its parent page names all three. Treat REST web services as a supported channel, but confirm
> in your account if you depend on it.
<!-- source-error: topic:suiteql | - | availability_list_omits_rest_web_services -->

Access follows workbook rules: the role-based restrictions that apply to workbooks apply to SuiteQL,
and some roles have no access to the analytics data source at all.

## Dialect

SuiteQL accepts both SQL-92 and Oracle SQL syntax, but **a single query must use one of them, not a
mix**. Prefer Oracle syntax: SQL-92 queries can run into timeouts that cannot be recovered from.
In the same spirit, never combine ANSI-style joins and Oracle-style joins in one statement (see
[SuiteQL joins](suiteql.joins.md)).

Only the functions on the allowed list can be called; anything else is rejected. This restriction
exists to prevent SQL injection.

- Allowed functions: [catalog:suiteql_functions](../gen/catalogs/suiteql_functions.md)
- Functions you cannot use, each with the suggested alternative (the list gives examples and is not
  complete): [catalog:suiteql_unsupported_functions](../gen/catalogs/suiteql_unsupported_functions.md)
- NetSuite-specific built-ins: [catalog:suiteql_builtin_functions](../gen/catalogs/suiteql_builtin_functions.md),
  explained in [SuiteQL built-in functions](suiteql.builtin_functions.md)

### Syntax rules

| Rule | What to do |
|---|---|
| String concatenation | Use `\|\|`; `+` does not concatenate |
| Dates | There are no date literals; build dates with `TO_DATE` |
| `IN (...)` lists | At most 1,000 arguments |
| `WITH` clauses | Not supported |
| Oracle-style right outer join | Not supported |
| Square brackets around names | Not supported |
| Quoted field names inside sub-selects | Not allowed |
| SQL-92 syntax: `NULL` in character fields, `DEFAULT` column clauses, sub-queries in the select list | Not supported |

### Values and field context

Unless you ask otherwise, fields come back in the `RAW` field context (see
[`query.FieldContext`](../modules/N_query.md) for the other contexts).

When you build a query object in N/query, take its type from the query module's type enum; type
names given as strings are matched without regard to case.

## Result column names

Do not rely on the letter case of column names in results; normalize case before you look a column
up. In particular, `ResultSet.asMappedResults()` returns each alias in lower case, so an alias written
`InvoiceTotal` in the statement shows up as the key `invoicetotal`.

## Finding table and column names

The names SuiteQL uses come from the analytics data source, not from the search IDs listed in this
reference's record pages (`out/gen/records/`, which are N/search IDs). Look them up in:

- the [Records Catalog](records_catalog.md),
- customization lists in the account,
- the workbook UI, or
- a workbook exported as SuiteQL. Exporting needs the `AnalyticsAdmin` permission, and the exported
  statement should be run by the same user with the same role, because workbook and SuiteQL share the
  analytics data source and its access rules.

## Writing fast queries

- Select the columns you need instead of `SELECT *`, and avoid nested selects.
- Filter on indexed fields; `id` and `lastmodifieddate` are examples.
- For incremental loads, filter on `lastmodifieddate`.
- Compare values of the same type.
- Read large volumes in batches by ranges of `id`.
- Keep joins few.
- Split `OR` conditions where you can.
- Do not sort unless you need the order, and avoid the `TOP` clause.
- Calculated fields (for example the customer's on-credit-hold flag) and long-text (`WLONGVARCHAR`)
  fields (for example an item's featured description) are slow to query.

## Known quirks

- Sorting on a CLOB column only looks at its first 250 characters.
- Filtering transaction status values by transaction type is unreliable; match with `LIKE` instead.
- Hierarchy levels returned by the built-in functions can be off by one.
- Application data stored on custom field subtabs is not available.
- When results are exported to CSV, text values are wrapped in double quotes, and any value that
  starts with `-`, `+`, `=`, `@`, a tab or a line-break character gets a leading single quote.

## Example

Pull invoices changed since a given day, in id-ordered batches so each call stays under the
`runSuiteQL` row cap. The table and column names are illustrative; confirm them in the Records
Catalog for your account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const SQL = `
    SELECT t.id, t.tranid, t.lastmodifieddate
    FROM transaction t
    WHERE t.type = ?
      AND t.lastmodifieddate >= TO_DATE(?, 'YYYY-MM-DD')
      AND t.id > ?
    ORDER BY t.id`;

  const execute = () => {
    let lastId = 0;
    let batch;
    do {
      batch = query.runSuiteQL({ query: SQL, params: ['CustInvc', '2026-10-01', lastId] })
        .asMappedResults();
      batch.forEach((row) => {
        log.debug({ title: `Invoice ${row.tranid}`, details: row.lastmodifieddate });
        lastId = row.id;
      });
    } while (batch.length > 0);
  };

  return { execute };
});
```
