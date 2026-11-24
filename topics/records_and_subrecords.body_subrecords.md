# Body field subrecords
<!-- id: topic:records_and_subrecords.body_subrecords -->

Part of: [Scripting records and subrecords](records_and_subrecords.md)

Sub-topics:

- [Ship-to and bill-to address subrecords](records_and_subrecords.ship_bill_addresses.md)

See also: [Subrecord scripting basics](records_and_subrecords.subrecord_basics.md),
[SuiteScript Records Browser](records_browser.md), [Record Modes](record_modes.md),
[N/record](../modules/N_record.md), [N/currentRecord](../modules/N_currentRecord.md).

A body field subrecord hangs off a field in the parent record's body rather than off a sublist line.
In the [Records Browser](records_browser.md), the field that holds such a subrecord is shown with the
type "summary"; that is the quickest way to recognise one.

## Where you meet them

- **Assembly build inventory detail**: the inventory detail held in the body of an assembly build
  (see [assemblybuild](../gen/records/assemblybuild.md) and
  [inventorydetail](../gen/records/inventorydetail.md)).
- **Subsidiary addresses**: for example the subsidiary's `mainaddress` summary field
  ([subsidiary](../gen/records/subsidiary.md)).
- **Company information addresses**: the main, shipping and return addresses (`mainaddress`,
  `shippingaddress`, `returnaddress`) of the page reached under Setup > Company > Company Information.
  Company Information is a page, not a true record, yet scripts work with it as though it were one.
  On that page the state and country are plain body fields of the page itself, kept apart from the
  address subrecords.
- **Transaction ship-to and bill-to addresses** are body subrecords too, but they follow different
  rules from the others; see [Ship-to and bill-to address subrecords](records_and_subrecords.ship_bill_addresses.md).

You script body subrecords with [N/record](../modules/N_record.md) or
[N/currentRecord](../modules/N_currentRecord.md), and both standard and dynamic mode work (see
[Record Modes](record_modes.md)).

## Creating a body subrecord

1. Create or load the parent record.
2. Call [`Record.getSubrecord()`](../modules/N_record.md) with the ID of the summary field.
3. Fill the subrecord's body fields with `setValue()`.
4. If the subrecord has a sublist of its own, add lines to it. A subrecord that has a sublist must end
   up with at least one line.
5. Save the parent; the subrecord is written as part of that save.

Keep in mind that some subrecords have no body fields a script can write.

The source walks through creation with two cases:

- a subsidiary together with its main address, which needs a OneWorld account and a subsidiary name
  not already in use;
- an assembly build with inventory detail, which needs a lot-numbered assembly item and a receipt
  inventory number (`receiptinventorynumber`) that has not been used before.

Inventory detail work needs the feature "Advanced Bin / Numbered Inventory Management" to be enabled
(that is how the source names it here).

## Editing a body subrecord

1. Load the parent.
2. Call `getSubrecord()`.
3. Change body fields with `setValue()` and adjust the subrecord's lines as needed.
4. Save the parent.

An edit succeeds only where the parent record's own business logic permits that change, and, as with
creation, not every subrecord exposes writable body fields. The source's editing cases change a
subsidiary's main address (a OneWorld account is required) and the inventory detail of an assembly
build that already carries inventory detail (the replacement receipt inventory number must again be
unique).

## Changing lines inside the subrecord

- **Dynamic mode:** select the line, set values with `setCurrentSublistValue()`, then commit the line.
- **Standard mode:** call `setSublistValue()` with the sublist ID, the field ID, the line number and the
  value.

The inventory detail subrecord of an assembly build is the source's example of a body subrecord with
lines.

## Reading a body subrecord

Load the parent, call `getSubrecord()`, then read the subrecord's body fields with `getValue()` or its
line fields with `getSublistValue()`. The source's reading cases are the fields of a location's main
address subrecord and a line value from an assembly build's inventory detail subrecord.

## Example

Replace the first street line of a subsidiary's main address in standard mode. Only the subrecord
changes; saving the subsidiary writes it.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/runtime', 'N/log'], (record, runtime, log) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const subsidiaryId = script.getParameter({ name: 'custscript_target_subsidiary' });
    const street = script.getParameter({ name: 'custscript_new_street' });

    const sub = record.load({ type: record.Type.SUBSIDIARY, id: subsidiaryId });
    const address = sub.getSubrecord({ fieldId: 'mainaddress' });
    address.setValue({ fieldId: 'addr1', value: street });
    const savedId = sub.save();

    log.audit({ title: 'Main address updated', details: `Subsidiary ${savedId}` });
  };

  return { execute };
});
```
