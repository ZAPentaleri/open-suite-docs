# Item pricing sublist
<!-- id: topic:records_guide.pricing_sublist -->

Part of: [Item records](records_guide.items.md)

Item prices live in a pricing sublist whose shape depends on three account features. Most of the
work in a pricing script is picking the right sublist ID and the right family of methods.

## Which items have it

The pricing sublist appears on these record types:
[assemblyitem](../gen/records/assemblyitem.md),
[lotnumberedassemblyitem](../gen/records/lotnumberedassemblyitem.md),
[serializedassemblyitem](../gen/records/serializedassemblyitem.md),
[inventoryitem](../gen/records/inventoryitem.md),
[lotnumberedinventoryitem](../gen/records/lotnumberedinventoryitem.md),
[serializedinventoryitem](../gen/records/serializedinventoryitem.md),
[giftcertificateitem](../gen/records/giftcertificateitem.md) and
[kititem](../gen/records/kititem.md).

> [!SOURCE-ERROR] The source's screenshots of the pricing screens predate Version 2010 Release 2, so
> the UI in your account may not look like them. Go by the sublist and field IDs, not the pictures.
<!-- source-error: topic:records_guide.pricing_sublist | - | screenshots_predate_version_2010_release_2 -->

## Features that shape the sublist

| Feature | Where it is enabled | Effect |
|---|---|---|
| Multiple Currencies | Enable Features, Company subtab | One pricing subtab (and sublist) per currency |
| Multiple Prices | Enable Features, Transactions subtab | Several price levels; with it off, an item has only one price |
| Quantity Pricing | Enable Features, Transactions subtab | Quantity-break columns in the price matrix |

Enabling these features takes the Administrator role. The number of quantity columns in the matrix
comes from the preference for the maximum number of quantity-based price levels.

With none of the three features enabled there is no pricing sublist. The item then has a single
sales price on its Basic subtab, held in the body field `rate`. Treat `rate` as a body field; do not
use sublist methods on it.

A script can find out which features are on with
[`runtime.isFeatureInEffect`](../modules/N_runtime.md). The [features catalog](../gen/catalogs/features.md)
lists the IDs `MULTICURRENCY` (Multiple Currencies), `MULTPRICE` (Multiple Prices) and
`QUANTITYPRICING` (Quantity Pricing).

> [!SOURCE-ERROR] The pricing material gives the Multiple Prices feature ID as `MULTIPRICE` [sic],
> while the features catalog lists `MULTPRICE`. The source does not say which one
> `isFeatureInEffect` accepts; test both in your account before relying on either.
<!-- source-error: topic:records_guide.pricing_sublist | - | feature_id_MULTIPRICE_vs_catalog_MULTPRICE -->

## Sublist IDs

- Single currency: the sublist ID is `price`.
- Multiple currencies: one sublist per currency, named `price` followed by the currency's internal
  ID. If US dollars are currency 1 and Canadian dollars currency 3, the sublists are `price1` and
  `price3`. Currency internal IDs are in the ID column of the currencies list.

## Fields

Each price cell belongs to a matrix field whose ID is `price`. Even where the UI labels a price
column as an alternate price or an online price, the field behind it is still `price`.

The other sublist fields are `discount`, `currency` and `pricelevel`. The `currency` field is hidden
in the UI, and scripts can use it only when Multiple Currencies is on. Per currency it appears under
IDs such as `price1currency` and `price2currency`.

> [!SOURCE-ERROR] The source is not consistent about the per-currency field ID: it shows the pattern
> `price1currency` in one place and `pricecurrency3` in another, and its example uses plain
> `currency`. The source does not say which form is correct; read the field IDs from a loaded item
> in your account.
<!-- source-error: topic:records_guide.pricing_sublist | - | currency_field_id_pattern_inconsistent -->

Related body fields on the item:

| Field | Meaning |
|---|---|
| `quantitypricingschedule` | Choosing a quantity pricing schedule sets `overallquantitypricingtype` and drives the prices in the matrix |
| `overallquantitypricingtype` | Which quantity is used as the basis when an order is priced |
| `usemarginalrates` | How quantity discounts are applied on an order |
| `pricinggroup` | Supports customer-specific pricing |

`overallquantitypricingtype` and `usemarginalrates` do not change any matrix price settings, and
fields of the discount type leave the matrix prices unchanged as well.

## Matrix methods versus standard methods

Use the matrix methods for the price values and the ordinary sublist and body-field methods for
every other field. Which matrix method fits:

- **Header cells** (the quantity breaks along the top of the matrix): the header methods
  `getMatrixHeaderCount`, `getMatrixHeaderField`, `getMatrixHeaderValue`, `setMatrixHeaderValue`.
- **The line that is already selected**: `getCurrentMatrixSublistValue`,
  `setCurrentMatrixSublistValue`.
- **Any other line**: `getMatrixSublistValue`, `setMatrixSublistValue`, `getMatrixSublistField`, or
  `findMatrixSublistLineWithValue` to locate the line first.

| Method | On `record.Record` ([N/record](../modules/N_record.md)) | On `currentRecord.CurrentRecord` ([N/currentRecord](../modules/N_currentRecord.md)) |
|---|---|---|
| findMatrixSublistLineWithValue | `Record.findMatrixSublistLineWithValue` | `CurrentRecord.findMatrixSublistLineWithValue` |
| getCurrentMatrixSublistValue | `Record.getCurrentMatrixSublistValue` | `CurrentRecord.getCurrentMatrixSublistValue` |
| getMatrixHeaderCount | `Record.getMatrixHeaderCount` | `CurrentRecord.getMatrixHeaderCount` |
| getMatrixHeaderField | `Record.getMatrixHeaderField` | `CurrentRecord.getMatrixHeaderField` |
| getMatrixHeaderValue | `Record.getMatrixHeaderValue` | `CurrentRecord.getMatrixHeaderValue` |
| getMatrixSublistField | `Record.getMatrixSublistField` | `CurrentRecord.getMatrixSublistField` |
| getMatrixSublistValue | `Record.getMatrixSublistValue` | `CurrentRecord.getMatrixSublistValue` |
| setCurrentMatrixSublistValue | `Record.setCurrentMatrixSublistValue` | `CurrentRecord.setCurrentMatrixSublistValue` |
| setMatrixHeaderValue | `Record.setMatrixHeaderValue` | `CurrentRecord.setMatrixHeaderValue` |
| setMatrixSublistValue | `Record.setMatrixSublistValue` | `CurrentRecord.setMatrixSublistValue` |

> [!SOURCE-ERROR] The source's pricing code sample contains several mistakes: a price ID written
> with inconsistent letter case, a branch dealing with `rate` that conflicts with the rule for
> accounts without pricing features, a search object passed where a currency ID is expected, and a
> SuiteScript 1.0 matrix call inside a 2.x script. Do not copy that sample; follow the rules on this
> page and the method signatures in N/record.
<!-- source-error: topic:records_guide.pricing_sublist | - | pricing_code_sample_has_several_errors -->

## Example

A scheduled script that raises the first quantity tier of every price level on one inventory item by
4 percent, picking the sublist ID from the Multiple Currencies feature. The item ID is illustrative;
with several currencies this only touches the currency-1 sublist.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/runtime', 'N/log'], (record, runtime, log) => {
  const ITEM_ID = 7716;

  const execute = () => {
    const multiCurrency = runtime.isFeatureInEffect({ feature: 'MULTICURRENCY' });
    const sublistId = multiCurrency ? 'price1' : 'price';

    const item = record.load({ type: record.Type.INVENTORY_ITEM, id: ITEM_ID });
    const lineCount = item.getLineCount({ sublistId });

    for (let line = 0; line < lineCount; line += 1) {
      const current = item.getMatrixSublistValue({ sublistId, fieldId: 'price', column: 0, line });
      if (typeof current !== 'number') {
        continue;
      }
      const raised = Math.round(current * 104) / 100;
      item.setMatrixSublistValue({ sublistId, fieldId: 'price', column: 0, line, value: raised });
      log.debug({
        title: item.getSublistText({ sublistId, fieldId: 'pricelevel', line }),
        details: `${current} -> ${raised}`
      });
    }

    item.save();
  };

  return { execute };
});
```
