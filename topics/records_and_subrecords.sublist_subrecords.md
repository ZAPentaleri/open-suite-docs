# Sublist subrecords
<!-- id: topic:records_and_subrecords.sublist_subrecords -->

Part of: [Scripting records and subrecords](records_and_subrecords.md)

See also: [Subrecord scripting basics](records_and_subrecords.subrecord_basics.md),
[Record Modes](record_modes.md), [SuiteScript Records Browser](records_browser.md),
[N/record](../modules/N_record.md).

A sublist subrecord belongs to a single line of a parent record's sublist: every line carries its own
subrecord in a field of that line. In the [Records Browser](records_browser.md) that holding field is
listed with the type "summary". All of the work below uses [N/record](../modules/N_record.md), and
either standard or dynamic mode can be used.

Typical cases:

- inventory detail on purchase order item lines
  ([purchaseorder](../gen/records/purchaseorder.md), [inventorydetail](../gen/records/inventorydetail.md));
- landed cost on vendor bill lines ([vendorbill](../gen/records/vendorbill.md),
  [landedcost](../gen/records/landedcost.md));
- addresses in an employee's (or other entity's) address book, held in the `addressbookaddress` field
  of the `addressbook` sublist ([employee](../gen/records/employee.md));
- the order schedule on a blanket purchase order line
  ([orderschedule](../gen/records/orderschedule.md)).

## Which method reaches the line's subrecord

| Mode | Method | What it needs |
|---|---|---|
| Standard | [`Record.getSublistSubrecord()`](../modules/N_record.md) | the sublist ID, the field ID and the line number |
| Dynamic | [`Record.getCurrentSublistSubrecord()`](../modules/N_record.md) | the line selected first; then the sublist ID and field ID |

## Creating a sublist subrecord

**Dynamic mode**

1. Create or load the parent.
2. Select a new line (`selectNewLine()`) or an existing one (`selectLine()`).
3. Call `getCurrentSublistSubrecord()`.
4. Set the subrecord's body fields.
5. Add the subrecord's own sublist lines, selecting, setting and committing each one.
6. Commit the parent line that holds the subrecord (`commitLine()`).
7. Save the parent.

**Standard mode**

1. Create or load the parent.
2. Insert the parent line.
3. Call `getSublistSubrecord()` with that line number.
4. Set the subrecord's fields with `setValue()`, and insert its own sublist lines, filling them with
   `setSublistValue()`.
5. Save the parent.

A subrecord that has a sublist needs at least one line in it. Not every subrecord has body fields you
can edit.

## Editing a sublist subrecord

**Dynamic mode:** load the parent, select the line, call `getCurrentSublistSubrecord()`, set body
fields, select / set / commit the subrecord's lines, commit the parent line, then save.

**Standard mode:** load the parent, call `getSublistSubrecord()` with the line number, set body fields
with `setValue()` and line fields with `setSublistValue()`, then save.

Again, not every subrecord has writable body fields.

## Reading a sublist subrecord

- **Dynamic mode:** load, select the line, call `getCurrentSublistSubrecord()`, then read with
  `getValue()` or `getSublistValue()`.
- **Standard mode:** load, call `getSublistSubrecord()`, then read with `getValue()` or
  `getSublistValue()`.

## Inventory detail

Needs the feature "Advanced Bin / Numbered Inventory Management" (the name exactly as the source gives
it here). The inventory assignment lines use one of two fields, never both:

| Field | Purpose | Used on |
|---|---|---|
| `issueinventorynumber` | pick numbers that already exist | sales orders, item fulfillments, status changes, negative adjustments |
| `receiptinventorynumber` | enter new numbers | item receipts, positive adjustments |

The source's purchase order case needs a vendor, a location, a lot-numbered item and a receipt
inventory number that is not already in use.

## Landed cost

Needs the "Landed Cost" feature, and the vendor bill must have landed cost per line turned on.

## Order schedule on blanket purchase orders

Needs the "Blanket Purchase Order" feature, which is switched on from the Transactions subtab of the
Enable Features page.

> [!SOURCE-ERROR] This page names the feature in the singular, while the
> [feature list](../gen/catalogs/features.md) calls it "Blanket Purchase Orders" (id
> `BLANKETPURCHASEORDERS`). They appear to be the same feature; look for either spelling on the Enable
> Features page.
<!-- source-error: topic:records_and_subrecords.sublist_subrecords | - | blanket_purchase_order_feature_name_singular_vs_plural -->

The source's cases update an existing order schedule in dynamic mode (the blanket purchase order line
must already have one), add an order schedule to a blanket purchase order line, and read order
schedule values.

## Other cases in the source

Adding an employee address line in dynamic mode, updating an employee address in standard mode, and
reading a line value together with a field of the line's address.

## Example

Read the city of every address in a vendor's address book in standard mode, by asking each line for
its address subrecord.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/runtime', 'N/log'], (record, runtime, log) => {
  const execute = () => {
    const vendorId = runtime.getCurrentScript().getParameter({ name: 'custscript_vendor_to_audit' });
    const vend = record.load({ type: record.Type.VENDOR, id: vendorId });
    const lines = vend.getLineCount({ sublistId: 'addressbook' });

    for (let line = 0; line < lines; line += 1) {
      const label = vend.getSublistValue({ sublistId: 'addressbook', fieldId: 'label', line });
      const addr = vend.getSublistSubrecord({ sublistId: 'addressbook', fieldId: 'addressbookaddress', line });
      log.debug({ title: `Address "${label}"`, details: addr.getValue({ fieldId: 'city' }) });
    }
  };

  return { execute };
});
```
