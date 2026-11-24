# N/documentCapture
<!-- id: N/documentCapture -->

`N/documentCapture` extracts content from documents kept in the File Cabinet, such as invoices,
receipts and contracts. OCI Document Understanding does the work behind it. The module can:

- extract plain text, tables and labelled key/value fields;
- classify what kind of document a page is;
- attach a confidence score to each extracted item;
- handle many languages.

Two functions are the main entry points:

- [`documentCapture.documentToText`](#documentcapturedocumenttotextoptions) returns the plain text
  of a PDF, ready to pass to [`llm.generateText`](N_llm.md#llmgeneratetextoptions).
- [`documentCapture.documentToStructure`](#documentcapturedocumenttostructureoptions) returns a
  structured [`Document`](#document-object) (pages with lines, words, tables and fields) for PDF,
  JPG, PNG or TIFF files.

`documentToStructure` stops at 5 pages per call. For longer files, run the asynchronous
`DocumentCaptureTask` of [N/task](N_task.md); it saves its result as JSON in the File Cabinet, and
[`documentCapture.parseResult`](#documentcaptureparseresultoptions) turns that saved JSON back into
a `Document`.

- **Availability:** server scripts only; the Server SuiteScript feature is required.
- **File types:** PDF, PNG, JPG and TIFF in general; `documentToText` takes PDF only. Encrypted
  files are not supported.
- **Concurrency:** at most 5 requests at once; check the free slots with
  [`getRemainingConcurrency`](#documentcapturegetremainingconcurrency).
- **Deprecated:** `getRemainingFreeUsage` (and its promise form), which now delegates to
  [`llm.getRemainingUsage`](N_llm.md#llmgetremainingusage).
- **Load it as:** `define(['N/documentCapture'], (documentCapture) => { ... })`
- Related: [N/llm](N_llm.md), [N/task](N_task.md), [N/file](N_file.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`documentCapture.documentToStructure(options)`](#documentcapturedocumenttostructureoptions) | function | Extract text, tables, fields and document types (up to 5 pages) |
| [`documentCapture.documentToStructure.promise(options)`](#documentcapturedocumenttostructurepromiseoptions) | function | Promise form of `documentToStructure` |
| [`documentCapture.documentToText(options)`](#documentcapturedocumenttotextoptions) | function | Plain text of a PDF |
| [`documentCapture.documentToText.promise(options)`](#documentcapturedocumenttotextpromiseoptions) | function | Promise form of `documentToText` |
| [`documentCapture.parseResult(options)`](#documentcaptureparseresultoptions) | function | Turn a saved async-task JSON result into a `Document` |
| [`documentCapture.getRemainingConcurrency()`](#documentcapturegetremainingconcurrency) | function | Free request slots right now |
| [`documentCapture.getRemainingConcurrency.promise()`](#documentcapturegetremainingconcurrencypromise) | function | Promise form |
| [`documentCapture.getRemainingFreeUsage()`](#documentcapturegetremainingfreeusage) | function | Deprecated; remaining AI units |
| [`documentCapture.getRemainingFreeUsage.promise()`](#documentcapturegetremainingfreeusagepromise) | function | Deprecated promise form |
| [`Document`](#document-object) | object | Extraction result |
| [`Page`](#page-object) | object | One page of the result |
| [`Line`](#line-object) | object | A line of text |
| [`Word`](#word-object) | object | A word |
| [`Table`](#table-object) | object | A table |
| [`TableRow`](#tablerow-object) | object | A table row |
| [`Cell`](#cell-object) | object | A table cell |
| [`Field`](#field-object) | object | A key/value field |
| [`FieldLabel`](#fieldlabel-object) | object | The label part of a field |
| [`FieldValue`](#fieldvalue-object) | object | The value part of a field |
| [`documentCapture.DocumentType`](#documentcapturedocumenttype) | enum | Kind of document to expect |
| [`documentCapture.Feature`](#documentcapturefeature) | enum | What to extract |
| [`documentCapture.FieldType`](#documentcapturefieldtype) | enum | Kind of extracted field |
| [`documentCapture.Language`](#documentcapturelanguage) | enum | Document language |

## Functions

### documentCapture.documentToStructure(options)
<!-- id: N/documentCapture.documentToStructure -->

Analyses a PDF, JPG, PNG or TIFF file of up to 5 pages and returns its text, tables and fields as
a structured [`Document`](#document-object). Reach for it when you need tables or labelled
fields, or when the file is an image. What comes back depends on `options.features`; in
particular, fields are returned only when `FIELD_EXTRACTION` is requested. The file must live in
the File Cabinet, addressed by internal ID or path. Encrypted files are not supported, and at most
5 requests may run at once. For documents over 5 pages, use the asynchronous `DocumentCaptureTask`
of [N/task](N_task.md) instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.documentType` | string | No | `OTHERS` |
| `options.features` | string[] | No | |
| `options.language` | string | No | `ENG` |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |

- `options.file`: the file to analyse; it may also be given as the file's internal ID or its File
  Cabinet path. Since 2025.2.
- `options.documentType`: a [`documentCapture.DocumentType`](#documentcapturedocumenttype) value,
  selecting a model pretrained for that kind of document. Defaults to `OTHERS`; required when
  `FIELD_EXTRACTION` is requested. Since 2025.2.
- `options.features`: [`documentCapture.Feature`](#documentcapturefeature) values. When omitted,
  text and table extraction (`TEXT_EXTRACTION` and `TABLE_EXTRACTION`) are performed. An empty
  array is an error. Since 2025.2.
- `options.language`: a [`documentCapture.Language`](#documentcapturelanguage) value; defaults to
  `ENG`. Since 2025.2.
- `options.ociConfig`: no longer supported and always ignored. Since 2025.2.
- `options.timeout`: in milliseconds, with a minimum of 30000; a shorter value is replaced by the
  default of 30000. Since 2025.2.

**Returns:** [`documentCapture.Document`](#document-object), holding the text, tables and fields
the requested features produce.

**Errors:**

- `DOCUMENT_TOO_LONG`: the file has more than 5 pages.
- `FEATURES_CANNOT_BE_EMPTY`: `options.features` is an empty array.
- `FEATURE_1_DOES_NOT_SUPPORT_LANGUAGE_2`: a feature in `options.features` is not available for
  the chosen language.
- `INCOMPATIBLE_DOCUMENT_TYPE_FOR_FEATURE_1`: a feature in `options.features` is not supported for
  the chosen document type.
- `INVALID_DOCUMENT_CAPTURE_RESULT`: the service returned a result that is not valid.
- `INVALID_DOCUMENT_TYPE`: `options.documentType` is not a `DocumentType` value.
- `INVALID_LANGUAGE`: `options.language` is not a `Language` value.
- `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`: more than 5 requests are running at once.
- `SSS_MISSING_REQD_ARGUMENT`: `options.file` is missing.
- `UNSUPPORTED_FILE_TYPE`: the file is not a PDF, JPG, PNG or TIFF.

The codes containing `_1_` or `_2` are written exactly as the source gives them; the digits look
like slots for a feature or language name, so check the code your account actually returns.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.documentToStructure.promise(options)
<!-- id: N/documentCapture.documentToStructure.promise -->

The asynchronous form of [`documentToStructure`](#documentcapturedocumenttostructureoptions): same
parameters, same errors, returns a Promise. The same limits hold: PDF, JPG, PNG or TIFF only, at
most 5 pages, no encrypted files; longer documents need the `DocumentCaptureTask` of
[N/task](N_task.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.documentType` | string | No | `OTHERS` |
| `options.features` | string[] | No | |
| `options.language` | string | No | `ENG` |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |

**Returns:** `Promise`. The source does not state what it settles with (the synchronous form
returns a `Document`).

**Errors:** the same as `documentToStructure`.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.documentToText(options)
<!-- id: N/documentCapture.documentToText -->

Returns the text of a PDF as one plain string, whatever the PDF's length. A typical next step is
handing the text to [`llm.generateText`](N_llm.md#llmgeneratetextoptions), for example to ask what
an invoice is for. Only PDFs are accepted; for JPG, PNG or TIFF files, or when you need tables or
fields, use [`documentToStructure`](#documentcapturedocumenttostructureoptions). Encrypted files
are not supported, and at most 5 requests can run at once.

The source states no hard file-size limit for this function. It only recommends keeping File
Cabinet files under 100 MB and splitting large files so the call does not time out.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.timeout` | number | No | `30000` |

- `options.file`: the PDF; it may also be given as its internal ID or File Cabinet path. Since
  2025.2.
- `options.timeout`: in milliseconds, with a minimum of 30000; a shorter value is replaced by the
  default of 30000. Since 2025.2.

**Returns:** `string`, the PDF's text as plain text.

**Errors:**

- `FILE_CANNOT_BE_EMPTY`: the file is empty.
- `FILE_CORRUPTED_OR_INVALID`: the file cannot be parsed.
- `UNSUPPORTED_ENCODING_EXCEPTION`: the file contains characters that are not valid.
- `UNSUPPORTED_FILE_TYPE_1_USE_2`: the file is not a PDF. The code is written as the source has
  it; the digits look like slots, so verify the actual code.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.documentToText.promise(options)
<!-- id: N/documentCapture.documentToText.promise -->

The asynchronous form of [`documentToText`](#documentcapturedocumenttotextoptions). It works on
PDFs only, takes the same parameters, raises the same errors and returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.timeout` | number | No | `30000` |

**Returns:** `Promise`. The source does not state what it settles with (the synchronous form
returns a string).

**Errors:** the same as `documentToText`.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.parseResult(options)
<!-- id: N/documentCapture.parseResult -->

Reads the JSON result that an asynchronous `DocumentCaptureTask` ([N/task](N_task.md)) saved, and
returns it as a [`Document`](#document-object). From there you walk the result exactly as you
would the output of `documentToStructure`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |

- `options.file`: the JSON result file, which must be in the File Cabinet. The source types it as
  `file.File`; unlike the two extraction functions, it does not state whether an internal ID or a
  path is also accepted here, so pass a loaded file object. Since 2025.2.

**Returns:** [`documentCapture.Document`](#document-object), rebuilt from the task's JSON output.

**Errors:**

- `UNSUPPORTED_FILE_TYPE_1_USE_2`: the file is not a supported type (the code is written as the
  source has it, with slot-like digits).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.getRemainingConcurrency()
<!-- id: N/documentCapture.getRemainingConcurrency -->

Reports how many more document capture requests can start right now, out of a maximum of 5 at
once. Call it before starting a batch to avoid `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`.

**Returns:** `number`, the remaining concurrent request slots.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.getRemainingConcurrency.promise()
<!-- id: N/documentCapture.getRemainingConcurrency.promise -->

The asynchronous form of [`getRemainingConcurrency`](#documentcapturegetremainingconcurrency). Like
the synchronous form it takes no parameters and lists no errors; it reports the remaining
concurrent request slots (maximum 5) through a Promise.

**Returns:** `Promise`. The source does not state the settled value's type (the synchronous form
returns a number).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### documentCapture.getRemainingFreeUsage()
<!-- id: N/documentCapture.getRemainingFreeUsage -->

**Deprecated.** Returns the remaining AI units by delegating to
[`llm.getRemainingUsage`](N_llm.md#llmgetremainingusage); call that in new code.

**Returns:** `number`, the remaining AI units.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2 (deprecated).

### documentCapture.getRemainingFreeUsage.promise()
<!-- id: N/documentCapture.getRemainingFreeUsage.promise -->

**Deprecated.** The asynchronous form of `getRemainingFreeUsage`; it delegates to
[`llm.getRemainingUsage.promise`](N_llm.md#llmgetremainingusagepromise).

**Returns:** `Promise`. The source does not state the settled value.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2 (deprecated).

## Objects

All objects below are server-side and available since 2025.2. Their properties are read-only;
assigning one throws `READ_ONLY`.

Every `confidence` property is a number from 0 to 1 expressing how sure the service is about the
extracted item. Choose a threshold that suits your process, accept items above it, and route the
rest to a person.

### Document (object)
<!-- id: N/documentCapture.Document -->

The extraction result, returned by
[`documentToStructure`](#documentcapturedocumenttostructureoptions) and
[`parseResult`](#documentcaptureparseresultoptions). It carries a `mimeType` and a list of
`pages`, each holding fields, lines, tables and words. Which of those are filled depends on the
requested features; with no features given, text and table extraction are used.

#### Document.mimeType
<!-- id: N/documentCapture.Document#mimeType -->

`string`, read-only. The analysed file's MIME type: `image/jpeg`, `application/pdf`, `image/png`
or `image/tiff`. Assigning it throws `READ_ONLY`.

#### Document.pages
<!-- id: N/documentCapture.Document#pages -->

`documentCapture.Page[]`, read-only. One [`Page`](#page-object) per page. A synchronous extraction
yields at most 5 pages; a result from the asynchronous task has no page limit. Assigning it throws
`READ_ONLY`.

#### Document.getText()
<!-- id: N/documentCapture.Document#getText -->

Returns the text of the whole document as one string.

**Returns:** `string`, the entire document's text.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### Page (object)
<!-- id: N/documentCapture.Page -->

One page of a [`Document`](#document-object), with `fields`, `lines`, `tables` and `words` (plus
`detectedDocumentTypes` when classification was requested). When no features were requested,
text and table extraction fill it.

#### Page.lines
<!-- id: N/documentCapture.Page#lines -->

`documentCapture.Line[]`, read-only. The page's [`Line`](#line-object) objects, filled only when
`TEXT_EXTRACTION` was requested. Assigning it throws `READ_ONLY`.

#### Page.words
<!-- id: N/documentCapture.Page#words -->

`documentCapture.Word[]`, read-only. The page's [`Word`](#word-object) objects, filled only when
`TEXT_EXTRACTION` was requested. Assigning it throws `READ_ONLY`.

#### Page.tables
<!-- id: N/documentCapture.Page#tables -->

`documentCapture.Table[]`, read-only. The page's [`Table`](#table-object) objects, filled only
when `TABLE_EXTRACTION` was requested. Assigning it throws `READ_ONLY`.

#### Page.fields
<!-- id: N/documentCapture.Page#fields -->

`documentCapture.Field[]`, read-only. The page's key/value [`Field`](#field-object) objects,
filled only when `FIELD_EXTRACTION` was requested. Assigning it throws `READ_ONLY`.

#### Page.detectedDocumentTypes
<!-- id: N/documentCapture.Page#detectedDocumentTypes -->

`Object[]`, read-only. The document types the service considers this page might be. Each entry
has a `documentType` and a `confidence` from 0 to 1, for example an invoice at 0.95. Filled only
when `DOCUMENT_CLASSIFICATION` was requested. Assigning it throws `READ_ONLY`.

#### Page.getText()
<!-- id: N/documentCapture.Page#getText -->

Returns all the text on this page as one string.

**Returns:** `string`, the entire page's text.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### Line (object)
<!-- id: N/documentCapture.Line -->

The text on one line as laid out on the page; a line is a visual line, not a sentence. Filled only
with `TEXT_EXTRACTION`.

#### Line.text
<!-- id: N/documentCapture.Line#text -->

`string`, read-only. The line's text. Assigning it throws `READ_ONLY`.

#### Line.confidence
<!-- id: N/documentCapture.Line#confidence -->

`number`, read-only, 0 to 1. How sure the service is about this line's text; accept values above a
threshold you choose. Assigning it throws `READ_ONLY`.

### Word (object)
<!-- id: N/documentCapture.Word -->

A single word with its confidence. Filled only with `TEXT_EXTRACTION`.

#### Word.text
<!-- id: N/documentCapture.Word#text -->

`string`, read-only. The word. Assigning it throws `READ_ONLY`.

#### Word.confidence
<!-- id: N/documentCapture.Word#confidence -->

`number`, read-only, 0 to 1. How sure the service is about this word; accept values above a
threshold you choose. Assigning it throws `READ_ONLY`.

### Table (object)
<!-- id: N/documentCapture.Table -->

A table found on a page, split into header, body and footer rows, with row and column counts and
an overall confidence. Filled only with `TABLE_EXTRACTION`.

#### Table.headerRows
<!-- id: N/documentCapture.Table#headerRows -->

`documentCapture.TableRow[]`, read-only. The header [rows](#tablerow-object). Assigning it throws
`READ_ONLY`.

#### Table.bodyRows
<!-- id: N/documentCapture.Table#bodyRows -->

`documentCapture.TableRow[]`, read-only. The body rows only, without header and footer rows.
Assigning it throws `READ_ONLY`.

#### Table.footerRows
<!-- id: N/documentCapture.Table#footerRows -->

`documentCapture.TableRow[]`, read-only. The footer rows, which typically hold a subtotal, tax or
total. Assigning it throws `READ_ONLY`.

#### Table.rowCount
<!-- id: N/documentCapture.Table#rowCount -->

`number`, read-only. The number of rows, counting header, body and footer rows together. Assigning
it throws `READ_ONLY`.

#### Table.columnCount
<!-- id: N/documentCapture.Table#columnCount -->

`number`, read-only. The number of columns. Assigning it throws `READ_ONLY`.

#### Table.confidence
<!-- id: N/documentCapture.Table#confidence -->

`number`, read-only, 0 to 1. How sure the service is about the extracted table; accept values
above a threshold you choose. Assigning it throws `READ_ONLY`.

### TableRow (object)
<!-- id: N/documentCapture.TableRow -->

One row of a [`Table`](#table-object), made of cells.

#### TableRow.cells
<!-- id: N/documentCapture.TableRow#cells -->

`documentCapture.Cell[]`, read-only. The row's [cells](#cell-object). Assigning it throws
`READ_ONLY`.

### Cell (object)
<!-- id: N/documentCapture.Cell -->

One table cell, with its text and confidence.

#### Cell.text
<!-- id: N/documentCapture.Cell#text -->

`string`, read-only. The cell's text. Assigning it throws `READ_ONLY`.

#### Cell.confidence
<!-- id: N/documentCapture.Cell#confidence -->

`number`, read-only, 0 to 1. How sure the service is about the cell's text; accept values above a
threshold you choose. Assigning it throws `READ_ONLY`.

### Field (object)
<!-- id: N/documentCapture.Field -->

A key/value pair found in the document, such as an invoice number next to its label. It has a
`label`, a `type` and a `value`. Filled only with `FIELD_EXTRACTION`.

#### Field.label
<!-- id: N/documentCapture.Field#label -->

`documentCapture.FieldLabel`, read-only. The [label](#fieldlabel-object) half of the pair.
Assigning it throws `READ_ONLY`.

#### Field.type
<!-- id: N/documentCapture.Field#type -->

`string`, read-only. A [`documentCapture.FieldType`](#documentcapturefieldtype) value. Assigning
it throws `READ_ONLY`.

#### Field.value
<!-- id: N/documentCapture.Field#value -->

`documentCapture.FieldValue`, read-only. The [value](#fieldvalue-object) half of the pair.
Assigning it throws `READ_ONLY`.

### FieldLabel (object)
<!-- id: N/documentCapture.FieldLabel -->

The label of a [`Field`](#field-object), with its name and confidence.

#### FieldLabel.name
<!-- id: N/documentCapture.FieldLabel#name -->

`string`, read-only. The label's name. Assigning it throws `READ_ONLY`.

#### FieldLabel.confidence
<!-- id: N/documentCapture.FieldLabel#confidence -->

`number`, read-only, 0 to 1. How sure the service is about the label; accept values above a
threshold you choose. Assigning it throws `READ_ONLY`.

### FieldValue (object)
<!-- id: N/documentCapture.FieldValue -->

The value of a [`Field`](#field-object), with its text and confidence.

#### FieldValue.text
<!-- id: N/documentCapture.FieldValue#text -->

`string`, read-only. The extracted value as text. Assigning it throws `READ_ONLY`.

#### FieldValue.confidence
<!-- id: N/documentCapture.FieldValue#confidence -->

`number`, read-only, 0 to 1. How sure the service is about the value; accept values above a
threshold you choose. Assigning it throws `READ_ONLY`.

## Enums

The source lists only the names of these enum values; it does not state the underlying string
each name stands for. Always refer to them through the enum (for example
`documentCapture.Feature.TEXT_EXTRACTION`).

### documentCapture.DocumentType
<!-- id: N/documentCapture.DocumentType -->

For `options.documentType` of
[`documentToStructure`](#documentcapturedocumenttostructureoptions); it picks the model pretrained
for that kind of document. `OTHERS` is the default. Since 2025.2.

| Value | Value | Value | Value |
|---|---|---|---|
| `BANK_STATEMENT` | `CHECK` | `DRIVER_LICENSE` | `HEALTH_INSURANCE_ID` |
| `INVOICE` | `OTHERS` | `PASSPORT` | `PAYSLIP` |
| `RECEIPT` | `RESUME` | `TAX_FORM` | |

### documentCapture.Feature
<!-- id: N/documentCapture.Feature -->

For `options.features` of [`documentToStructure`](#documentcapturedocumenttostructureoptions).
When no features are given, `TEXT_EXTRACTION` and `TABLE_EXTRACTION` are used. Since 2025.2.

| Value | Fills |
|---|---|
| `DOCUMENT_CLASSIFICATION` | `Page.detectedDocumentTypes` |
| `FIELD_EXTRACTION` | `Page.fields` (needs a `documentType`) |
| `TABLE_EXTRACTION` | `Page.tables` |
| `TEXT_EXTRACTION` | `Page.lines` and `Page.words` |

### documentCapture.FieldType
<!-- id: N/documentCapture.FieldType -->

Values found in [`Field.type`](#fieldtype), which appear only when `FIELD_EXTRACTION` was
requested. The source does not state what each value means; the names are all it gives. Since
2025.2.

| Value |
|---|
| `KEY_VALUE` |
| `LINE_ITEM` |
| `LINE_ITEM_FIELD` |
| `LINE_ITEM_GROUP` |
| `UNKNOWN` |

### documentCapture.Language
<!-- id: N/documentCapture.Language -->

For `options.language` of [`documentToStructure`](#documentcapturedocumenttostructureoptions).
`ENG` is the default. The source lists only these codes, without the language names they stand
for. Since 2025.2.

| Value | Value | Value | Value |
|---|---|---|---|
| `ARA` | `CES` | `CHI_SIM` | `DAN` |
| `DEU` | `ELL` | `ENG` | `FIN` |
| `FRA` | `HIN` | `HUN` | `ITA` |
| `JPN` | `KOR` | `NLD` | `NOR` |
| `OTHERS` | `POL` | `POR` | `RON` |
| `RUS` | `SLK` | `SWE` | `TUR` |

## Examples

A scheduled script that reads a scanned supplier invoice from the File Cabinet and collects its
labelled fields and line-item table. Low-confidence values are set aside for someone to check.
The file path is invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/documentCapture', 'N/log'], (documentCapture, log) => {
  const MIN_CONFIDENCE = 0.85;

  const execute = () => {
    if (documentCapture.getRemainingConcurrency() < 1) {
      log.audit({ title: 'Capture busy', details: 'No free slots; will retry next run' });
      return;
    }

    const doc = documentCapture.documentToStructure({
      file: '/AP Inbox/acme-parts-0458.pdf',
      documentType: documentCapture.DocumentType.INVOICE,
      features: [
        documentCapture.Feature.FIELD_EXTRACTION,
        documentCapture.Feature.TABLE_EXTRACTION
      ],
      language: documentCapture.Language.ENG
    });

    const accepted = {};
    const toReview = [];
    doc.pages.forEach((page) => {
      page.fields
        .filter((f) => f.type === documentCapture.FieldType.KEY_VALUE)
        .forEach((f) => {
          if (f.value.confidence >= MIN_CONFIDENCE) {
            accepted[f.label.name] = f.value.text;
          } else {
            toReview.push(`${f.label.name}=${f.value.text} (${f.value.confidence})`);
          }
        });

      page.tables.forEach((table, t) => {
        const lines = table.bodyRows.map((row) => row.cells.map((c) => c.text).join(' | '));
        log.debug({ title: `Page table ${t + 1} (${table.columnCount} cols)`, details: lines.join('\n') });
      });
    });

    log.audit({ title: 'Invoice fields', details: JSON.stringify(accepted) });
    if (toReview.length > 0) {
      log.audit({ title: 'Needs review', details: toReview.join('; ') });
    }
  };

  return { execute };
});
```

Pairing `documentToText` with [N/llm](N_llm.md): a RESTlet receives the internal ID of a signed
contract PDF and asks the model for its renewal and termination terms.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/documentCapture', 'N/llm'], (documentCapture, llm) => {
  const post = (body) => {
    const contractText = documentCapture.documentToText({
      file: Number(body.fileId),
      timeout: 60000
    });
    const reply = llm.generateText({
      preamble: 'You review supplier contracts for a procurement team. Be brief and quote clause numbers.',
      prompt: `List the renewal and termination terms in this contract:\n\n${contractText}`,
      modelParameters: { temperature: 0.1 }
    });
    return { terms: reply.text };
  };

  return { post };
});
```

Reading the saved JSON output of an asynchronous capture task (for a document over 5 pages) and
reporting which document type each page most likely is:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/documentCapture', 'N/file'], (documentCapture, file) => {
  const summarizeTaskOutput = (resultFileId) => {
    const doc = documentCapture.parseResult({ file: file.load({ id: resultFileId }) });
    return doc.pages.map((page, i) => {
      const best = (page.detectedDocumentTypes || [])
        .slice()
        .sort((a, b) => b.confidence - a.confidence)[0];
      return { page: i + 1, type: best ? best.documentType : 'unclassified', chars: page.getText().length };
    });
  };

  return { summarizeTaskOutput };
});
```
