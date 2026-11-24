# Custom transactions
<!-- id: topic:custom_transactions -->

See also: [Custom segments](custom_segments.md), [Customization records](records_guide.customization.md),
[CSV import](csv_import.md), [SuiteCloud Development Framework](sdf.md),
generated record page: [Custom Transaction](../gen/records/customtransaction.md).

Custom transaction types let an account define transaction kinds of its own, beside the standard
ones.

## Prerequisites

- The `Custom Transactions` feature must be enabled.
- Defining custom transaction types needs the custom transaction types permission, or an
  administrator role.

## Place in the product

- Custom transactions belong to SuiteGL, together with custom GL lines and custom segments.
- Custom transactions count toward the account's monthly transaction lines metric.
- Custom transaction types can be defined and deployed with [SDF](sdf.md).

## Working with instances

Transactions of a custom type can be created and handled through [CSV import](csv_import.md),
SuiteScript and REST web services. REST web services handle the basic, journal and header-only
styles of custom transaction.

This topic does not state the type IDs a script uses. The generated
[Custom Transaction](../gen/records/customtransaction.md) page gives the record-level facts: the ID
is `customtransaction` followed by the type's own ID, and searches go through the transaction
record. Read the exact type ID from your account.

## Example

A scheduled script that loads one instance of a custom transaction type (its internal ID comes from a
script parameter) and logs its entry number (`tranid`). The type ID `customtransaction_fuel_levy` is
an invented placeholder in the format above; use the ID from your account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/runtime', 'N/log'], (record, runtime, log) => {
  const execute = () => {
    const levyId = runtime.getCurrentScript().getParameter({ name: 'custscript_levy_id' });
    if (!levyId) {
      return;
    }
    const levy = record.load({ type: 'customtransaction_fuel_levy', id: levyId });
    log.audit({ title: 'Fuel levy loaded', details: levy.getValue({ fieldId: 'tranid' }) });
  };

  return { execute };
});
```
