# Custom Form Samples
<!-- id: topic:suitelet.samples.forms -->

Part of: [Suitelet Samples](suitelet.samples.md)

See also: [Suitelet](../script-types/Suitelet.md), [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
[N/email](../modules/N_email.md), [N/format](../modules/N_format.md), [N/crypto](../modules/N_crypto.md),
[N/encode](../modules/N_encode.md).

The custom form group shows Suitelets that draw NetSuite-style forms with
[N/ui/serverWidget](../modules/N_ui_serverWidget.md). The patterns it covers:

- a survey-style form mixing inline HTML with radio-button fields;
- a form with an inline editor sublist;
- a form for writing and sending an email ([`email.send`](../modules/N_email.md));
- creating a secret key and encoding a string with it;
- parsing a date typed in the user's date format and formatting it back
  ([`format.parse`](../modules/N_format.md), [`format.format`](../modules/N_format.md));
- a form with a credential field;
- adding a secret key field ([`Form.addSecretKeyField`](../modules/N_ui_serverWidget.md));
- a sublist field that keeps a running total;
- a form combining many field types with buttons, tabs and a sublist.

The source's date sample assumes the user's date preference is MM/DD/YYYY. Code that hard-codes a
date layout breaks for users with another preference; let `format.parse` read the value in the
user's own format instead, as the second example below does.

## Example: send a dispute note to a vendor

A purchasing clerk writes a short note about a disputed delivery. GET draws the form; POST sends the
email as the current user and shows a confirmation.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/email', 'N/runtime', 'N/http'], (serverWidget, email, runtime, http) => {
  const draftForm = () => {
    const form = serverWidget.createForm({ title: 'Delivery dispute note' });
    form.addField({ id: 'custpage_to', type: serverWidget.FieldType.EMAIL, label: 'Vendor contact' });
    form.addField({ id: 'custpage_subject', type: serverWidget.FieldType.TEXT, label: 'Subject' });
    form.addField({ id: 'custpage_note', type: serverWidget.FieldType.TEXTAREA, label: 'Note' });
    form.addSubmitButton({ label: 'Send note' });
    return form;
  };

  const onRequest = (context) => {
    const { request, response } = context;
    if (request.method !== http.Method.POST) {
      response.writePage({ pageObject: draftForm() });
      return;
    }

    const params = request.parameters;
    email.send({
      author: runtime.getCurrentUser().id,
      recipients: params.custpage_to,
      subject: params.custpage_subject || 'Delivery dispute',
      body: params.custpage_note || ''
    });

    const done = serverWidget.createForm({ title: 'Note sent' });
    const info = done.addField({ id: 'custpage_done', type: serverWidget.FieldType.INLINEHTML, label: 'Result' });
    info.defaultValue = '<p>Your note is on its way. A message record was created for it.</p>';
    response.writePage({ pageObject: done });
  };

  return { onRequest };
});
```

## Example: mileage claim with a running total

Field technicians enter trips in an inline editor sublist. The amount column is a running total, and
on submit the Suitelet reads every line, parses the trip date in the user's own date format and logs
a normalized summary.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/format', 'N/http', 'N/log'], (serverWidget, format, http, log) => {
  const TRIPS = 'custpage_trips';

  const claimForm = () => {
    const form = serverWidget.createForm({ title: 'Mileage claim' });
    const trips = form.addSublist({ id: TRIPS, type: serverWidget.SublistType.INLINEEDITOR, label: 'Trips' });
    trips.addField({ id: 'custpage_tripdate', type: serverWidget.FieldType.DATE, label: 'Date' });
    trips.addField({ id: 'custpage_dest', type: serverWidget.FieldType.TEXT, label: 'Destination' });
    trips.addField({ id: 'custpage_km', type: serverWidget.FieldType.FLOAT, label: 'Kilometres' });
    trips.addField({ id: 'custpage_amount', type: serverWidget.FieldType.CURRENCY, label: 'Amount' });
    trips.updateTotallingFieldId({ id: 'custpage_amount' });
    form.addSubmitButton({ label: 'Submit claim' });
    return form;
  };

  const onRequest = (context) => {
    const { request, response } = context;
    if (request.method !== http.Method.POST) {
      response.writePage({ pageObject: claimForm() });
      return;
    }

    const count = request.getLineCount({ group: TRIPS });
    let total = 0;
    for (let line = 0; line < count; line += 1) {
      const typed = request.getSublistValue({ group: TRIPS, name: 'custpage_tripdate', line });
      const tripDate = format.parse({ value: typed, type: format.Type.DATE });
      const amount = parseFloat(request.getSublistValue({ group: TRIPS, name: 'custpage_amount', line })) || 0;
      total += amount;
      log.audit({
        title: `Trip ${line + 1}`,
        details: `${format.format({ value: tripDate, type: format.Type.DATE })}: ${amount.toFixed(2)}`
      });
    }

    response.write({ output: `Claim received: ${count} trips, total ${total.toFixed(2)}` });
  };

  return { onRequest };
});
```

## Example: sign a payload with a secret key field

An administrator pastes a partner's shared secret into a secret key field once; the form stores only a
reference to it. The Suitelet then signs a test payload with an HMAC so the administrator can compare
the result with the partner's test value. The field is restricted to this script's ID, and nothing
secret ever appears in the code.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/crypto', 'N/encode', 'N/http'], (serverWidget, crypto, encode, http) => {
  const SCRIPT_ID = 'customscript_partner_payload_signer';

  const onRequest = (context) => {
    const { request, response } = context;
    const form = serverWidget.createForm({ title: 'Partner payload signer' });

    if (request.method !== http.Method.POST) {
      form.addSecretKeyField({
        id: 'custpage_partner_key',
        label: 'Partner shared secret',
        restrictToScriptIds: [SCRIPT_ID],
        restrictToCurrentUser: true
      });
      form.addField({ id: 'custpage_payload', type: serverWidget.FieldType.TEXTAREA, label: 'Test payload' });
      form.addSubmitButton({ label: 'Sign' });
      response.writePage({ pageObject: form });
      return;
    }

    const key = crypto.createSecretKey({
      guid: request.parameters.custpage_partner_key,
      encoding: encode.Encoding.UTF_8
    });
    const hmac = crypto.createHmac({ algorithm: crypto.HashAlg.SHA256, key });
    hmac.update({ input: request.parameters.custpage_payload || '', inputEncoding: encode.Encoding.UTF_8 });
    const signature = hmac.digest({ outputEncoding: encode.Encoding.HEX });

    const out = form.addField({ id: 'custpage_signature', type: serverWidget.FieldType.TEXT, label: 'HMAC-SHA256 (hex)' });
    out.defaultValue = signature;
    response.writePage({ pageObject: form });
  };

  return { onRequest };
});
```
