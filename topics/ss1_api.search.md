# SuiteScript 1.0 searches
<!-- id: topic:ss1_api.search -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is [N/search](../modules/N_search.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## The pieces of a 1.0 search

| Piece | Role |
|---|---|
| [`nlobjSearchFilter`](../ss1/objects-record-search.md) | One condition: field, optional join, operator, values |
| [`nlobjSearchColumn`](../ss1/objects-record-search.md) | One value to return, optionally from a joined record and with a summary |
| [`nlobjSearchResult`](../ss1/objects-record-search.md) | One row of the results |
| [`nlapiSearchRecord`](../ss1/search-scheduling-context.md) | Runs the search and returns the rows |

These objects are not used by global search ([`nlapiSearchGlobal`](../ss1/search-scheduling-context.md))
or duplicate search ([`nlapiSearchDuplicate`](../ss1/search-scheduling-context.md)), which take
their own inputs.

## Filters, columns and joins

- The filter and column IDs that a record type accepts are listed with each record in this
  reference's [record pages](../gen/records/index.md) (search filters, search columns and search
  joins).
- To reach a field on a related record, pass the join ID as the `join` argument of the filter or
  column. For instance, a customer search can filter on the `customer` field of its `pricing` join
  with the `is` operator and a value such as 121.
- Not every join that the search UI offers is available to scripts.

## Operators and field types

The operators each field type accepts are listed in
[catalog:search_operator_field_types](../gen/catalogs/search_operator_field_types.md); that list
applies to every SuiteScript version. Where the catalog speaks of text types, it means these field
types: email address, free-form text, long text, password, percent, phone number, rich text and
text area.

## Summary types

A column can summarize with `group`, `count`, `sum`, `min`, `max` or `avg`. These names are the
same in every SuiteScript version.

## Relative date filters

The relative date values (such as `today` or `lastMonth`) are listed in
[catalog:search_date_filters](../gen/catalogs/search_date_filters.md), valid for every SuiteScript
version. They are matched without regard to case.

The catalog has two groups. The first, from `fiscalHalfBeforeLast` through `yesterday`, carries no
lifecycle tag. The second group is tagged as deprecated since 2015.1; the source notes that this
second group is still supported, so legacy scripts using these values keep working, but prefer a
value from the first group when repairing or converting code. The deprecated entries are:

- `lastFiscalHalfOneYearAgo`, `lastFiscalQuarterOneYearAgo`
- `lastMonthOneQuarterAgo`, `lastMonthOneYearAgo`, `lastMonthTwoQuartersAgo`, `lastMonthTwoYearsAgo`
- `lastQuarterTwoYearsAgo`
- `previousMonthsSameFiscalHalfLastYear`, `previousMonthsSameFiscalQuartherLastFiscalYear` [sic]
- `previousQuartersLastFiscalYear`, `previousQuartersThisFiscalYear`
- `sameHalfLastFiscalYear`, `sameHalfLastFiscalYearToDate`, `sameQuarterLastFiscalYear`,
  `sameQuarterLastFiscalYearToDate`
- `startOfLastHalfOneYearAgo`, `startOfLastMonthOneQuarterAgo`, `startOfLastMonthOneYearAgo`,
  `startOfLastQuarterOneYearAgo`, `startOfNextYear`
- `startOfSameHalfLastFiscalYear`, `startOfSameQuarterLastFiscalYear`
- `startOfTheHalfBeforeLast`, `startOfTheMonthBeforeLast`, `startOfTheQuarterBeforeLast`,
  `startOfTheWeekBeforeLast`, `startOfTheYearBeforeLast`
- `threeQuartersAgo`, `threeQuartersAgoToDate`, `threeYearsAgo`, `threeYearsAgoToDate`
- The number templates `daysagoxx`, `weeksagoxx`, `monthsagoxx`, `quartersagoxx`, `yearsagoxx`,
  `daysfromnowxx`, `weeksfromnowxx`, `monthsfromnowxx`, `quartersfromnowxx` and `yearsfromnowxx`,
  where `xx` stands for a number.

> [!SOURCE-ERROR] One deprecated value contains "quarther", which looks like a typo for "quarter".
> The source spelling is kept, and the source does not say which spelling the API actually
> matched; since the value is deprecated, use a current value from the catalog instead.
<!-- source-error: topic:ss1_api.search | - | deprecated_value_spelled_quarther -->

## Example (1.0, for reading legacy code)

Counting high-priority support cases per category with a summary search. Once one column groups,
every column needs a summary type.

```js
function urgentCaseCountsByCategory() {
  var filters = [new nlobjSearchFilter('priority', null, 'anyof', '1')];
  var columns = [
    new nlobjSearchColumn('category', null, 'group'),
    new nlobjSearchColumn('title', null, 'count')
  ];
  var rows = nlapiSearchRecord('supportcase', null, filters, columns) || [];
  var counts = {};
  for (var i = 0; i < rows.length; i++) {
    var label = rows[i].getText('category', null, 'group') || '(none)';
    counts[label] = parseInt(rows[i].getValue('title', null, 'count'), 10) || 0;
  }
  return counts;
}
```
