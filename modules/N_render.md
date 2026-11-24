# N/render
<!-- id: N/render -->

`N/render` turns NetSuite data into printable output and template-driven email. It covers three
jobs:

- **Ready-made printouts** for specific documents: a transaction, a customer statement, a picking
  ticket, a packing slip, and a bill of materials. Each returns a `file.File` holding a PDF or HTML
  document.
- **Your own templates** through a [`TemplateRenderer`](#templaterenderer-object): bind records,
  search results, query results or custom JSON/XML to a FreeMarker template (choosing the template
  by id, by script id or by assigning its source), then render to a string, a PDF file, or straight
  into a Suitelet response.
- **Mail merge** with scriptable email templates through [`render.mergeEmail`](#rendermergeemailoptions).

[`render.xmlToPdf`](#renderxmltopdfoptions) converts XML markup into a PDF using the BFO tag
library.

- **Availability:** server scripts only.
- **Permissions:** the role running the script needs the Advanced PDF/HTML Templates and Custom
  PDF Layouts permissions.
- **Load it as:** `define(['N/render'], (render) => { ... })`
- Building print URLs by hand and manipulating them is not a supported way to print; use these
  functions instead.
- Related: [Advanced templates](../topics/advanced_templates.md),
  [BFO markup](../topics/advanced_templates.bfo.md),
  [Scriptable templates](../topics/scriptable_templates.md), [N/file](N_file.md),
  [N/email](N_email.md), [N/xml](N_xml.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`render.transaction(options)`](#rendertransactionoptions) | function | Print a transaction to PDF or HTML |
| [`render.statement(options)`](#renderstatementoptions) | function | Print a customer statement |
| [`render.pickingTicket(options)`](#renderpickingticketoptions) | function | Print a picking ticket |
| [`render.packingSlip(options)`](#renderpackingslipoptions) | function | Print a packing slip |
| [`render.bom(options)`](#renderbomoptions) | function | Print a bill of materials |
| [`render.xmlToPdf(options)`](#renderxmltopdfoptions) | function | Convert BFO XML markup to a PDF file |
| [`render.create()`](#rendercreate) | function | Get a `TemplateRenderer` for an advanced template |
| [`render.mergeEmail(options)`](#rendermergeemailoptions) | function | Merge a scriptable email template |
| [`TemplateRenderer`](#templaterenderer-object) | object | Binds data to a template and renders it |
| [`EmailMergeResult`](#emailmergeresult-object) | object | Subject and body produced by `mergeEmail` |
| [`render.PrintMode`](#renderprintmode) | enum | Output format for the printing functions |
| [`render.DataSource`](#renderdatasource) | enum | Format of a custom data source |

## Functions

### render.transaction(options)
<!-- id: N/render.transaction -->

Prints one transaction and gives you the result as a file. When Advanced Printing is enabled, the
layout comes from the advanced template attached to the transaction's custom form. The output file
can be at most 10 MB.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.entityId` | number | Yes | |
| `options.printMode` | enum | No | |
| `options.formId` | number | No | |
| `options.inCustLocale` | boolean | No | |

- `options.entityId`: the internal id of the transaction to print (despite the parameter's name, it
  identifies the transaction, not an entity).
- `options.printMode`: a [`render.PrintMode`](#renderprintmode) value. When omitted, the company's
  or user's printing preference decides the format. Here the type is given as `enum`, while the
  other printing functions type the same option as `string`; either way you pass a
  `render.PrintMode` value.
- `options.formId`: the transaction form number to print with.
- `options.inCustLocale`: only matters when advanced templates are used; `true` prints in the
  customer's locale. With basic printing it is ignored, because basic printing always uses the
  customer's locale.

**Returns:** `file.File`, containing the PDF or HTML document.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### render.statement(options)
<!-- id: N/render.statement -->

Prints a customer statement and returns it as a file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.entityId` | number | Yes | |
| `options.printMode` | string | No | |
| `options.formId` | number | No | |
| `options.inCustLocale` | boolean | No | |
| `options.consolidateStatements` | boolean | No | |
| `options.openTransactionsOnly` | boolean | No | |
| `options.startDate` | Date | No | |
| `options.statementDate` | Date | No | |
| `options.subsidiaryId` | Integer | No | |

- `options.entityId`: internal id of the entity (customer) the statement is for.
- `options.printMode`: a [`render.PrintMode`](#renderprintmode) value; defaults to the company's or
  user's printing preference.
- `options.formId`: the form to print with; the source gives no further description.
- `options.inCustLocale`: as for [`render.transaction`](#rendertransactionoptions): honoured only
  with advanced templates; basic printing always uses the customer's locale.
- `options.consolidateStatements`: `true` converts amounts to the base currency.
- `options.openTransactionsOnly`: `true` lists only open transactions.
- `options.startDate`: the date of the oldest transaction to show.
- `options.statementDate`: the date printed as the statement date.
- `options.subsidiaryId`: applies when Advanced Printing is in use.

**Returns:** `file.File`, containing the PDF or HTML document.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### render.pickingTicket(options)
<!-- id: N/render.pickingTicket -->

Prints a picking ticket and returns it as a file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.entityId` | number | Yes | |
| `options.printMode` | string | No | |
| `options.formId` | number | No | |
| `options.inCustLocale` | boolean | No | |
| `options.shipgroup` | number | No | |
| `options.location` | number | No | |

- `options.entityId`: the internal id of the transaction the ticket belongs to.
- `options.printMode`: a [`render.PrintMode`](#renderprintmode) value; defaults to the company's or
  user's printing preference.
- `options.inCustLocale`: honoured only with advanced templates; basic printing always uses the
  customer's locale.
- `options.shipgroup`: the shipping group the ticket covers.
- `options.location`: the location the ticket covers.
- `options.formId`: the form to print with.

> [!SOURCE-ERROR] The source describes this function's form id as a *packing slip* form, which looks
> copied from `render.packingSlip`. Presumably it should be a picking-ticket form, but the spec
> does not confirm that; verify which form id is accepted.
<!-- source-error: N/render.pickingTicket | - | form_id_described_as_packing_slip_form -->

**Returns:** `file.File`, containing the PDF or HTML document.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### render.packingSlip(options)
<!-- id: N/render.packingSlip -->

Prints a packing slip and returns it as a file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.entityId` | number | Yes | |
| `options.printMode` | string | No | |
| `options.formId` | number | No | |
| `options.inCustLocale` | boolean | No | |
| `options.fulfillmentId` | number | No | |

- `options.entityId`: the internal id of the transaction the slip belongs to.
- `options.printMode`: a [`render.PrintMode`](#renderprintmode) value; defaults to the company's or
  user's printing preference.
- `options.formId`: the form to print with; the source gives no further description.
- `options.inCustLocale`: honoured only with advanced templates; basic printing always uses the
  customer's locale.
- `options.fulfillmentId`: the fulfillment to print.

**Returns:** `file.File`, containing the PDF or HTML document.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### render.bom(options)
<!-- id: N/render.bom -->

Prints a bill of materials document and returns it as a file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.entityId` | number | Yes | |
| `options.printMode` | string | No | |
| `options.inCustLocale` | boolean | No | |

- `options.entityId`: the internal id of the transaction to print.
- `options.printMode`: a [`render.PrintMode`](#renderprintmode) value; defaults to the company's or
  user's printing preference.
- `options.inCustLocale`: honoured only with advanced templates; basic printing always uses the
  customer's locale.

**Returns:** `file.File`, containing the PDF or HTML document.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### render.xmlToPdf(options)
<!-- id: N/render.xmlToPdf -->

Converts XML markup written for the BFO report library into a PDF. A common pattern is to render a
template to a string with [`TemplateRenderer.renderAsString()`](#templaterendererrenderasstring)
and pass that string here. The resulting file can be at most 10 MB. This is the 2.x counterpart of
1.0's `nlapiXMLToPDF`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.xmlString` | xml.Document \| string | Yes | |

- `options.xmlString`: either a markup string or an already parsed `xml.Document` (see
  [N/xml](N_xml.md)).

**Returns:** `file.File`, the PDF.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### render.create()
<!-- id: N/render.create -->

Returns a new [`TemplateRenderer`](#templaterenderer-object). Templates are FreeMarker. You can send
the rendered result to a Suitelet's `http.ServerResponse` as HTML, or render it to an XML string
and hand that to [`render.xmlToPdf`](#renderxmltopdfoptions). Requires the Advanced Printing
(`ADVANCEDPRINTING`) feature.

**Returns:** `render.TemplateRenderer`

**Governance:** 0 units.

**Availability:** server scripts; feature `ADVANCEDPRINTING`.

**Since:** 2015.2

### render.mergeEmail(options)
<!-- id: N/render.mergeEmail -->

Runs a mail merge against a scriptable email template and gives you the merged subject and body.
It does not send anything: you need [`email.send`](N_email.md) to deliver the result and to add
attachments. See [Scriptable templates](../topics/scriptable_templates.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.templateId` | number | Yes | |
| `options.recipient` | RecordRef | No | |
| `options.entity` | RecordRef | No | |
| `options.customRecord` | RecordRef | No | |
| `options.supportCaseId` | number | No | |
| `options.transactionId` | number | No | |

- `RecordRef` values are plain objects with two required members: `id` (number) and `type`
  (string).
- `options.templateId`: the internal id of the scriptable email template.
- `options.recipient`: conditionally required. You do not have to name the recipient here: it can
  be set on the [`email.send`](N_email.md) call that delivers the merged message instead. The
  source does not state any other condition under which this option becomes mandatory.
- `options.entity`, `options.customRecord`, `options.supportCaseId`, `options.transactionId`: the
  source gives only their names and types (two `RecordRef`s and two numeric ids) and does not
  describe what each one contributes to the merge.

**Returns:** `render.EmailMergeResult`, holding the merged subject and body.

**Errors:** the source does not state which errors this function can raise.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Objects

### TemplateRenderer (object)
<!-- id: N/render.TemplateRenderer -->

Produces HTML or PDF documents from advanced (FreeMarker) templates. Typical flow: get one from
[`render.create()`](#rendercreate); choose a template (by id, by script id, or by assigning
`templateContent`); bind data with `addRecord`, `addSearchResults`, `addQuery` (a query data source)
or `addCustomDataSource` (custom XML or JSON); then render. Output can go to an
`http.ServerResponse` as HTML, or be produced as an XML string for
[`render.xmlToPdf`](#renderxmltopdfoptions). Requires the `ADVANCEDPRINTING` feature. Server
scripts; since 2015.2.

#### TemplateRenderer.templateContent
<!-- id: N/render.TemplateRenderer#templateContent -->

`string`. The template source itself. You can assign any template source string here instead of
pointing at a stored template. Server scripts; since 2015.2.

#### TemplateRenderer.setTemplateById(options)
<!-- id: N/render.TemplateRenderer#setTemplateById -->

Selects a stored template by its numeric id. Both standard and advanced templates are accepted. To
find an id, look at the URL behind the Edit or Customize link in the template list (hovering or
clicking the link shows it). For instance, a standard PDF template might have id 4 and an advanced
HTML template id 19.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.1

#### TemplateRenderer.setTemplateByScriptId(options)
<!-- id: N/render.TemplateRenderer#setTemplateByScriptId -->

Selects a stored template by its script id. Template script ids start with `custtmpl`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.1

#### TemplateRenderer.addRecord(options)
<!-- id: N/render.TemplateRenderer#addRecord -->

Makes a record available to the template under a variable name of your choice.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.templateName` | string | Yes | |
| `options.record` | record.Record | Yes | |

- `options.templateName`: the variable name the template uses to reach the record.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### TemplateRenderer.addSearchResults(options)
<!-- id: N/render.TemplateRenderer#addSearchResults -->

Makes search results available to the template under a variable name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.templateName` | string | Yes | |
| `options.searchResult` | search.Result | Yes | |

- `options.templateName`: the variable name the template uses for the results.
- `options.searchResult`: typed as a single `search.Result`.

> [!SOURCE-ERROR] The source types `options.searchResult` as one `search.Result`, but its syntax
> sample passes an array of results. Which shape the method really expects is not settled; test
> with your own data before relying on either.
<!-- source-error: N/render.TemplateRenderer#addSearchResults | options.searchResult | type_single_result_but_syntax_passes_result_array -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### TemplateRenderer.addQuery(options)
<!-- id: N/render.TemplateRenderer#addQuery -->

Binds the results of a query to a template variable. Give the query either as a `query.Query`
object (`options.query`) or as the id of a saved workbook (`options.id`), never both. At most 5000
rows come back; when you expect more, page through them with `pageIndex` and `pageSize`.

In the template the results are exposed through an iterator that only moves forward, so you cannot
jump around in them. If the template needs random access to the rows, process the data in the
script first and pass it in through
[`addCustomDataSource`](#templaterendereraddcustomdatasourceoptions) instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.templateName` | string | Yes | |
| `options.query` | query.Query | No | |
| `options.id` | string | No | |
| `options.pageIndex` | number | No | |
| `options.pageSize` | number | No | |

- `options.templateName`: the variable name the template uses for the results.
- `options.query` / `options.id`: conditionally required. Exactly one of the two must be given;
  they are mutually exclusive.
- `options.pageSize`: between 5 and 1000 rows.

**Returns:** `void`

**Errors:**

- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `options.query` and `options.id` were given.
- `NEITHER_ARGUMENT_DEFINED`: neither `options.id` nor `options.query` was given.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.
- `UNABLE_TO_LOAD_QUERY`: the query or workbook could not be loaded from the value given.
- `WRONG_PARAMETER_TYPE`: an argument has the wrong type.

**Governance:** 0 units for the call itself. The spec adds that rendering costs 10 units per
iteration over the query results, plus 5 more when a workbook id is used; it does not reconcile
these figures with the 0 listed for the method.

**Availability:** server scripts.

**Since:** 2019.2

#### TemplateRenderer.addCustomDataSource(options)
<!-- id: N/render.TemplateRenderer#addCustomDataSource -->

Feeds your own XML or JSON data (or a plain object) to the template under an alias. Two caveats:
every value arrives in the template as an untyped string, and JSON keys must be valid XML names.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.format` | render.DataSource | Yes | |
| `options.alias` | string | Yes | |
| `options.data` | Object \| Document \| string | Yes | |

- `options.format`: a [`render.DataSource`](#renderdatasource) value describing what `data` is.
- `options.alias`: the name the template uses to reach this data.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.1

#### TemplateRenderer.renderAsString()
<!-- id: N/render.TemplateRenderer#renderAsString -->

Renders the template with the bound data and returns the output as a string. Pair it with
[`render.xmlToPdf`](#renderxmltopdfoptions) when you want to post-process the markup before
producing a PDF.

**Returns:** `string`, the rendered template.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### TemplateRenderer.renderAsPdf()
<!-- id: N/render.TemplateRenderer#renderAsPdf -->

Renders the template straight to a PDF file.

**Returns:** `file.File`, the PDF.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### TemplateRenderer.renderPdfToResponse(options)
<!-- id: N/render.TemplateRenderer#renderPdfToResponse -->

Renders the template as a PDF and writes it into a server response, so the user's browser can show
or download it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.response` | http.ServerResponse | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### TemplateRenderer.renderToResponse(options)
<!-- id: N/render.TemplateRenderer#renderToResponse -->

Renders the template and writes the output into a server response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.response` | http.ServerResponse | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### EmailMergeResult (object)
<!-- id: N/render.EmailMergeResult -->

What [`render.mergeEmail`](#rendermergeemailoptions) gives back. Server scripts; since 2015.2.

#### EmailMergeResult.body
<!-- id: N/render.EmailMergeResult#body -->

`string`, read-only. The merged email body. Since 2015.2.

#### EmailMergeResult.subject
<!-- id: N/render.EmailMergeResult#subject -->

`string`, read-only. The merged email subject. Since 2015.2.

## Enums

### render.PrintMode
<!-- id: N/render.PrintMode -->

The output format for the printing functions: [`bom`](#renderbomoptions),
[`pickingTicket`](#renderpickingticketoptions), [`statement`](#renderstatementoptions),
[`transaction`](#rendertransactionoptions) and [`packingSlip`](#renderpackingslipoptions). Since
2015.2. (The module-level description names only `bom`, `pickingTicket` and `statement` as users of
this enum, but the `printMode` option of `transaction` and `packingSlip` also takes its values.)

| Value |
|---|
| `DEFAULT` |
| `HTML` |
| `PDF` |

The source lists these names without stating what each value produces. What it does state is the
behaviour when the option is omitted: the company's or user's printing preference applies.

### render.DataSource
<!-- id: N/render.DataSource -->

The format of the `data` passed to
[`TemplateRenderer.addCustomDataSource`](#templaterendereraddcustomdatasourceoptions). Since 2015.2.

| Value |
|---|
| `JSON` |
| `OBJECT` |
| `XML_DOC` |
| `XML_STRING` |

The source does not state the meaning of each value or which `data` type goes with which. It only
says that `data` may be an Object, a Document or a string and that the data source carries XML or
JSON.

## Examples

A Suitelet that shows a receiving checklist for a purchase order as a PDF in the browser. The
template is stored in the account under a `custtmpl` script id; the PO record and a small block of
dock information are bound to it.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/render', 'N/record'], (render, record) => {
  const onRequest = (context) => {
    const poId = Number(context.request.parameters.po);
    const purchaseOrder = record.load({ type: record.Type.PURCHASE_ORDER, id: poId });

    const renderer = render.create();
    renderer.setTemplateByScriptId({ scriptId: 'custtmpl_po_receiving_checklist' });
    renderer.addRecord({ templateName: 'po', record: purchaseOrder });
    renderer.addCustomDataSource({
      format: render.DataSource.OBJECT,
      alias: 'dock',
      data: { door: 'B3', shift: 'early', printedAt: new Date().toISOString() }
    });

    renderer.renderPdfToResponse({ response: context.response });
  };

  return { onRequest };
});
```

Producing month-end statements for a list of customers and filing them in the File Cabinet. Each
`render.statement` call costs 10 units and each save 20, so budget accordingly.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/render', 'N/log'], (render, log) => {
  const STATEMENT_FOLDER_ID = 1440;
  const CUSTOMER_IDS = [3381, 3392, 4017];

  const execute = () => {
    const statementDate = new Date();
    const startDate = new Date(statementDate.getFullYear(), statementDate.getMonth() - 3, 1);

    CUSTOMER_IDS.forEach((customerId) => {
      const pdf = render.statement({
        entityId: customerId,
        printMode: render.PrintMode.PDF,
        startDate,
        statementDate,
        openTransactionsOnly: true
      });
      pdf.name = `statement_${customerId}_${Date.now()}.pdf`;
      pdf.folder = STATEMENT_FOLDER_ID;
      const fileId = pdf.save();
      log.debug({ title: `Statement for ${customerId}`, details: fileId });
    });
  };

  return { execute };
});
```

Rendering markup to a string, adjusting it, and converting it to a PDF. The inline template is
FreeMarker producing BFO markup; see [BFO markup](../topics/advanced_templates.bfo.md) for what the
converter accepts. User-supplied text is escaped with `xml.escape` before it is spliced in.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/render', 'N/xml'], (render, xml) => {
  const buildLabelPdf = (palletCode, note) => {
    const renderer = render.create();
    renderer.templateContent =
      '<pdf><body><p>Pallet ${label.code}</p><p>NOTE_PLACEHOLDER</p></body></pdf>';
    renderer.addCustomDataSource({
      format: render.DataSource.JSON,
      alias: 'label',
      data: JSON.stringify({ code: palletCode })
    });

    const markup = renderer
      .renderAsString()
      .replace('NOTE_PLACEHOLDER', xml.escape({ xmlText: note }));

    return render.xmlToPdf({ xmlString: markup });
  };

  return { buildLabelPdf };
});
```

Merging a scriptable email template for a new order and sending it with the order PDF attached.
The recipient is named only on `email.send`, which the merge allows.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/render', 'N/email'], (render, email) => {
  const ORDER_CONFIRMATION_TEMPLATE = 58;
  const SENDER_EMPLOYEE_ID = 207;

  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const order = context.newRecord;
    const customerId = Number(order.getValue({ fieldId: 'entity' }));

    const merged = render.mergeEmail({
      templateId: ORDER_CONFIRMATION_TEMPLATE,
      entity: { type: 'customer', id: customerId },
      transactionId: order.id
    });

    const orderPdf = render.transaction({
      entityId: order.id,
      printMode: render.PrintMode.PDF
    });

    email.send({
      author: SENDER_EMPLOYEE_ID,
      recipients: customerId,
      subject: merged.subject,
      body: merged.body,
      attachments: [orderPdf]
    });
  };

  return { afterSubmit };
});
```
