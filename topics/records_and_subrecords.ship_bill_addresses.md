# Ship-to and bill-to address subrecords
<!-- id: topic:records_and_subrecords.ship_bill_addresses -->

Part of: [Body field subrecords](records_and_subrecords.body_subrecords.md)

See also: [Subrecord scripting basics](records_and_subrecords.subrecord_basics.md),
[N/record](../modules/N_record.md), [salesorder](../gen/records/salesorder.md),
[address](../gen/records/address.md).

The shipping and billing addresses on a transaction are body subrecords, but they behave differently
from other body subrecords because they interact with the customer's address book.

## Where the address comes from

- By default a new transaction takes the entity's default address.
- To use one of the customer's existing addresses, set the address list select field (on a sales order,
  `shipaddresslist` for shipping) to that address's internal ID. This selects the address that fills the
  `shippingaddress` summary field. No subrecord methods are involved in this case.
- To use a different address, replace the default with a custom one. Either remove the existing
  subrecord with [`Record.removeSubrecord()`](../modules/N_record.md) and then set the new address, or
  set the address list field to null and then set the new address.

## Custom versus saved addresses

In the UI, the sales order's Ship To selector offers both "New" and "Custom":

- a **new** address is saved to the customer record as well as to the transaction;
- a **custom** address is stored only on the transaction.

An address created by a script is always treated as custom. A script cannot save the address it
creates back to the customer (entity).

## Rules when setting address fields

In dynamic mode, set the country first, before any other address field.

## Typical tasks

The spec illustrates these: logging the shipping address fields from a user event script, writing a
custom shipping address on a sales order, picking one of the customer's saved addresses as the shipping
address, and reading a single shipping address field.

## Example

Give an existing sales order a one-off delivery address in dynamic mode. The address list is cleared
first so the order no longer points at a saved address, and the country is set before the other
fields.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/log'], (record, log) => {
  const execute = () => {
    const order = record.load({ type: record.Type.SALES_ORDER, id: 8812, isDynamic: true });

    order.setValue({ fieldId: 'shipaddresslist', value: null });
    const shipTo = order.getSubrecord({ fieldId: 'shippingaddress' });
    shipTo.setValue({ fieldId: 'country', value: 'CA' });
    shipTo.setValue({ fieldId: 'attention', value: 'Receiving dock 4' });
    shipTo.setValue({ fieldId: 'addr1', value: '410 Harbourfront Way' });
    shipTo.setValue({ fieldId: 'city', value: 'Halifax' });
    shipTo.setValue({ fieldId: 'zip', value: 'B3J 1A1' });

    const id = order.save();
    log.audit({ title: 'Custom ship-to set', details: `Sales order ${id}` });
  };

  return { execute };
});
```
