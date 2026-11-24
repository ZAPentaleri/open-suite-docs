# N/transaction
<!-- id: N/transaction -->

`N/transaction` voids transactions. Voiding keeps the transaction in the system (the record is not
removed) but sets its total and line amounts to zero. Once a transaction is voided you can no longer
change its general ledger impact.

There are two kinds of void, and an account preference decides which one you get:

- **Direct void**: the transaction itself is zeroed out. This is what happens when the "void using
  reversing journals" accounting preference is off.
- **Void by reversing journal**: a new journal entry that reverses the transaction is created. This
  happens when that preference is on.

Transaction types that use a direct void need the reversing-journal preference turned off; that
preference lives on the Accounting Preferences page, under the accounting preferences area of
Setup. The
[`transaction.Type`](#transactiontype) table shows which kind of void each type supports.

- **Availability:** client and server scripts.
- **Since:** the source does not state a version for the module itself; its members date from
  2015.2.
- **Load it as:** `define(['N/transaction'], (transaction) => { ... })`
- Related: [N/record](N_record.md), [N/search](N_search.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`transaction.void(options)`](#transactionvoidoptions) | function | Void a transaction |
| [`transaction.void.promise(options)`](#transactionvoidpromiseoptions) | function | Promise form of `void` |
| [`transaction.Type`](#transactiontype) | enum | Transaction types and how they can be voided |

## Functions

### transaction.void(options)
<!-- id: N/transaction.void -->

Voids one transaction, for example a sales order that will never be fulfilled. Whether it is a
direct void or a reversing journal depends on the account's preferences (see above). Types that use
a direct void require the reversing-journal preference to be disabled. After the void, fields with
general ledger impact cannot be changed.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | transaction.Type | Yes | |
| `options.id` | number \| string | Yes | |

- `options.type`: a [`transaction.Type`](#transactiontype) value. Since 2015.2.
- `options.id`: internal ID of the transaction. Since 2015.2.

**Returns:** `number`. For a direct void, the ID of the voided transaction; for a void by reversing
journal, the ID of the newly created reversing journal entry.

**Errors:**

- `INVALID_RECORD_TYPE`: `options.type` is not a valid type, or that type cannot be voided.
- `THAT_RECORD_DOES_NOT_EXIST`: `options.id` does not identify an existing transaction.
- `SSS_MISSING_REQD_ARGUMENT`: `options.type` or `options.id` was left out.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### transaction.void.promise(options)
<!-- id: N/transaction.void.promise -->

Asynchronous form of [`transaction.void`](#transactionvoidoptions). It takes the same parameters and
raises the same errors, but returns a Promise. As with the synchronous call, the account's void
preference decides the kind of void, and GL-impacting changes are not possible afterwards.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | transaction.Type | Yes | |
| `options.id` | number \| string | Yes | |

**Returns:** `Promise`, settling with the voided transaction's ID (direct void) or the new reversing
journal's ID (void by reversing journal).

**Errors:** same as `transaction.void`.

**Governance:** 10 units.

**Availability:** client and server scripts (the spec does not restrict this variant to client
scripts).

**Since:** 2015.2

## Enums

### transaction.Type
<!-- id: N/transaction.Type -->

The transaction type values used by `options.type` of `transaction.void`, with the kind of void the
spec records for each. Types that use a direct void need the "void using reversing journals"
preference turned off (on the Accounting Preferences page, in Setup's accounting preferences).
Since 2015.2.

Rows marked "None" are part of the enum, but the source records no kind of void for them. The source
does not state whether `transaction.void` accepts these types; if a type cannot be voided, the error
`transaction.void` gives is `INVALID_RECORD_TYPE`.

| Value | Kind of void |
|---|---|
| `CASH_REFUND` | Direct void |
| `CASH_SALE` | Direct void |
| `CREDIT_MEMO` | Direct void |
| `CUSTOMER_DEPOSIT` | Direct void |
| `CUSTOMER_PAYMENT` | Direct void |
| `ESTIMATE` | Direct void |
| `EXPENSE_REPORT` | Direct void |
| `INVOICE` | Direct void |
| `JOURNAL_ENTRY` | Direct void |
| `PAYCHECK_JOURNAL` | Direct void |
| `RETURN_AUTHORIZATION` | Direct void |
| `SALES_ORDER` | Direct void |
| `TRANSFER_ORDER` | Direct void |
| `VENDOR_BILL` | Direct void |
| `VENDOR_CREDIT` | Direct void |
| `VENDOR_RETURN_AUTHORIZATION` | Direct void |
| `WAVE` | Direct void |
| `WORK_ORDER` | Direct void |
| `WORK_ORDER_CLOSE` | Direct void |
| `WORK_ORDER_COMPLETION` | Direct void |
| `WORK_ORDER_ISSUE` | Direct void |
| `CHECK` | Void by reversing journal |
| `CUSTOM_TRANSACTION` | Void by reversing journal |
| `CUSTOMER_REFUND` | Direct void or void by reversing journal |
| `VENDOR_PAYMENT` | Direct void or void by reversing journal |
| `VENDOR_PREPAYMENT` | Direct void or void by reversing journal |
| `VENDOR_PREPAYMENT_APPLICATION` | Direct void or void by reversing journal |
| `ASSEMBLY_BUILD` | None |
| `ASSEMBLY_UNBUILD` | None |
| `BIN_TRANSFER` | None |
| `BIN_WORKSHEET` | None |
| `BLANKET_PURCHASE_ORDER` | None |
| `CREDIT_CARD_CHARGE` | None |
| `CREDIT_CARD_REFUND` | None |
| `CUSTOMER_PAYMENT_AUTHORIZATION` | None |
| `DEPOSIT` | None |
| `DEPOSIT_APPLICATION` | None |
| `FULFILLMENT_REQUEST` | None |
| `INBOUND_SHIPMENT` | None |
| `INVENTORY_ADJUSTMENT` | None |
| `INVENTORY_COST_REVALUATION` | None |
| `INVENTORY_COUNT` | None |
| `INVENTORY_STATUS_CHANGE` | None |
| `INVENTORY_TRANSFER` | None |
| `ITEM_FULFILLMENT` | None |
| `ITEM_RECEIPT` | None |
| `OPPORTUNITY` | None |
| `ORDER_RESERVATION` | None |
| `PAYCHECK` | None |
| `PERIOD_END_JOURNAL` | None |
| `PURCHASE_CONTRACT` | None |
| `PURCHASE_ORDER` | None |
| `PURCHASE_REQUISITION` | None |
| `REVENUE_ARRANGEMENT` | None |
| `REVENUE_COMMITMENT` | None |
| `REVENUE_COMMITMENT_REVERSAL` | None |
| `STORE_PICKUP_FULFILLMENT` | None |

## Examples

A RESTlet that an order-management tool calls to cancel sales orders that will never ship. Each
order is voided on its own so one bad ID does not stop the rest, and the response reports what
happened to each.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/transaction', 'N/log'], (transaction, log) => {
  const post = (body) => {
    const outcomes = (body.orderIds || []).map((orderId) => {
      try {
        const resultId = transaction.void({ type: transaction.Type.SALES_ORDER, id: orderId });
        return { orderId, voided: true, resultId };
      } catch (e) {
        log.error({ title: `Void failed for order ${orderId}`, details: e.name });
        return { orderId, voided: false, reason: e.name };
      }
    });
    return { outcomes };
  };

  return { post };
});
```

A client script button handler that voids a customer refund with the promise form and tells the user
whether a reversing journal was created (the returned ID then differs from the refund's own ID):

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/transaction', 'N/currentRecord', 'N/ui/dialog'], (transaction, currentRecord, dialog) => {
  const pageInit = () => {};

  const voidRefund = () => {
    const refundId = currentRecord.get().id;
    transaction.void.promise({ type: transaction.Type.CUSTOMER_REFUND, id: refundId })
      .then((resultId) => {
        const how = String(resultId) === String(refundId)
          ? 'voided directly'
          : `reversed by journal ${resultId}`;
        return dialog.alert({ title: 'Refund voided', message: `Refund was ${how}.` });
      })
      .catch((e) => dialog.alert({ title: 'Void failed', message: e.message }));
  };

  return { pageInit, voidRefund };
});
```
