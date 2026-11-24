# SuiteQL joins
<!-- id: topic:suiteql.joins -->

Part of: [SuiteQL](suiteql.md)

SuiteQL supports five kinds of join: cross, inner, left outer, right outer and full outer. Some of
them can be written in more than one way. Whatever forms you use, keep to one style per statement:
as the [SuiteQL page](suiteql.md) explains, ANSI-style joins (`JOIN ... ON`) and Oracle-style joins
(conditions in `WHERE`, `(+)`) must not be mixed in one query.

The table and column names below are illustrative; look up the real ones in the
[Records Catalog](records_catalog.md) for your account.

## Join forms at a glance

| Join | Explicit keyword form | Implicit (Oracle-style) form |
|---|---|---|
| Inner | `INNER JOIN ... ON` | Tables listed in `FROM`, condition in `WHERE` |
| Left outer | `LEFT OUTER JOIN ... ON` or `LEFT JOIN ... ON` | `(+)` on the optional side in `WHERE` |
| Right outer | `RIGHT OUTER JOIN ... ON` | None |
| Full outer | `FULL OUTER JOIN ... ON` | None |
| Cross | `CROSS JOIN` (not over SuiteAnalytics Connect) | Tables listed in `FROM` with no join condition |

Workbooks join with a left outer join by default, so a query exported from a workbook will use that
form unless you change it.

## Inner joins

Both forms return only rows that have a match on each side:

```sql
SELECT t.tranid, e.entityid
FROM transaction t
INNER JOIN entity e ON e.id = t.entity
```

```sql
SELECT t.tranid, e.entityid
FROM transaction t, entity e
WHERE e.id = t.entity
```

## Left outer joins

Keep every row from the left table, with nulls where the right table has no match. `LEFT JOIN` is
shorthand for `LEFT OUTER JOIN`, and the Oracle `(+)` marker achieves the same thing in the
implicit style:

```sql
SELECT t.tranid, e.entityid
FROM transaction t
LEFT OUTER JOIN employee e ON e.id = t.employee
```

```sql
SELECT t.tranid, e.entityid
FROM transaction t, employee e
WHERE t.employee = e.id(+)
```

## Right and full outer joins

These exist only in the explicit keyword form; there is no implicit equivalent. (The
[SuiteQL page](suiteql.md) also lists the Oracle-style right outer join as unsupported.)

```sql
SELECT d.name, e.entityid
FROM employee e
FULL OUTER JOIN department d ON d.id = e.department
```

## Cross joins

A cross join pairs every row of one table with every row of the other, so it gets expensive quickly;
use it only on small tables. SuiteAnalytics Connect does not accept the `CROSS JOIN` keywords: write
the tables side by side in `FROM` instead.

```sql
SELECT s.name AS subsidiary_name, c.symbol AS currency_symbol
FROM subsidiary s, currency c
```

## Example

A scheduled script that lists customers together with their sales rep, keeping customers that have
no rep (left outer join).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const SQL = `
    SELECT c.id, c.entityid, e.entityid AS repname
    FROM customer c
    LEFT JOIN employee e ON e.id = c.salesrep
    WHERE c.isinactive = 'F'
    ORDER BY c.id`;

  const execute = () => {
    const rows = query.runSuiteQL({ query: SQL }).asMappedResults();
    const unassigned = rows.filter((row) => row.repname === null);
    log.audit({
      title: 'Customers without a sales rep',
      details: `${unassigned.length} of ${rows.length}`
    });
  };

  return { execute };
});
```
