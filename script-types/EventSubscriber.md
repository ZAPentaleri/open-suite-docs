# EventSubscriber
<!-- id: script:EventSubscriber -->

An event subscriber script is server-side code that reacts after a record of a chosen type has been
created, updated or deleted. It runs asynchronously, once the record operation has finished, so it
suits follow-up work that need not be done before the operation completes: sending a notification
after an invoice is created, updating related data after a change, or starting external or
long-running processing.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `EventSubscriber` |
| API version | SuiteScript 2.1 required |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server, asynchronously |
| Events | Record created, record updated, record deleted (for the record type you subscribe to) |
| Deployed with | SDF, in a SuiteCloud project |
| Entry points | [`handle`](#handlecontext-payload) |

Related pages: [SDF object](../topics/sdf.eventsubscriber.md),
[SDF object elements](../gen/catalogs/sdf_eventsubscriber_object.md), [SDF](../topics/sdf.md),
[UserEventScript](UserEventScript.md), [N/record](../modules/N_record.md).

## How it runs

**What triggers it.** The event subscriber definition (an SDF object) holds one or more subscription
criteria. When a supported event (create, update or delete) happens on a record of the selected type
that meets those criteria, NetSuite runs the script's `handle` entry point after the operation.

**How it differs from a user event script.** A [user event script](UserEventScript.md) runs while the
record operation is in progress. An event subscriber runs after the operation has completed and does
not hold it up. Choose it when the follow-up work need not finish before the record operation does;
choose a user event script when you must validate or change the record as it is saved.

**What you need.** Two pieces:

- The script file: written in SuiteScript 2.1, annotated `@NScriptType EventSubscriber`, and
  implementing the `handle` entry point.
- The event subscriber SDF object definition: it references the script file and lists at least one
  subscription criterion.

Both are deployed with the SuiteCloud Development Framework as part of a SuiteCloud project.

## Entry points

### handle(context, payload)
<!-- id: script:EventSubscriber.handle -->

Processes one event the script is subscribed to. NetSuite calls it and passes both arguments
automatically: the event context and the event payload (two arguments, not one context object).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | - | |
| `context.recordType` | string | Yes | |
| `payload` | Object | - | |
| `payload.recordId` | string | Yes | |

Both arguments exist since 2026.2. The spec gives no required flag for the two arguments themselves,
but says both are always provided.

- `context`: the event context. Its `recordType` member holds the record type ID of the record that
  raised the event.
- `payload`: the event payload. Its `recordId` member holds the internal ID of that record, as a
  string.
- The spec lists no other members of either argument.

**Returns:** `Promise<void>`: a promise that resolves with no value, so `handle` is usually written as
an `async` function.

**Errors:** the source does not state any error codes for this entry point.

## Examples

Notify the assigned sales rep when an invoice is created. The subscription criteria (in the SDF
object, not shown) limit the script to invoice creation; the script still checks the record type.
The sender's employee ID comes from a script parameter whose ID is invented:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType EventSubscriber
 */
define(['N/record', 'N/email', 'N/runtime', 'N/log'], (record, email, runtime, log) => {
  const handle = async (context, payload) => {
    if (context.recordType !== 'invoice') {
      return;
    }
    const invoice = record.load({ type: 'invoice', id: payload.recordId });
    const repId = invoice.getValue({ fieldId: 'salesrep' });
    if (!repId) {
      log.debug({ title: 'No sales rep', details: `Invoice ${payload.recordId}` });
      return;
    }
    const sender = runtime.getCurrentScript().getParameter({ name: 'custscript_billing_sender' });
    email.send({
      author: Number(sender),
      recipients: Number(repId),
      subject: `Invoice ${invoice.getValue({ fieldId: 'tranid' })} issued`,
      body: `Total ${invoice.getValue({ fieldId: 'total' })}, due ${invoice.getValue({ fieldId: 'duedate' })}.`
    });
  };

  return { handle };
});
```

Keep a count on a related customer up to date when support cases change. The custom field ID is
invented. The source does not say whether a deleted record can still be read when `handle` runs, so
the script guards the lookup and logs when it fails:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType EventSubscriber
 */
define(['N/search', 'N/record', 'N/log'], (search, record, log) => {
  const countCases = (customerId) => {
    let total = 0;
    search.create({
      type: 'supportcase',
      filters: [['company', 'anyof', customerId]],
      columns: ['internalid']
    }).run().each(() => {
      total += 1;
      return true;
    });
    return total;
  };

  const handle = async (context, payload) => {
    let customerId;
    try {
      const fields = search.lookupFields({ type: 'supportcase', id: payload.recordId, columns: ['company'] });
      customerId = fields.company && fields.company[0] && fields.company[0].value;
    } catch (e) {
      log.audit({ title: `${context.recordType} ${payload.recordId} not readable`, details: e.message });
      return;
    }
    if (!customerId) {
      return;
    }
    record.submitFields({
      type: 'customer',
      id: customerId,
      values: { custentity_case_count: countCases(customerId) }
    });
  };

  return { handle };
});
```
