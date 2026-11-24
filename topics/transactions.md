# Transaction records
<!-- id: topic:transactions -->

See also: [Record actions and macros](record_actions_macros.md), [N/record](../modules/N_record.md),
[record reference index](../gen/records/index.md).

## Transaction lines and your service tier

Transaction lines count toward the monthly transaction lines metric, which is one of the limits of
your NetSuite service tier.

## Tax macros

Transaction records offer two macros, run with [`Record.executeMacro()`](../modules/N_record.md):

- `calculateTax` does the same thing as clicking the Preview Tax button. Taxes are still calculated
  again when the record is saved.
- `getSummaryTaxTotals` checks that tax has been calculated and collects the totals per tax type.
  Its result holds `notifications` and a `response` whose `taxTotals` array has one entry per tax type,
  each with `taxTypeId`, `taxTypeName` and `taxTotal`.

### When they work

Both macros need the SuiteTax feature to be enabled and a transaction type that can carry tax. The
taxable types are:

- [cash refund](../gen/records/cashrefund.md)
- [cash sale](../gen/records/cashsale.md)
- [credit card charge](../gen/records/creditcardcharge.md)
- [credit memo](../gen/records/creditmemo.md)
- [estimate](../gen/records/estimate.md)
- [expense report](../gen/records/expensereport.md)
- [invoice](../gen/records/invoice.md)
- [opportunity](../gen/records/opportunity.md)
- [purchase order](../gen/records/purchaseorder.md)
- [return authorization](../gen/records/returnauthorization.md)
- [sales order](../gen/records/salesorder.md)
- [vendor bill](../gen/records/vendorbill.md)
- [vendor credit](../gen/records/vendorcredit.md)
- [vendor return authorization](../gen/records/vendorreturnauthorization.md)

### Errors

- `SSS_UNSUPPORTED_METHOD`: SuiteTax is not enabled, or the transaction type is not taxable.
- `SSS_TAX_REGISTRATION_REQUIRED`: the transaction's subsidiary has no tax registration.

## Example

Before saving a sales order, preview its tax and log the total per tax type.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log'], (log) => {
  const beforeSubmit = (context) => {
    const order = context.newRecord;
    try {
      order.executeMacro({ id: 'calculateTax' });
      const summary = order.executeMacro({ id: 'getSummaryTaxTotals' });
      const totals = (summary && summary.response && summary.response.taxTotals) || [];
      totals.forEach((t) => {
        log.debug({ title: `Tax ${t.taxTypeName} (${t.taxTypeId})`, details: String(t.taxTotal) });
      });
    } catch (e) {
      // SSS_UNSUPPORTED_METHOD or SSS_TAX_REGISTRATION_REQUIRED
      log.error({ title: 'Tax preview skipped', details: e.name });
    }
  };

  return { beforeSubmit };
});
```
