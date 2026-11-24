# N/email
<!-- id: N/email -->

`N/email` sends three kinds of email from a script: regular messages (`send`), bulk messages
(`sendBulk`) and lead-nurturing campaign emails (`sendCampaignEvent`). A copy of each message is
kept as a message record on every record type its recipients belong to, so it appears in the
communication history.

- **Availability:** client and server scripts. The `.promise` variants are for client scripts.
  File attachments can only be sent from server scripts.
- **Load it as:** `define(['N/email'], (email) => { ... })`
- **Sending as someone else:** to send with an `author` other than the current user, the role needs
  the vicarious email permission; without it the call fails with a permission violation. (The
  permission id is spelled differently for `send` and `sendBulk`; see those sections.)
- **Which function?** `send` goes through the regular mail path, receives bounce-back notices and
  achieves the higher delivery rate. `sendBulk` and `sendCampaignEvent` go out through the bulk
  email server; use them only when you do not need bounce-backs.
- Related: [N/render](N_render.md) (`render.mergeEmail` builds subject and body from a template),
  [N/file](N_file.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`email.send(options)`](#emailsendoptions) | function | Regular email, up to 10 recipients in total |
| [`email.send.promise(options)`](#emailsendpromiseoptions) | function | Promise version of `send` (client) |
| [`email.sendBulk(options)`](#emailsendbulkoptions) | function | Bulk email via the bulk server |
| [`email.sendBulk.promise(options)`](#emailsendbulkpromiseoptions) | function | Promise version of `sendBulk` (client) |
| [`email.sendCampaignEvent(options)`](#emailsendcampaigneventoptions) | function | Send one campaign drip email |
| [`email.sendCampaignEvent.promise(options)`](#emailsendcampaigneventpromiseoptions) | function | Promise version of `sendCampaignEvent` (client) |

## Functions

### email.send(options)
<!-- id: N/email.send -->

Sends an email to one or more recipients. Bounce-back notifications come back to the sender, and a
message record is created. Limits:

- at most 10 addresses across `recipients`, `cc` and `bcc` combined;
- each attachment at most 10 MB;
- the whole message, attachments included, at most 15 MB.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.author` | number | Yes | |
| `options.body` | string | Yes | |
| `options.recipients` | number \| string \| number[] \| string[] | Yes | |
| `options.cc` | number[] \| string[] | No | |
| `options.bcc` | number[] \| string[] | No | |
| `options.attachments` | file.File[] | No | |
| `options.isInternalOnly` | boolean | No | `false` |
| `options.relatedRecords` | Object | No | |
| `options.relatedRecords.activityId` | number | No | |
| `options.relatedRecords.entityId` | number | No | |
| `options.relatedRecords.transactionId` | number | No | |
| `options.relatedRecords.customRecord` | Object | No | |
| `options.relatedRecords.customRecord.id` | number | No | |
| `options.relatedRecords.customRecord.recordType` | string | No | |
| `options.replyTo` | string | No | |
| `options.subject` | string | Yes | |

- `options.author`: internal ID of an employee (you can look it up under Lists > Employees). It may
  be another user when sending on their behalf, which needs the `ADMI_VICARIOUS_EMAIL` permission.
- `options.body`: treated as HTML when it contains HTML tags, otherwise as plain text. To show XML
  as literal text, wrap it in a `<pre>` element.
- `options.recipients`: an internal ID, an email address, or an array mixing both. Only the first
  recipient is shown on the Communication subtab; viewing the message opens the message record with
  all recipients.
- `options.cc`, `options.bcc`: internal IDs, email addresses, or a mixed array.
- `options.attachments`: files of any media type, each at most 10 MB; only usable in server
  scripts.

> [!SOURCE-ERROR] The source's type cell for `attachments` was damaged and had to be repaired when
> the spec was built; `file.File[]` is the repaired value. Pass an array of `file.File` objects,
> and test if you depend on other forms.
<!-- source-error: N/email.send | options.attachments | source_type_cell_repaired -->

- `options.isInternalOnly`: when `true`, the resulting message record is hidden from external
  entities.
- `options.relatedRecords`: attaches the message record to other records. `customRecord`,
  `transactionId` and `activityId` are mutually exclusive; `entityId` can be combined with any of
  them. `activityId` is for a case or campaign; `entityId` for any entity type; `transactionId` for
  a transaction or opportunity. Within `customRecord`, `id` and `recordType` must be given together,
  and `recordType` is the integer ID of the custom record type (the number that identifies the type
  in a custom record entry's URL), passed as a string.
- `options.replyTo`: a single external email address, or an email capture plug-in address.
- `options.subject`: the subject line.

**Returns:** `void`

**Errors:**

- `SSS_AUTHOR_MUST_BE_EMPLOYEE`: `options.author` is not a valid employee.
- `SSS_INVALID_TO_EMAIL`: a recipient address is invalid.
- `SSS_INVALID_CC_EMAIL`: an address in `options.cc` is invalid.
- `SSS_INVALID_BCC_EMAIL`: an address in `options.bcc` is invalid.
- `SSS_MAXIMUM_NUMBER_RECIPIENTS_EXCEEDED`: `options.recipients`, `options.cc` and `options.bcc`
  together exceed 10.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.
- `WRONG_PARAMETER_TYPE`: an option has the wrong type.
- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: an attachment in `options.attachments` is larger than 10 MB.
- `ATTACH_SIZE_EXCEEDED`: the message as a whole, attachments included, is larger than 15 MB.
- A permission violation occurs when sending on behalf of another user without the vicarious email
  permission (no code is named).

**Governance:** 20 units.

**Availability:** client and server scripts (attachments: server only).

**Since:** 2015.2

> [!SOURCE-ERROR] The source spells the permission for sending on another user's behalf
> `ADMI_VICARIOUS_EMAIL` here but `ADMI_VICARIOUS_EMAILS` for `sendBulk`. It does not say which
> spelling is correct; check the permission id in your account before checking for it in code.
<!-- source-error: N/email.send | - | vicarious_permission_id_email_vs_emails -->

### email.send.promise(options)
<!-- id: N/email.send.promise -->

Asynchronous form of [`email.send`](#emailsendoptions) for client scripts. It returns a Promise and
takes the same parameters and raises the same errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.author` | number | Yes | |
| `options.body` | string | Yes | |
| `options.recipients` | number \| string \| number[] \| string[] | Yes | |
| `options.cc` | number[] \| string[] | No | |
| `options.bcc` | number[] \| string[] | No | |
| `options.attachments` | file.File[] | No | |
| `options.isInternalOnly` | boolean | No | `false` |
| `options.relatedRecords` | Object | No | |
| `options.relatedRecords.activityId` | number | No | |
| `options.relatedRecords.entityId` | number | No | |
| `options.relatedRecords.transactionId` | number | No | |
| `options.relatedRecords.customRecord` | Object | No | |
| `options.relatedRecords.customRecord.id` | number | No | |
| `options.relatedRecords.customRecord.recordType` | string | No | |
| `options.replyTo` | string | No | |
| `options.subject` | string | Yes | |

Parameter rules are as for `email.send` (attachments remain server-only, so in practice leave them
out here).

**Returns:** `Promise`

**Errors:** same as `email.send`.

**Governance:** 20 units.

**Availability:** client scripts.

**Since:** 2015.2

### email.sendBulk(options)
<!-- id: N/email.sendBulk -->

Sends email through the bulk email server. Use it when you do not need bounce-back handling;
prefer `send` when delivery rate matters. Limits: message at most 15 MB, each attachment at most
10 MB.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.author` | number | Yes | |
| `options.body` | string | Yes | |
| `options.recipients` | number \| string \| number[] \| string[] | Yes | |
| `options.attachments` | file.File[] | No | |
| `options.isInternalOnly` | boolean | No | `false` |
| `options.replyTo` | string | No | |
| `options.relatedRecords` | Object | No | |
| `options.relatedRecords.activityId` | number | No | |
| `options.relatedRecords.entityId` | number | No | |
| `options.relatedRecords.customRecord` | Object | No | |
| `options.relatedRecords.customRecord.id` | number | No | |
| `options.relatedRecords.customRecord.recordType` | string | No | |
| `options.relatedRecords.transactionId` | number | No | |
| `options.subject` | string | Yes | |
| `options.bcc` | number[] \| string[] | No | |
| `options.cc` | number[] \| string[] | No | |

- `options.author`: internal ID of the sending employee (see Lists > Employees). Sending as another
  user needs the `ADMI_VICARIOUS_EMAILS` permission.
- `options.body`: HTML if it contains HTML tags, plain text otherwise; wrap XML in `<pre>` to show
  it as text.
- `options.recipients`: an internal ID, an email address, or a mixed array. Only the first
  recipient is listed on the Communication subtab; viewing opens the message record with all of
  them.
- `options.attachments`: server scripts only.
- `options.isInternalOnly`: `true` hides the message from the external entity.
- `options.replyTo`: an external address or an email capture plug-in address.
- `options.relatedRecords`: at most one of each key. For `customRecord.recordType`, pass the custom
  record type's integer ID as a string.

> [!SOURCE-ERROR] The permission id given here, `ADMI_VICARIOUS_EMAILS`, ends in an S, while
> `email.send` names `ADMI_VICARIOUS_EMAIL`. The source does not settle which is right; verify the
> id in your account.
<!-- source-error: N/email.sendBulk | - | vicarious_permission_id_email_vs_emails -->

**Returns:** `void`

**Errors:**

- `SSS_AUTHOR_MUST_BE_EMPLOYEE`: `options.author` is not a valid employee.
- `SSS_INVALID_TO_EMAIL`: a recipient address is invalid.
- `SSS_INVALID_CC_EMAIL`: an address in `options.cc` is invalid.
- `SSS_INVALID_BCC_EMAIL`: an address in `options.bcc` is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.
- `WRONG_PARAMETER_TYPE`: an option has the wrong type.
- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: a size limit was exceeded.
- `ATTACH_SIZE_EXCEEDED`: a size limit was exceeded.

For `sendBulk` the source does not say which of the two size codes belongs to which limit (10 MB per
attachment, 15 MB per message); for `send` it ties the first to an oversized attachment and the
second to an oversized message.

**Governance:** 10 units.

**Availability:** client and server scripts (attachments: server only).

**Since:** 2015.2

### email.sendBulk.promise(options)
<!-- id: N/email.sendBulk.promise -->

Asynchronous form of [`email.sendBulk`](#emailsendbulkoptions) for client scripts. It returns a
Promise and takes the same parameters and raises the same errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.author` | number | Yes | |
| `options.body` | string | Yes | |
| `options.recipients` | number \| string \| number[] \| string[] | Yes | |
| `options.attachments` | file.File[] | No | |
| `options.isInternalOnly` | boolean | No | `false` |
| `options.replyTo` | string | No | |
| `options.relatedRecords` | Object | No | |
| `options.relatedRecords.activityId` | number | No | |
| `options.relatedRecords.entityId` | number | No | |
| `options.relatedRecords.customRecord` | Object | No | |
| `options.relatedRecords.customRecord.id` | number | No | |
| `options.relatedRecords.customRecord.recordType` | string | No | |
| `options.relatedRecords.transactionId` | number | No | |
| `options.subject` | string | Yes | |
| `options.bcc` | number[] \| string[] | No | |
| `options.cc` | number[] \| string[] | No | |

**Returns:** `Promise`

**Errors:** same as `email.sendBulk`.

**Governance:** 10 units.

**Availability:** client scripts.

**Since:** 2015.2

### email.sendCampaignEvent(options)
<!-- id: N/email.sendCampaignEvent -->

Sends a single lead-nurturing (drip) campaign email to one recipient through the bulk email
server. The campaign event must come from a campaign's drip sublist (`campaigndrip`); events on the
`campaignemail` sublist are not supported. Use `send` instead when you need a higher delivery rate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.campaignEventId` | number | Yes | |
| `options.recipientId` | number | Yes | |

- `options.campaignEventId`: an event from the campaign's `campaigndrip` sublist.
- `options.recipientId`: the recipient's record must have an email address.

**Returns:** `number`, the ID of the campaign response; `-1` when the email could not be sent.

**Errors:**

- `SSS_AUTHOR_MUST_BE_EMPLOYEE`: the author is not an employee.
- `SSS_INVALID_TO_EMAIL`: the recipient's email address is invalid.
- `SSS_INVALID_CC_EMAIL`, `SSS_INVALID_BCC_EMAIL`: listed by the source, although there are no cc or
  bcc options.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.
- `WRONG_PARAMETER_TYPE`: a parameter has the wrong type.
- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: an attachment is larger than 10 MB.
- `ATTACH_SIZE_EXCEEDED`: the message is larger than 15 MB.

> [!SOURCE-ERROR] The source lists cc and bcc address errors for this function even though it has
> no cc or bcc parameters. Those two errors are unlikely to be reachable; handle the others.
<!-- source-error: N/email.sendCampaignEvent | - | lists_cc_bcc_errors_without_cc_bcc_params -->

> [!SOURCE-ERROR] An "author must be an employee" error is listed, but the function takes no
> author option, and the source does not say where the sender comes from. Treat this error as
> possible but unexplained.
<!-- source-error: N/email.sendCampaignEvent | - | lists_author_error_without_author_param -->

> [!SOURCE-ERROR] Attachment size errors are listed although the function has no attachments
> option. Whether they can occur (for example from content the campaign itself carries) is not
> stated.
<!-- source-error: N/email.sendCampaignEvent | - | lists_attachment_errors_without_attachments_param -->

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### email.sendCampaignEvent.promise(options)
<!-- id: N/email.sendCampaignEvent.promise -->

Asynchronous form of [`email.sendCampaignEvent`](#emailsendcampaigneventoptions) for client
scripts. It returns a Promise, takes the same parameters and raises the same errors. As with the
synchronous call, a failed send yields `-1`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.campaignEventId` | number | Yes | |
| `options.recipientId` | number | Yes | |

**Returns:** `Promise` (resolving to the campaign response ID, or `-1` on failure).

**Errors:** same as `email.sendCampaignEvent`.

**Governance:** 10 units.

**Availability:** client scripts.

**Since:** 2015.2

## Examples

A user event script that tells the order's sales rep when a sales order is approved, files the
message on both the customer and the order, and marks it internal so the message record is hidden
from the customer. The sending employee comes from a script parameter.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/email', 'N/runtime'], (email, runtime) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.APPROVE) {
      return;
    }
    const order = context.newRecord;
    const customerId = Number(order.getValue({ fieldId: 'entity' }));
    const salesRepId = Number(order.getValue({ fieldId: 'salesrep' }));
    const sender = Number(runtime.getCurrentScript().getParameter({ name: 'custscript_order_desk_employee' }));

    email.send({
      author: sender,
      recipients: [salesRepId],
      subject: `Order ${order.getValue({ fieldId: 'tranid' })} approved`,
      body: '<p>This order has been approved and released to the warehouse.</p>',
      isInternalOnly: true,
      relatedRecords: { entityId: customerId, transactionId: order.id }
    });
  };

  return { afterSubmit };
});
```

Attaching a generated file only when it fits under the 10 MB attachment limit, and otherwise
sending a note that the file is in the File Cabinet:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/email', 'N/file'], (email, file) => {
  const ATTACHMENT_LIMIT_BYTES = 10 * 1024 * 1024;

  const sendReconciliation = (senderId, controllerEmail, fileId) => {
    const report = file.load({ id: fileId });
    const fits = report.size <= ATTACHMENT_LIMIT_BYTES;

    email.send({
      author: senderId,
      recipients: controllerEmail,
      subject: `Bank reconciliation: ${report.name}`,
      body: fits
        ? '<p>The reconciliation report is attached.</p>'
        : `<p>The report is too large to attach; it is stored at ${report.path}.</p>`,
      attachments: fits ? [report] : []
    });
  };

  return { sendReconciliation };
});
```

Pushing the next drip step to a list of leads and counting failures, which come back as `-1`:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/email'], (email) => {
  const sendDripStep = (dripEventId, leadIds) => {
    let failed = 0;
    leadIds.forEach((leadId) => {
      const responseId = email.sendCampaignEvent({ campaignEventId: dripEventId, recipientId: leadId });
      if (responseId === -1) {
        failed += 1;
      }
    });
    return { sent: leadIds.length - failed, failed };
  };

  return { sendDripStep };
});
```

A client script button handler that notifies a support queue without blocking the page:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/email', 'N/currentRecord'], (email, currentRecord) => {
  const pageInit = () => {};

  const flagForReview = (reviewerEmployeeId) => {
    const rec = currentRecord.get();
    email.send.promise({
      author: reviewerEmployeeId,
      recipients: ['support-queue@example.com'],
      subject: `Review requested for record ${rec.id}`,
      body: 'A user flagged this record for review from the form.'
    }).then(() => {
      window.alert('Support has been notified.');
    }).catch((e) => {
      window.alert(`Could not send: ${e.message}`);
    });
  };

  return { pageInit, flagForReview };
});
```
