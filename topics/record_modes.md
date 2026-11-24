# Record Modes
<!-- id: topic:record_modes -->

See also: [Scripting records and subrecords](records_and_subrecords.md), [N/record](../modules/N_record.md),
[N/currentRecord](../modules/N_currentRecord.md).

A [`record.Record`](../modules/N_record.md) object works in one of two modes, **standard** or
**dynamic**. The mode controls when NetSuite sources dependent values, runs its calculations and
validates input, and so whether the order in which your script sets fields makes a difference.

## Choosing the mode

Unless you ask for something else, [`record.create()`](../modules/N_record.md),
[`record.copy()`](../modules/N_record.md), [`record.load()`](../modules/N_record.md) and
[`record.transform()`](../modules/N_record.md) all hand back a record in standard mode. Dynamic mode is
switched on through the dynamic-mode argument of those same four calls: pass `isDynamic: true`.

To find out which mode an object is in at run time, read [`Record.isDynamic`](../modules/N_record.md)
or [`CurrentRecord.isDynamic`](../modules/N_currentRecord.md).

In user event scripts, the new and old record objects that the entry point receives are always in
standard mode, whatever you might expect from the context.

## Standard mode

Standard mode is also known as deferred dynamic mode.

- Sourcing, calculation and validation are postponed until the record is saved with
  [`Record.save()`](../modules/N_record.md).
- At save time NetSuite works out for itself in which order to process the body fields and the lines.
- Because of that, the order in which you set values is, for the most part, irrelevant.

## Dynamic mode

- Sourcing, calculation and validation run in real time, as soon as each value is set, so the record
  behaves the way the form does for a user typing into it.
- Order matters: set values in the sequence a user would enter them in the UI. On an invoice, for
  instance, the customer goes in before the terms.
- If you set values out of that UI sequence, the results can be inaccurate. Before you write a
  dynamic-mode script, look at the form in the UI to see which fields depend on which.

## getText() and the two modes

In standard mode, [`Record.getText()`](../modules/N_record.md) works only in these cases:

- the field was set with `setText()` earlier in the same script; or
- the record was loaded or copied and the field has not been changed with `setValue()`.

Outside those cases it throws `SSS_INVALID_API_USAGE`. Two typical ways to hit the error:

- on a record you just created, reading the text of a field you never set with `setText()`;
- on a record you loaded, reading the text of a field after changing it with `setValue()`.

In dynamic mode there are no such restrictions: `getText()` can be called on any field at any point.

## Example

Create an invoice in dynamic mode. The customer is set first, as a clerk would do in the form, so that
values sourced from it are in place before the terms and the line are added. Because the record is
dynamic, the text of the terms field can be read back straight after `setValue()`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/log'], (record, log) => {
  const execute = () => {
    const inv = record.create({ type: record.Type.INVOICE, isDynamic: true });
    log.debug({ title: 'Dynamic?', details: String(inv.isDynamic) });

    // Customer first, as a user would enter it; terms come after.
    inv.setValue({ fieldId: 'entity', value: 1042 });
    inv.setValue({ fieldId: 'terms', value: 4 });
    inv.setValue({ fieldId: 'memo', value: 'Quarterly maintenance retainer' });
    log.debug({ title: 'Terms chosen', details: inv.getText({ fieldId: 'terms' }) });

    inv.selectNewLine({ sublistId: 'item' });
    inv.setCurrentSublistValue({ sublistId: 'item', fieldId: 'item', value: 77 });
    inv.setCurrentSublistValue({ sublistId: 'item', fieldId: 'quantity', value: 1 });
    inv.commitLine({ sublistId: 'item' });

    log.audit({ title: 'Invoice created', details: String(inv.save()) });
  };

  return { execute };
});
```
