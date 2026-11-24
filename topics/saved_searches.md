# Saved searches
<!-- id: topic:saved_searches -->

Sub-topics:

- [Saved search definition options](saved_searches.definition.md)

See also: [N/search](../modules/N_search.md), [Search formulas](search_formulas.md),
[SuiteQL](suiteql.md).

A saved search is a stored query definition: criteria, result columns, sorting and audience. Scripts
create, load and run them through [N/search](../modules/N_search.md)
([`search.create`](../modules/N_search.md#searchcreateoptions),
[`search.load`](../modules/N_search.md#searchloadoptions),
[`Search.save()`](../modules/N_search.md#searchsave)). The same definitions feed many other parts of
NetSuite: list views, preferred forms, reminders, KPIs, dashboard portlets, email alerts and the web
site.

## Scope and joins

Every search targets exactly one record type. The only exception is global search
([`search.global`](../modules/N_search.md#searchglobaloptions)), which looks across record types.
To bring in data from other records, a search reaches them through related-record fields (joins),
not by querying a second record type.

## Sorting and output

A search always has at least one sort. If you define none, results are sorted by the first result
column. Results can be presented as a normal list, a report, a grid or a graph.

Integer values are displayed with up to 13 digits; a longer integer is shown as a run of `#`
characters. If you need to see the full value, return it as text with a formula (see
[Search formulas](search_formulas.md)).

## Signs on account amounts

Since 2014.1, searches treat expense accounts as debit accounts when they sign amounts. If you want a
different sign convention in your results, flip the sign per account type with a `DECODE` on the
account type in a formula column. An illustrative formula (the account-type labels must match what
your account returns):

```text
DECODE({accounttype}, 'Income', -1, 'Other Income', -1, 1) * {amount}
```

## Persisting large result sets

Persisting a search runs it asynchronously and writes the results to a CSV file; a persisted run can
take up to 3 hours. The CSV file is kept for 7 days. A persisted run passes through these statuses:
pending, completed, timed out, or failed. A run that is still pending after 24 hours is marked as
failed. Persisting requires the Persist Search permission (`LIST_BIG_SEARCH`).

The search execution log keeps 60 days of history.

## Permissions and access

| Permission | What it allows |
|---|---|
| Perform Search | View and run searches; the Full level is needed to save them |
| Publish Search | Create, share and email searches that other users can see |
| Persist Search | Run a search asynchronously to CSV (up to 3 hours) |
| Export Lists | Export search results |
| Tableau workbooks export | Export to Tableau workbooks |

To save transaction searches, a role also needs Find Transaction (`TRAN_FIND`) at the Create level.
All of these are in the [permissions catalog](../gen/catalogs/permissions.md).

- You can use a search somebody else created only if it is public or you belong to its audience.
- Each custom field has its own search access setting: the field can be editable in searches,
  usable only for running searches, or not available to searches at all.
- Customers, vendors and most partners cannot use saved searches, not even public ones.
- The saved-search menu shows at most 100 saved searches.

### Running unrestricted

The Run Unrestricted option lets the search show results beyond the record restrictions of the user
who runs it. The time-and-expenses restriction setting still overrides it.

### Ownership and scheduled email

If a search's owner changes, its scheduled email stops being sent until someone saves the search
again.

## Using a search as a KPI

To serve as a key performance indicator, a saved search must have no date criteria, must have a
summary field, and must offer date as an available filter.

> [!SOURCE-ERROR] The source's wording of the KPI rule can be read as "the search must have only one
> field, and it must have a summary type", which is stricter than "one summary field". The spec does
> not settle which reading is meant; test the search in the KPI setup before relying on either.
<!-- source-error: topic:saved_searches | - | kpi_rule_reads_have_only_field_with_summary -->

## Example

Build a summary search of last month's posted amounts grouped by account type, with the sign
flipped for income, and an explicit sort so the order is not left to the default.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const typeColumn = search.createColumn({
      name: 'accounttype',
      summary: search.Summary.GROUP,
      sort: search.Sort.ASC
    });
    const signedColumn = search.createColumn({
      name: 'formulacurrency',
      summary: search.Summary.SUM,
      formula: "DECODE({accounttype}, 'Income', -1, 1) * {amount}",
      label: 'Signed amount'
    });

    const byType = search.create({
      type: search.Type.TRANSACTION,
      filters: [
        ['posting', search.Operator.IS, 'T'],
        'AND',
        ['trandate', search.Operator.WITHIN, 'lastMonth']
      ],
      columns: [typeColumn, signedColumn]
    });

    byType.run().each((row) => {
      log.debug({
        title: row.getValue(typeColumn),
        details: row.getValue(signedColumn)
      });
      return true;
    });
  };

  return { execute };
});
```
