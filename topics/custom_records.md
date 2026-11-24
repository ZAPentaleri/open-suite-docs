# Custom records
<!-- id: topic:custom_records -->

See also: [Custom fields](custom_fields.md), [Custom lists](custom_lists.md),
[Custom segments](custom_segments.md), [Customization records](records_guide.customization.md),
[SuiteCloud Development Framework](sdf.md), [N/record](../modules/N_record.md),
generated record page: [Custom Record](../gen/records/customrecord.md).

Custom record types let an account store data of its own design next to the standard records.

## Prerequisites

- The `Custom Records` feature must be enabled.
- Creating a custom record type needs the custom record types permission at Create level, or an
  administrator role.

## What they can do

- Point to entities, items and transactions through custom fields.
- Show up as a subtab on another record.
- Be defined and deployed with [SDF](sdf.md).

## Scripting

Do not try to reach into the custom record UI through the DOM; that is not supported. Use the
SuiteScript APIs, such as [N/record](../modules/N_record.md) and [N/search](../modules/N_search.md),
to work with custom record instances.

This topic does not state type ID formats or limits (for example how many record types or fields an
account may have). The generated [Custom Record](../gen/records/customrecord.md) page holds the
record-level facts the source gives: type IDs start with `customrecord`, field IDs with `custrecord`,
a script passes the type's string ID (custom types are not in the record type enum), and scripts
can create, update, copy, delete and search instances but cannot create or change the record types
themselves. Read the exact type ID from the record type in your account.

## Example

A scheduled script that writes one entry to an account-specific custom record type used as a
loading-dock audit log. The type ID `customrecord_dock_audit` and its field IDs are invented
placeholders that follow the prefixes above; use the IDs from your own account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/log'], (record, log) => {
  const execute = () => {
    const entry = record.create({ type: 'customrecord_dock_audit' });
    entry.setValue({ fieldId: 'custrecord_dock_audit_note', value: `Dock sweep ${new Date().toISOString()}` });
    entry.setValue({ fieldId: 'custrecord_dock_audit_door', value: 'D4' });
    const id = entry.save();
    log.audit({ title: 'Dock audit entry saved', details: id });
  };

  return { execute };
});
```
