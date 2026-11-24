# N/currency
<!-- id: N/currency -->

`N/currency` answers one question: what is the exchange rate between two currencies on a given
day? The rate is read from the account's currency exchange rate records. The module needs the
`MULTICURRENCY` feature to be enabled. To display rates or amounts in a user's format, pair it with
[N/format](N_format.md).

- **Availability:** client and server scripts.
- **Requires feature:** `MULTICURRENCY`.
- **Load it as:** `define(['N/currency'], (currency) => { ... })`

## Members

| Member | Kind | Summary |
|---|---|---|
| [`currency.exchangeRate(options)`](#currencyexchangerateoptions) | function | Rate from a source currency to a target currency on a date |

## Functions

### currency.exchangeRate(options)
<!-- id: N/currency.exchangeRate -->

Returns the exchange rate of the source currency relative to the target currency on the given
date, taken from the currency exchange rate records. As an illustration: for a pound sterling
invoice in a subsidiary whose currency is the US dollar, a GBP source and a USD target might give
a rate such as 1.52.

The target must be the base currency. If you pass a currency other than the base currency as the
target, the rate that comes back may not be accurate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.date` | Date | No | |
| `options.source` | number \| string | Yes | |
| `options.target` | number \| string | Yes | |

- `options.date`: a `Date` object (for example one built for 28 July 2015). Left out, today's date
  is used. If several rate entries exist for that day, the latest one is used. The source describes
  the date as following the account's date format. Since 2015.2.
- `options.source`: the currency you are converting from, given either as its internal ID (such as
  `1`) or as its ISO code (such as `'USD'`). In accounts that use multiple currencies, the internal
  IDs are shown on the currencies list under the accounting lists. Since 2015.2.
- `options.target`: the currency you are converting to, as an internal ID or ISO code; it must be
  the base currency (see above). Since 2015.2.

**Returns:** `number`, the exchange rate from source to target on that date.

**Errors:**

- `MISSING_REQD_ARGUMENT`: `options.source` or `options.target` was not supplied.
- `SSS_INVALID_CURRENCY_ID`: `options.source` or `options.target` is not a valid currency.

**Governance:** 10 units.

**Availability:** client and server scripts; requires the `MULTICURRENCY` feature.

**Since:** 2015.2

## Examples

A user event script on vendor bills that records, in the memo, the rate between the bill's currency
and euros (the base currency in this scenario) on the bill date.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/currency'], (currency) => {
  const beforeSubmit = (context) => {
    const bill = context.newRecord;
    const billCurrency = bill.getValue({ fieldId: 'currency' });
    const billDate = bill.getValue({ fieldId: 'trandate' });

    const rate = currency.exchangeRate({
      source: billCurrency,
      target: 'EUR',
      date: billDate
    });
    bill.setValue({ fieldId: 'memo', value: `EUR rate on bill date: ${rate}` });
  };

  return { beforeSubmit };
});
```
