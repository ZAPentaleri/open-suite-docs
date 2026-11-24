# SuiteQL built-in functions
<!-- id: topic:suiteql.builtin_functions -->

Part of: [SuiteQL](suiteql.md)

See also: [SuiteQL in N/query](query_definition.suiteql.md), [Records Catalog](records_catalog.md),
[N/query](../modules/N_query.md).

Besides the standard SQL functions, SuiteQL offers NetSuite-specific helpers for jobs plain SQL
handles poorly: display values, composite status codes, currency consolidation and conversion,
hierarchies, multi-select filtering, named groups, and relative periods and dates. The full list is
the [SuiteQL built-in functions catalog](../gen/catalogs/suiteql_builtin_functions.md); the general
allowed-function list is [catalog:suiteql_functions](../gen/catalogs/suiteql_functions.md).

Every built-in must be called with the `BUILTIN.` prefix, as in `BUILTIN.DF(...)`; the bare name is
not accepted. Table and column names in the snippets below are illustrative only. SuiteQL names come
from the account's [Records Catalog](records_catalog.md), not from the N/search IDs on this
reference's record pages, so check them before use.

## Display values and composite fields

**`BUILTIN.DF`** returns the display value of a field (for example the referenced customer's name
instead of its internal ID) without you writing the join to the referenced table.

**`BUILTIN.CF`** returns the criteria value of a composite field. Transaction status is the usual
case: its criteria value is the status prefixed with the transaction type. Whenever you filter on a
field with a composite key like this, compare through `BUILTIN.CF`. The status literal below only
illustrates that shape; take real values from your own data.

```sql
SELECT t.tranid,
       BUILTIN.DF(t.entity) AS customer_name,
       BUILTIN.DF(t.status) AS status_label
FROM transaction t
WHERE BUILTIN.CF(t.status) = 'SalesOrd:B'
```

## Currency functions

### BUILTIN.CONSOLIDATE

Consolidates an amount across subsidiaries. Its parameters, in order:

| Position | Parameter | Allowed values |
|---|---|---|
| 1 | Amount field | The amount to consolidate |
| 2 | View type | `LEDGER` or `INCOME` |
| 3 | Consolidation rate type | `DEFAULT`, `STANDARD` or `BUDGET` |
| 4 | Subsidiary rate type | `DEFAULT`, `CURRENT`, `HISTORICAL` or `AVERAGE` |
| 5 | Target subsidiary | The subsidiary to consolidate into |
| 6 | Period | The accounting period |
| 7 | Book | The accounting book |

To take the default for any of these parameters, pass the quoted string `'DEFAULT'`.

### BUILTIN.CURRENCY

Returns the currency code of an amount. The argument can be an amount field, an aggregate such as
`SUM(amount)`, or the result of `BUILTIN.CONSOLIDATE` or `BUILTIN.CURRENCY_CONVERT`.

### BUILTIN.CURRENCY_CONVERT

Converts an amount into another currency. Parameters, in order: the amount field, then the target
currency (optional), then the date whose exchange rate is applied (optional). Without a target, the
amount is converted to the subsidiary's currency; without a date, today's rate is used.

> [!SOURCE-ERROR] The source gives the accepted values of the target currency parameter as "number"
> and "date", which does not describe a currency and looks carried over from another parameter. The
> form of the target is therefore not settled; test whether your account expects a currency ID or a
> code before relying on either.
<!-- source-error: topic:suiteql.builtin_functions | - | currency_convert_target_values_listed_as_number_and_date -->

```sql
SELECT t.tranid,
       t.foreigntotal,
       BUILTIN.CURRENCY(t.foreigntotal) AS original_currency,
       BUILTIN.CURRENCY_CONVERT(t.foreigntotal) AS in_subsidiary_currency
FROM transaction t
WHERE t.type = 'VendBill'
```

## Hierarchies

**`BUILTIN.HIERARCHY`** returns the full hierarchical path of a value, either as identifiers or as
display values. It takes two required parameters, in order: the hierarchical field and the expansion
type. The expansion type is written as a single-quoted string and is one of:

| Expansion type | Expansion type |
|---|---|
| `DISPLAY` | `IDENTIFIER_SEPARATED` |
| `DISPLAY_JOINED` | `LEVEL` |
| `DISPLAY_SEPARATED` | `SELF_DISPLAY` |
| `IDENTIFIER` | `SELF_IDENTIFIER` |

The source does not describe what each expansion type returns beyond its name. As noted on the
[SuiteQL page](suiteql.md), hierarchy levels returned by built-in functions can be off by one.

## Multi-select filtering

**`BUILTIN.MNFILTER`** filters on multi-select fields. Its parameters, in order: the relationship
(multi-select) field, an operator, either `BUILTIN.DF` or an empty string, a case-sensitivity flag,
and the values to compare against. The operators are:

| Operator | Operator |
|---|---|
| `MN_INCLUDE` | `MN_EXCLUDE` |
| `MN_INCLUDE_ALL` | `MN_EXCLUDE_ALL` |
| `MN_INCLUDE_EXACTLY` | `MN_EXCLUDE_EXACTLY` |

The source does not describe the format of the value list.

> [!SOURCE-ERROR] The source calls the case-sensitivity flag a boolean, yet its own sample passes it
> as the quoted string `'TRUE'`. Which form the function accepts is not settled; try the call in your
> account before depending on either form.
<!-- source-error: topic:suiteql.builtin_functions | - | mnfilter_case_flag_boolean_but_example_passes_quoted_TRUE -->

> [!SOURCE-ERROR] The source's sample for this function also has a comma right before the `FROM`
> keyword, so it would not run as printed. Do not copy that punctuation; end the select list without
> a trailing comma.
<!-- source-error: topic:suiteql.builtin_functions | - | mnfilter_example_has_comma_before_from -->

## Named groups

**`BUILTIN.NAMED_GROUP`** stands for a predefined group of records relative to the current user. It
takes two required parameters: the record type and the workgroup. It works with departments,
classes, locations, subsidiaries and entities, and there are two workgroups:

- `me`, for entity-type records (entity, customer, vendor, partner): the current user;
- `mine`, for classes, departments, locations and subsidiaries: the ones belonging to the current
  user.

## Relative periods and dates

### BUILTIN.PERIOD

Selects accounting periods relative to today. It serves as the sub-selection of an `IN` predicate,
giving an expression shaped like
`<period column> IN BUILTIN.PERIOD(<range id>, <range type>, <adjustment>, <operator>)`. All four
parameters are required:

| Position | Parameter | Allowed values |
|---|---|---|
| 1 | Range ID | A relative range code, for example `LFY` (last fiscal year) |
| 2 | Range type | `START` or `END` |
| 3 | Adjustment | `NOT_LAST` or `ALL` |
| 4 | Operator | `BETWEEN`, `NOT BETWEEN`, `<`, `<=`, `>` or `>=` |

Write the range type, adjustment and operator values exactly as shown; `NOT BETWEEN` is two words
with a space between them. The valid range IDs are the codes in the date ID column of
[`query.RelativeDateRange`](../modules/N_query.md#queryrelativedaterange) in N/query (the short codes
such as `LFY` that each range carries as its `dateId`). The source does not say whether the
arguments are passed as quoted strings.

### BUILTIN.RELATIVE_RANGES

Returns the calendar start or end of a relative date range, for use in relative date filters.
Parameters, in order: the range ID (required), the range type `START` or `END` (required), and a date
flag (optional) from this list:

| Date flag | Date flag | Date flag |
|---|---|---|
| `DATE` | `DATE_TO_USER_TZ` | `ODBC_TIMESTAMP_TO_GMT_AS_DATE` |
| `DATETIME_AS_DATE` | `ODBC_DATE_TO_GMT` | `TIMESTAMP_WITH_TIMEZONE` |
| `DATE_FROM_GMT_TO_USER_TZ` | `ODBC_TIMESTAMP_TO_GMT` | `TIMESTAMP_WITH_TIMEZONE_AS_DATE` |

The source lists the date flags without describing what each one does.

```sql
SELECT COUNT(t.id) AS bill_count
FROM transaction t
WHERE t.type = 'VendBill'
  AND t.trandate BETWEEN BUILTIN.RELATIVE_RANGES('LFY', 'START')
                     AND BUILTIN.RELATIVE_RANGES('LFY', 'END')
```

## Example

A RESTlet that lists one vendor's bills with readable names, using `BUILTIN.DF` for display values
and `BUILTIN.CURRENCY` for the currency code of each total. Table and column names are illustrative.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/query'], (query) => {
  const SQL = `
    SELECT t.id,
           t.tranid,
           BUILTIN.DF(t.status) AS statuslabel,
           t.foreigntotal,
           BUILTIN.CURRENCY(t.foreigntotal) AS currencycode
    FROM transaction t
    WHERE t.type = 'VendBill'
      AND t.entity = ?
    ORDER BY t.id`;

  const get = (requestParams) => {
    const vendorId = Number(requestParams.vendor);
    if (!vendorId) {
      return { error: 'vendor parameter is required' };
    }
    const rows = query.runSuiteQL({ query: SQL, params: [vendorId] }).asMappedResults();
    return { count: rows.length, bills: rows };
  };

  return { get };
});
```
