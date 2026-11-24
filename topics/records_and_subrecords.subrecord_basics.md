# Subrecord scripting basics
<!-- id: topic:records_and_subrecords.subrecord_basics -->

Part of: [Scripting records and subrecords](records_and_subrecords.md)

See also: [Body field subrecords](records_and_subrecords.body_subrecords.md),
[Sublist subrecords](records_and_subrecords.sublist_subrecords.md), [N/record](../modules/N_record.md),
[N/currentRecord](../modules/N_currentRecord.md), [SuiteScript Records Browser](records_browser.md).

## What a subrecord looks like

A subrecord is a record that lives inside a parent record and exists only together with it. On the
parent it is represented by a **summary field**, whose field type is "summary".

- In the UI the parent shows an icon for the subrecord; clicking it opens the subrecord in a new
  window. The inventory detail icon on a purchase order item line is a typical case.
- The summary field can be a body field or a sublist field:

  | Parent record | Summary field | Placement |
  |---|---|---|
  | Subsidiary | `mainaddress` | body |
  | Employee | `addressbook` sublist | sublist |
  | Purchase order | `inventorydetail` on the `item` sublist | sublist |
  | Assembly build | `inventorydetail` | body |

- The placement decides how the subrecord is created and reached (body methods or sublist methods).
- One subrecord type can be a body subrecord on one record type and a sublist subrecord on another.
- When the summary field is on a sublist line, the line's other columns are not part of the subrecord;
  they belong to the parent.
- Whether a summary field is present can depend on the parent's data. Inventory detail is only
  available for serialized or lot-numbered items (on an assembly build, the item being built must be
  one), and the landed cost subrecord on a vendor bill depends on the bill's landed-cost-per-line
  setting.

### How subrecords differ in shape

| Subrecord | Body fields | Sublist |
|---|---|---|
| [address](../gen/records/address.md) | writable | none |
| [landedcost](../gen/records/landedcost.md) | read-only | writable |
| [orderschedule](../gen/records/orderschedule.md) | writable | writable |

Subrecord sublists carry no label in the UI; look up their IDs in the
[Records Browser](records_browser.md).

## Getting a subrecord in a script

Three [`Record`](../modules/N_record.md) methods give access to a subrecord:

- [`Record.getSubrecord()`](../modules/N_record.md), for a summary field in the body;
- [`Record.getSublistSubrecord()`](../modules/N_record.md), for a summary field on a given sublist line;
- [`Record.getCurrentSublistSubrecord()`](../modules/N_record.md), for a summary field on the currently
  selected sublist line.

Each call needs the summary field ID, and the sublist methods also need the sublist ID. The result is
a record object or a current record object, and you set the subrecord's fields with the ordinary record
methods. Not every record method is supported on a subrecord, though.

A get call loads the subrecord when one exists and creates a new, empty one when it does not.

There is no separate save step: the subrecord is written when the parent is saved.

### When subrecords are read-only

- In a client script context, which includes a record obtained with
  [`currentRecord.get()`](../modules/N_currentRecord.md), subrecords can only be read.
- In a `beforeLoad` context they are read-only too, and subrecord methods there only work on a parent
  record that the script loads itself.

## Changes from SuiteScript 1.0

- The get methods now either load or create the subrecord; there are no separate create and edit
  calls.
- A subrecord is never saved explicitly.
- Addresses must be handled with subrecord methods. In 1.0, setting the legacy flat address fields on
  the parent created the address subrecord automatically; in 2.x a new address has to be created
  through its summary field. The [Records Browser](records_browser.md) still lists those legacy address
  fields, but they cannot be used that way in 2.x.

## Finding IDs

- **Subrecord body field IDs:** turn on the preference that shows internal IDs, then click the field's
  label in the UI.
- **Subrecord field and sublist IDs:** see the subrecord's own page in the
  [Records Browser](records_browser.md), where each subrecord appears with its label beneath its ID.
- **Where the summary field sits:** look at the parent record in the UI or in the Records Browser.

## Addresses are special

The address subrecord differs from other subrecords:

- one address can be shared across records;
- it has its own custom entry forms;
- the parent displays a summary text of it;
- client scripts can be attached to an address form.

Rules and quirks:

- In dynamic mode, set the country first. A change of country resets the address form, discarding
  values set before it.
- On a sales order, the choice in `shipaddresslist` determines what the `shippingaddress` summary field
  holds.
- The address fields `override`, `phone` and `zip` cannot be searched from the customer record.
- The `addrtext` field is generated from the address form's template and is what the parent shows in
  its address column. The template can be viewed among the address forms under Customization > Forms.
  If `addrtext` looks wrong, the template may be at fault rather than the address data.

### Client scripts on address forms

- Attach the script on the Custom Code subtab of a custom address form. Such a script can treat the
  address subrecord as if it were a record.
- A client script on an address form may run on the server as well as in the browser, so make it
  return early when it finds itself on the server. The source's sample tells the two apart by checking
  whether `typeof document` is not `'undefined'` (a browser has a `document` object).
- Loading [N/currentRecord](../modules/N_currentRecord.md) in an address-form client script makes the
  script fail. Use the `currentRecord` that the entry point receives in its context instead.

## Where subrecord scripts can run

Supported:

- a server script deployed on the parent record type;
- a server script that loads the parent record;
- a client script on a custom address form.

Not supported:

- deploying a server script, or a client script, directly to a subrecord type;
- creating a subrecord on the current record from a client script. A client script can still read
  existing subrecords and remove a subrecord.

Subrecord scripting is not supported together with the Advanced Employee Permissions feature.

## Examples

Report the main address city for a list of subsidiaries. Because `getSubrecord()` would create an
empty address when none exists, the script first checks with `hasSubrecord()`; nothing is saved, so no
empty subrecord is written.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/log'], (record, log) => {
  const SUBSIDIARY_IDS = [2, 3, 5];

  const execute = () => {
    SUBSIDIARY_IDS.forEach((id) => {
      const sub = record.load({ type: record.Type.SUBSIDIARY, id });
      if (!sub.hasSubrecord({ fieldId: 'mainaddress' })) {
        log.audit({ title: `Subsidiary ${id}`, details: 'No main address on file' });
        return;
      }
      const addr = sub.getSubrecord({ fieldId: 'mainaddress' });
      log.debug({
        title: `Subsidiary ${id}`,
        details: `${addr.getValue({ fieldId: 'city' })}, ${addr.getValue({ fieldId: 'country' })}`
      });
    });
  };

  return { execute };
});
```

A client script for a custom address form that only acts in the browser. It uses the record from the
entry point's context rather than loading N/currentRecord.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define([], () => {
  const fieldChanged = (context) => {
    if (typeof document === 'undefined') {
      return; // running on the server: do nothing
    }
    if (context.fieldId === 'city') {
      const addr = context.currentRecord;
      const city = String(addr.getValue({ fieldId: 'city' }) || '');
      if (city !== city.trim()) {
        addr.setValue({ fieldId: 'city', value: city.trim() });
      }
    }
  };

  return { fieldChanged };
});
```
