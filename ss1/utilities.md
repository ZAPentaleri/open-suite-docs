# Utility functions: dates, currency, XML, files, errors, email, configuration and workflow (SuiteScript 1.0)

> **Legacy API.** This page exists so that tools and engineers can read, convert and minimally repair
> existing SuiteScript 1.0 scripts. It is not a guide for writing new 1.0 code; new work belongs in
> SuiteScript 2.1. Each section names the 2.x replacement where the mapping gives one.

1.0 functions take **positional** arguments, so the parameter tables list them in the order the call
must use. A `**Since:**` line appears where the source states the release that introduced a
function. Where a section says the source does not state availability, the source gives no list of
script types for that function.

Related reading:

- Concepts: [SuiteScript 1.0 API overview](../topics/ss1_api.md),
  [1.0 scheduled scripts](../topics/ss1_api.scheduled_scripts.md),
  [1.0 Suitelets](../topics/ss1_api.suitelets.md),
  [1.0 workflow action scripts](../topics/ss1_api.workflow_action.md),
  [1.0 mass update scripts](../topics/ss1_api.mass_update.md),
  [1.0 user events](../topics/ss1_api.user_events.md).
- Objects returned here (`nlobjFile`, `nlobjError`, `nlobjConfiguration`, `nlobjLogin`,
  `nlobjJobManager`): [1.0 service objects](objects-services.md).
- Converting: [SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).

## Dates and currency

### nlapiAddDays(d, days)
<!-- id: ss1:nlapiAddDays -->

Returns a date moved by a number of days.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `d` | Date | Yes | |
| `days` | number | Yes | |

- `days` may be negative to move backwards.

**Returns:** `Date`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** no equivalent function. The mapping points to the native JavaScript `Date.setDate` method
instead.

### nlapiAddMonths(d, months)
<!-- id: ss1:nlapiAddMonths -->

Returns a date moved by a number of months.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `d` | Date | Yes | |
| `months` | number | Yes | |

- `months` may be negative to move backwards.

**Returns:** `Date`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** no equivalent function. The mapping points to the native JavaScript `Date.setMonth` method
instead.

### nlapiDateToString(d, format)
<!-- id: ss1:nlapiDateToString -->

Turns a `Date` into a string laid out according to the current user's date and time preferences.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `d` | Date | Yes | |
| `format` | string | No | |

- `format`: `date`, `timeofday`, `datetime` or `datetimetz`; `date` is used when it is omitted. It
  only chooses which parts appear in the string; it has no effect on the time zone.

**Returns:** `string`, formatted by the user's preferences.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

- The time zone depends on where the code runs: in client scripts it is the user's computer clock,
  in server scripts it is Pacific time with daylight saving applied.

**2.x:** `format.format(options)` in [N/format](../modules/N_format.md).

### nlapiStringToDate(str, format)
<!-- id: ss1:nlapiStringToDate -->

Parses a date string written in the user's preferred format back into a `Date`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `str` | string | Yes | |
| `format` | string | No | |

- `format`: `datetime` or `datetimetz`. Without it, the string is read using the date format. The
  argument also decides whether the input may contain seconds: not when it is omitted, not with
  `datetime`, and seconds are required with `datetimetz`. It does not affect the time zone.

**Returns:** `Date`. If the month or day in the string has a leading zero, the result is `NaN`
rather than a valid date.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

- Time zone: the user's computer clock on the client; Pacific time with daylight saving on the
  server.

**2.x:** `format.parse(options)` in [N/format](../modules/N_format.md).

### nlapiExchangeRate(sourceCurrency, targetCurrency, effectiveDate)
<!-- id: ss1:nlapiExchangeRate -->

Looks up the exchange rate between two currencies. If several rates exist on the effective date, the
latest one on that date is used.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sourceCurrency` | string \| number | Yes | |
| `targetCurrency` | string \| number | Yes | |
| `effectiveDate` | string \| number | No | |

- `sourceCurrency`, `targetCurrency`: a currency internal ID or a currency symbol.
- `effectiveDate`: defaults to the current date.

**Returns:** `number`, the rate as a decimal at the precision the UI shows.

**Errors:**

- `SSS_INVALID_CURRENCY_ID`: a currency argument does not identify a valid currency.

**Governance:** 10 units.

**Availability:** all script types.

**Since:** 2009.1

- The Currency Exchange Rate record itself cannot be scripted; this function is how scripts read
  rates.

**2.x:** `currency.exchangeRate(options)` in [N/currency](../modules/N_currency.md).

### nlapiFormatCurrency(str)
<!-- id: ss1:nlapiFormatCurrency -->

Formats a number string the way a currency field displays it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `str` | string | Yes | |

**Returns:** `string`, a currency field value.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** `format.format(options)` in [N/format](../modules/N_format.md).

## Encoding and XML

### nlapiEncrypt(s, algorithm, key)
<!-- id: ss1:nlapiEncrypt -->

Transforms a string with one of a few algorithms, some of which are hashing or plain encoding rather
than encryption.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `s` | string | Yes | |
| `algorithm` | string | Yes | |
| `key` | string | No | |

- `algorithm`: `sha1`, `aes`, `base64` or `xor`. `sha1` is deprecated. `base64` and `xor` are not
  encryption at all; treat their output as merely encoded.
- `key`: used only with `aes`; a hexadecimal key of 128, 192 or 256 bits. Legacy scripts that embed
  this key in source should be repaired to read it from a protected setting (general guidance, not a
  source statement).

**Returns:** `string`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** approximate only; the mapping points to the [N/crypto](../modules/N_crypto.md) and
[N/encode](../modules/N_encode.md) modules together, so the rewrite depends on the algorithm used.

### nlapiEscapeXML(text)
<!-- id: ss1:nlapiEscapeXML -->

Makes a string safe to place inside XML by escaping angle brackets, quotation marks and ampersands.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `text` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** `xml.escape(options)` in [N/xml](../modules/N_xml.md).

### nlapiStringToXML(text)
<!-- id: ss1:nlapiStringToXML -->

Parses a string into an XML document, which you then query with
[`nlapiSelectNode`](#nlapiselectnodenode-xpath) and its siblings.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `text` | string | Yes | |

**Returns:** `Document`, a W3C DOM document.

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** `Parser.fromString(options)` (`N/xml.Parser#fromString`) in [N/xml](../modules/N_xml.md).

### nlapiValidateXML(xmlDocument, schemaDocument, schemaFolderId)
<!-- id: ss1:nlapiValidateXML -->

Checks an XML document against a schema; XSD is the only schema language supported. Both documents
must have been produced by [`nlapiStringToXML`](#nlapistringtoxmltext).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `xmlDocument` | document | Yes | |
| `schemaDocument` | document | Yes | |
| `schemaFolderId` | string | No | |

- `xmlDocument`, `schemaDocument`: typed `document` (lower case) here, while `nlapiXMLToString` takes
  a `Document`; the source does not explain the difference. Either way, both must come from
  `nlapiStringToXML`.
- `schemaFolderId`: the File Cabinet folder ID that holds child schemas. Required when the schema
  imports or includes other schemas by file path; ignored when it is not needed.

**Returns:** `void`. A valid document returns `undefined`; warnings on their own do not make
validation fail.

**Errors:**

- `SSS_XML_DOES_NOT_CONFORM_TO_SCHEMA`: the document does not match the schema. The thrown
  `nlobjError` carries the first 10 validation errors. They are not logged automatically, so catch
  the error and log its details yourself.
- `SSS_XML_SCHEMA_MISSING_DEPENDECY_FOLDER_ID` [sic]: an argument value is invalid; going by the code's
  name, the folder ID for dependent schemas (`schemaFolderId`) is missing.

> [!SOURCE-ERROR] The second error code is spelled "DEPENDECY" in the source, which looks like a typo
> for "DEPENDENCY". The spec keeps the source spelling; if code compares against this string, check
> the code actually thrown in the account rather than assuming either spelling.
<!-- source-error: ss1:nlapiValidateXML | - | error_code_spells_dependecy -->

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2014.1

**2.x:** `xml.validate(options)` in [N/xml](../modules/N_xml.md).

### nlapiXMLToString(xml)
<!-- id: ss1:nlapiXMLToString -->

Serializes an XML document back into a string, typically so that it can be stored in a custom
field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `xml` | Document | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** `Parser.toString(options)` (`N/xml.Parser#toString`) in [N/xml](../modules/N_xml.md).

### nlapiXMLToPDF(xmlstring)
<!-- id: ss1:nlapiXMLToPDF -->

Renders an XML document that describes a page layout into a PDF, using the built-in BFO report
generator (nothing has to be installed).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `xmlstring` | string | Yes | |

- The XML must carry the XML declaration on its first line, have `pdf` as its root element with
  `head` and `body` inside it, close every element, and quote every attribute value.
- To print Cyrillic text, the `head` must link a NetSuite-provided font.

**Returns:** `nlobjFile` holding the PDF. The generated file may be at most 10 MB.

**Errors:**

- `ERROR_PARSING_XML`: the input is not well-formed XML.

**Governance:** 10 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2009.1

**2.x:** split between `render.xmlToPdf(options)` and `TemplateRenderer.renderAsPdf()`
(`N/render.TemplateRenderer#renderAsPdf`) in [N/render](../modules/N_render.md).

### nlapiSelectNode(node, xpath)
<!-- id: ss1:nlapiSelectNode -->

Returns the first node that an XPath expression selects, evaluated relative to `node`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `node` | node | Yes | |
| `xpath` | string | Yes | |

**Returns:** `Node`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** `XPath.select(options)` (`N/xml.XPath#select`) in [N/xml](../modules/N_xml.md).

### nlapiSelectNodes(node, xpath)
<!-- id: ss1:nlapiSelectNodes -->

Returns every node that an XPath expression selects, evaluated relative to `node`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `node` | node | Yes | |
| `xpath` | string | Yes | |

**Returns:** `Node[]`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** `XPath.select(options)` (`N/xml.XPath#select`) in [N/xml](../modules/N_xml.md).

### nlapiSelectValue(node, xpath)
<!-- id: ss1:nlapiSelectValue -->

Returns the text value of the node that an XPath expression selects.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `node` | node | Yes | |
| `xpath` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** approximate: select with `XPath.select(options)`, then read `Node.textContent`
(`N/xml.Node#textContent`) in [N/xml](../modules/N_xml.md).

### nlapiSelectValues(node, path)
<!-- id: ss1:nlapiSelectValues -->

Returns the text values of all nodes that an XPath expression selects. (The second parameter is
named `path` here, not `xpath` as on its siblings; it is positional either way.)

| Parameter | Type | Required | Default |
|---|---|---|---|
| `node` | node | Yes | |
| `path` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**2.x:** approximate: `XPath.select(options)` plus `Node.textContent` on each result
([N/xml](../modules/N_xml.md)).

## Files

### nlapiCreateFile(name, type, contents)
<!-- id: ss1:nlapiCreateFile -->

Creates a file in memory. Typical uses: an email or fax attachment, content a Suitelet streams back
to the browser, or a file you then store in the File Cabinet with
[`nlapiSubmitFile`](#nlapisubmitfilefile). Nothing is stored until the file is submitted.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `contents` | string | Yes | |

- `type`: one of `AUTOCAD`, `BMPIMAGE`, `CSV`, `EXCEL`, `FLASH`, `GIFIMAGE`, `GZIP`, `HTMLDOC`,
  `ICON`, `JAVASCRIPT`, `JPGIMAGE`, `JSON`, `MESSAGERFC`, `MP3`, `MPEGMOVIE`, `MSPROJECT`, `PDF`,
  `PJPGIMAGE`, `PLAINTEXT`, `PNGIMAGE`, `POSTSCRIPT`, `POWERPOINT`, `QUICKTIME`, `RTF`, `SMS`,
  `STYLESHEET`, `TIFFIMAGE`, `VISIO`, `WORD`, `XMLDOC` or `ZIP` (common choices are `PLAINTEXT`,
  `HTMLDOC` and `XMLDOC`). For files built on the fly, such as ad hoc attachments, only non-binary
  types work unless the contents are base64-encoded.

> [!SOURCE-ERROR] The source's page of file types shows an example in which the type ID is padded
> with spaces. The spec lists the IDs without padding, and the source does not explain the padding;
> pass the ID exactly as listed above and verify in the account if a call fails.
<!-- source-error: ss1:nlapiCreateFile | type | file_types_page_example_pads_type_id_with_spaces -->

- `contents`: binary types such as `PDF` need base64-encoded contents.

**Returns:** `nlobjFile`. The file may be at most 10 MB.

**Governance:** 0 units.

**Availability:** user event, scheduled, portlet, mass update and Suitelet scripts.

**Since:** 2008.1

**2.x:** `file.create(options)` in [N/file](../modules/N_file.md).

### nlapiDeleteFile(id)
<!-- id: ss1:nlapiDeleteFile -->

Deletes a file from the File Cabinet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | number | Yes | |

**Returns:** `number`, the internal ID of the deleted file.

**Governance:** 20 units.

**Availability:** user event, scheduled, portlet and Suitelet scripts.

**Since:** 2009.1

**2.x:** `file.delete(options)` in [N/file](../modules/N_file.md).

### nlapiLoadFile(id)
<!-- id: ss1:nlapiLoadFile -->

Loads a file from the File Cabinet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | string \| number | Yes | |

- `id`: the file's internal ID or its path relative to the File Cabinet root.

**Returns:** `nlobjFile`. For binary files, the contents come back base64-encoded.

**Governance:** 10 units.

**Availability:** server scripts.

- The running user needs permission on the folder that holds the file.
- Files hidden inside bundles cannot be loaded.
- The size limit is the File Cabinet's own file size limit.

**2.x:** `file.load(options)` in [N/file](../modules/N_file.md).

### nlapiSubmitFile(file)
<!-- id: ss1:nlapiSubmitFile -->

Saves a file object to the File Cabinet. A file with the same name in the same folder is
overwritten.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `file` | nlobjFile | Yes | |

**Returns:** `number`, the file's internal ID.

**Governance:** 20 units.

**Availability:** user event, scheduled, portlet and Suitelet scripts.

**Since:** 2009.1

- The size limit is the File Cabinet's file size limit.

**2.x:** `File.save()` (`N/file.File#save`) in [N/file](../modules/N_file.md); the mapping calls it a
rename, and the call moves onto the file object itself.

## Errors

### nlapiCreateError(code, details, suppressNotification)
<!-- id: ss1:nlapiCreateError -->

Builds an error object, stack trace included, for the script to `throw`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `code` | string | Yes | |
| `details` | string | Yes | |
| `suppressNotification` | boolean | No | `false` |

- `code`: an error code you define yourself.
- `suppressNotification`: `true` stops the error notification email from being sent.

**Returns:** `nlobjError`

**Governance:** 0 units.

**Availability:** user event, scheduled, portlet and Suitelet scripts.

**2.x:** `error.create(options)` in [N/error](../modules/N_error.md).

## Email, fax and sign-on

### nlapiSendCampaignEmail(campaigneventid, recipientid)
<!-- id: ss1:nlapiSendCampaignEmail -->

Sends one campaign email from a Lead Nurturing campaign (the `campaigndrip` kind, not
`campaignemail`) to one recipient. Messages sent this way count against the account's campaign email
volume provisioning.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `campaigneventid` | number | Yes | |
| `recipientid` | number | Yes | |

- `recipientid`: the recipient must have an email address.

**Returns:** `number`, the campaign response ID, or `-1` when sending fails.

**Governance:** 10 units.

**Availability:** user event, scheduled, Suitelet, mass update and workflow action scripts.

**Since:** 2010.1

**2.x:** `email.sendCampaignEvent(options)` in [N/email](../modules/N_email.md).

### nlapiSendEmail(author, recipient, subject, body, cc, bcc, records, attachments, notifySenderOnBounce, internalOnly, replyTo)
<!-- id: ss1:nlapiSendEmail -->

Sends an email and, optionally, records it against transactions, entities, activities or custom
records. With eleven positional arguments, legacy calls often contain runs of `null`; count the
slots carefully when converting.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `author` | number | Yes | |
| `recipient` | string \| number | Yes | |
| `subject` | string | Yes | |
| `body` | string \| nlobjFile | Yes | |
| `cc` | string \| string[] | No | |
| `bcc` | string \| string[] | No | |
| `records` | Object | No | |
| `attachments` | nlobjFile \| nlobjFile[] | No | |
| `notifySenderOnBounce` | boolean | No | |
| `internalOnly` | boolean | No | |
| `replyTo` | string | No | |

- `author`: the internal ID of an employee.
- `recipient`: one email address, several addresses separated by commas, or an entity's internal
  ID. With an entity ID, the message is attached to that entity's record.
- `subject`: a blank, `null` or empty subject throws a JavaScript exception.
- `body`: a blank, `null` or empty body throws a JavaScript exception.

> [!SOURCE-ERROR] The source's type entry for `body` is garbled (it appears to mention an
> `nlobjFile` array). The spec records `string | nlobjFile`; treat any other form as unverified.
<!-- source-error: ss1:nlapiSendEmail | body | body_type_cell_garbled_nlobjfile_array -->

- `records`: an object whose keys are `transaction`, `activity`, `entity`, `record` and
  `recordtype`. For a custom record, give both `record` and `recordtype`.
- `attachments`: not supported in client scripts.
- `notifySenderOnBounce`: `true` sends bounce notices to the sender. In that mode the recipients, CC
  and BCC addresses together may not exceed 10, and the message goes out through a transactional
  mail server with a higher delivery rate.
- `internalOnly`: `true` keeps the message out of the Customer Center.
- `replyTo`: an email address, which may also be an Email Capture plug-in address.

**Returns:** `void` (the call returns `undefined`).

**Errors:**

- `SSS_AUTHOR_MUST_BE_EMPLOYEE`: `author` is not an employee.
- `SSS_AUTHOR_REQD`: `author` is missing.
- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: the message or an attachment is over the size limit.
- `SSS_INVALID_BCC_EMAIL`: a BCC address is invalid.
- `SSS_INVALID_CC_EMAIL`: a CC address is invalid.
- `SSS_INVALID_RECIPIENT_ID`: the recipient ID is invalid.
- `SSS_INVALID_REPLYTO_EMAIL`: the reply-to address is invalid.
- `SSS_INVALID_TO_EMAIL`: a recipient address is invalid.
- `SSS_MAXIMUM_NUMBER_RECIPIENTS_EXCEEDED`: there are too many recipients.
- `SSS_MISSING_REQD_ARG` [sic]: a required argument is missing.
- `SSS_RECIPIENT_REQD`: `recipient` is missing.

> [!SOURCE-ERROR] Two error codes on this function are unreliable in the source. The size error is
> written in mixed case there; the spec normalizes it to `SSS_FILE_CONTENT_SIZE_EXCEEDED`. The
> missing-argument code is given as `SSS_MISSING_REQD_ARG`, while other functions use
> `SSS_MISSING_REQD_ARGUMENT`. The source does not say which form is actually thrown; when code
> matches on these codes, accept both spellings or verify in the account.
<!-- source-error: ss1:nlapiSendEmail | - | error_code_written_mixed_case -->
<!-- source-error: ss1:nlapiSendEmail | - | missing_arg_code_differs_from_other_pages -->

**Governance:** 10 units; 20 units when `notifySenderOnBounce` is `true`.

**Availability:** client and server scripts.

- The whole message, attachments included, may be at most 15 MB, and each attachment at most 10 MB.
- To show XML in the body as plain text, wrap it in a `<pre>` element.
- Only the first recipient is shown on the record's Communication tab.
- For template-based messages, build the content with `nlapiCreateEmailMerger`.

**2.x:** split into `email.send(options)` and `email.sendBulk(options)` in
[N/email](../modules/N_email.md). The mapping carries the first eight arguments across; it lists
none for `notifySenderOnBounce`, `internalOnly` or `replyTo`.

### nlapiSendFax(author, recipient, subject, body, records, attachments)
<!-- id: ss1:nlapiSendFax -->

Sends a fax using the account's fax settings.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `author` | number | Yes | |
| `recipient` | string | Yes | |
| `subject` | string | Yes | |
| `body` | string | No | |
| `records` | Object | No | |
| `attachments` | nlobjFile | No | |

- `recipient`: when it is an internal ID, the fax is also saved in NetSuite.
- `records`: keys `transaction`, `activity`, `entity`, `record` and `recordtype`. When set, the fax
  is saved against those records.
- `attachments`: a single `nlobjFile` or an array of them; not supported in client scripts.

> [!SOURCE-ERROR] The source's type entry for `attachments` lists only `nlobjFile`, although its
> description also accepts an array of files. The spec keeps the narrower type; the array form is
> described but should be verified before relying on it.
<!-- source-error: ss1:nlapiSendFax | attachments | type_cell_omits_array_form -->

**Returns:** `void` (the call returns `undefined`).

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

**Since:** 2008.1

**2.x:** no equivalent; the source names no alternative.

### nlapiOutboundSSO(id)
<!-- id: ss1:nlapiOutboundSSO -->

Generates the URL for an outbound single sign-on connection. The URL carries an OAuth token plus the
integration variables configured for the connection.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | string | Yes | |

- `id`: the SuiteSignOn record's custom script ID or its system-generated ID; either way it starts
  with `customsso`.

**Returns:** `string`, the sign-on URL.

**Errors:**

- `SSS_SUITESIGNON_NOT_CONFIGURED`: the source lists this code without saying when it is raised.
- `SSS_INVALID_SUITESIGNON`: `id` does not identify a valid SuiteSignOn record.

**Governance:** 20 units.

**Availability:** portlet, user event and Suitelet scripts. Both of these features must be enabled:
`"SuiteSignOn"` and `"web services"` (names exactly as the source gives them).

> [!SOURCE-ERROR] The second feature name is written all in lower case, "web services", which does
> not match any feature label as the product shows it; the [features catalog](../gen/catalogs/features.md)
> has labels such as `SOAP Web Services` and `REST Web Services` instead. The source does not say
> which feature is meant, so confirm in the account which web services feature has to be on.
<!-- source-error: ss1:nlapiOutboundSSO | - | web_services_feature_name_lowercase_not_ui_label -->

**Since:** 2009.2

**2.x:** no equivalent; the mapping marks the feature as retired.

## Configuration

### nlapiLoadConfiguration(type)
<!-- id: ss1:nlapiLoadConfiguration -->

Loads one of the account's setup pages as an `nlobjConfiguration`, which you change with
`nlobjConfiguration.setFieldValue` and save with
[`nlapiSubmitConfiguration`](#nlapisubmitconfigurationname).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

- `type`: `companyinformation`, `companypreferences`, `userpreferences`, `accountingpreferences`,
  `accountingperiods`, `taxperiods` or `companyfeatures`. Use `companyfeatures` to find out which
  features are enabled.

**Returns:** `nlobjConfiguration`

**Governance:** 10 units.

**Availability:** scheduled, user event and Suitelet scripts.

**Since:** 2009.2

- On company information, addresses are ordinary fields; do not use the subrecord functions on them.

**2.x:** `config.load(options)` in [N/config](../modules/N_config.md), which returns a record object.

### nlapiSubmitConfiguration(name)
<!-- id: ss1:nlapiSubmitConfiguration -->

Saves the changes made to a configuration object loaded with `nlapiLoadConfiguration`. The pages
listed as supported are company information, general preferences, enable features, accounting
preferences, accounting periods and tax periods.

> [!SOURCE-ERROR] This list of supported pages does not match the types `nlapiLoadConfiguration`
> accepts: it includes "enable features" and lacks user preferences. The source does not say which
> list is complete; test the save for any page you rely on.
<!-- source-error: ss1:nlapiSubmitConfiguration | - | supported_page_list_differs_from_load_list -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | nlobjConfiguration | Yes | |

- `name`: despite its name, this is the `nlobjConfiguration` object to save.

**Returns:** `void`

**Governance:** 20 units.

**Availability:** scheduled and Suitelet scripts.

**Since:** 2009.2

**2.x:** `Record.save()` (`N/record.Record#save`) on the record returned by `config.load`.

## Workflow

### nlapiInitiateWorkflow(recordtype, id, workflowid, initialvalues)
<!-- id: ss1:nlapiInitiateWorkflow -->

Starts a workflow on a record; it is the scripted counterpart of the Initiate Workflow action.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `recordtype` | string | Yes | |
| `id` | number | Yes | |
| `workflowid` | number \| string | Yes | |
| `initialvalues` | Object | No | |

- `recordtype`: a record type such as `customer`, `salesorder` or `lead`.
- `workflowid`: the workflow's internal ID or script ID.
- `initialvalues`: name/value pairs used as defaults when the workflow initializes.

**Returns:** `number`, the ID of the new workflow instance.

**Errors:**

- A user error is thrown when the record is invalid or its type is not supported for workflows. The
  source does not state the error code.

**Governance:** 20 units.

**Availability:** user event, scheduled, portlet, Suitelet, mass update and workflow action scripts.

**Since:** 2010.1

- For a queued, asynchronous start see
  [`nlapiInitiateWorkflowAsync`](#nlapiinitiateworkflowasyncrecordtype-id-workflowid-initialvalues).

**2.x:** `workflow.initiate(options)` in [N/workflow](../modules/N_workflow.md).

### nlapiInitiateWorkflowAsync(recordType, id, workflowId, initialValues)
<!-- id: ss1:nlapiInitiateWorkflowAsync -->

Queues a job that starts a workflow on a record, instead of starting it in the current execution.
If an identical instance (same record type, record ID and workflow) is already running or queued, no
new job is queued.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `recordType` | string | Yes | |
| `id` | number | Yes | |
| `workflowId` | number \| string | Yes | |
| `initialValues` | Object | No | |

- `workflowId`: the workflow's internal ID or script ID.
- `initialValues`: name/value pairs used as defaults when the workflow initializes.

**Returns:** `string`, the job status: `QUEUED`, `INQUEUE` or `INPROGRESS`.

**Errors:**

- An error is thrown when the record is invalid or not supported for workflows. The source does not
  state the error code.

> [!SOURCE-ERROR] The source leaves this function out of its own API list, and its example of the
> thrown error is empty. The spec still records the function and its error case from the function's
> own page; verify the exact error details in the account before matching on them.
<!-- source-error: ss1:nlapiInitiateWorkflowAsync | - | missing_from_page_api_list_and_throws_example_empty -->

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2014.2

**2.x:** a pattern change: build and submit a workflow trigger task (`N/task.WorkflowTriggerTask`)
in [N/task](../modules/N_task.md).

### nlapiTriggerWorkflow(recordtype, id, workflowid, actionid, workflowstateid)
<!-- id: ss1:nlapiTriggerWorkflow -->

Makes a running workflow instance evaluate the actions and transitions of its current state. With
`actionid`, it behaves as if a user had clicked that workflow button.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `recordtype` | string | Yes | |
| `id` | number | Yes | |
| `workflowid` | number \| string | Yes | |
| `actionid` | string \| number | No | |
| `workflowstateid` | string \| number | No | |

- `workflowid`: internal ID or script ID.
- `actionid`: the internal ID of a workflow button; the call acts as if that button were pressed.
  Required whenever `workflowstateid` is given.
- `workflowstateid` (added in 2015.2): internal ID or script ID of a state. Use it when the same
  action script ID appears in more than one state; without it, the action with the lowest internal
  ID is chosen.

> [!SOURCE-ERROR] The source is inconsistent about the last parameter's name: its index calls it
> `stateid` [sic] while the member list and parameter list use `workflowstateid`, and the heading and
> the parameter list name the parameters differently as well. The 2.x mapping also labels the fifth
> slot `stateid`. Because 1.0 arguments are positional, the name does not change how a call behaves;
> this page follows `workflowstateid`.
<!-- source-error: ss1:nlapiTriggerWorkflow | - | index_names_last_parameter_stateid -->
<!-- source-error: ss1:nlapiTriggerWorkflow | - | index_names_last_parameter_stateid -->
<!-- source-error: ss1:nlapiTriggerWorkflow | - | heading_and_parameter_list_names_differ -->

**Returns:** `number`, the workflow instance ID.

**Governance:** 20 units.

**Availability:** user event, scheduled, portlet, Suitelet, mass update and workflow action scripts.

**Since:** 2010.1

**2.x:** `workflow.trigger(options)` in [N/workflow](../modules/N_workflow.md).

## Account and jobs

### nlapiGetLogin()
<!-- id: ss1:nlapiGetLogin -->

Returns the current user's login credentials as an `nlobjLogin`, so that a custom UI (for example a
Suitelet page for mobile users, or an SSP page in a web store) can let users manage their own
credentials.

**Returns:** `nlobjLogin`

**Governance:** 10 units.

**Availability:** user event, portlet, Suitelet, RESTlet and SSP application scripts.

**Since:** 2012.2

**2.x:** split into `auth.changeEmail(options)` and `auth.changePassword(options)` in
[N/auth](../modules/N_auth.md); call the one for the change you need.

### nlapiGetJobManager(jobType)
<!-- id: ss1:nlapiGetJobManager -->

Returns a job manager for the given job type. The only job type is merging duplicate records: build
the job, then hand it over with `nlobjJobManager.submit`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `jobType` | string | Yes | |

- `jobType`: `DUPLICATERECORDS`.

**Returns:** `nlobjJobManager`

**Governance:** 0 units.

**Availability:** server scripts; not client scripts.

**Since:** 2013.1

- A merge-duplicates job handles at most 200 records.
- Only entity records can be merged.
- Once merged, records no longer show up as duplicates in
  [`nlapiSearchDuplicate`](search-scheduling-context.md#nlapisearchduplicatetype-fields-id).

**2.x:** `task.create(options)` in [N/task](../modules/N_task.md).

### nlapiCheckPasswordField()
<!-- id: ss1:nlapiCheckPasswordField -->

Only the name of this function is known. Its signature has not been verified against the source, so
its parameters, return value and behaviour are not documented here; read them from the legacy call
itself.

**Governance:** not documented; assumed to be free (0 units) by project convention.

**2.x:** the mapping names `crypto.checkPasswordField(options)` in [N/crypto](../modules/N_crypto.md)
as the counterpart. For conversion it lists six argument slots, in order `type`, `id`, `value`,
`field`, `sublist` and `line`; since the 1.0 signature itself is unverified, check those against the
legacy call before relying on them.

## Examples

These 1.0 snippets are plain functions, written to show how legacy code is shaped so it can be read
and converted.

Working out a follow-up date 30 days after a date the user typed, and writing it back in the user's
format. Note that `nlapiStringToDate` gives `NaN` for day or month values with a leading zero:

```js
function followUpFrom(typedDate) {
  var start = nlapiStringToDate(typedDate);
  if (isNaN(start)) {
    return null;
  }
  var followUp = nlapiAddDays(start, 30);
  return nlapiDateToString(followUp, 'date');
}
```

Reading tracking numbers out of a carrier's XML feed:

```js
function trackingNumbersFrom(feedText) {
  var doc = nlapiStringToXML(feedText);
  var parcels = nlapiSelectNodes(doc, '//parcel');
  var numbers = [];
  for (var i = 0; i < parcels.length; i++) {
    numbers.push(nlapiSelectValue(parcels[i], 'tracking'));
  }
  return numbers;
}
```

A packing slip PDF built from escaped line descriptions, filed in the File Cabinet and emailed to the
customer with the fulfillment recorded on the message:

```js
function mailPackingSlip(senderId, customerId, fulfillmentId, lines, folderId) {
  var rows = '';
  for (var i = 0; i < lines.length; i++) {
    rows += '<tr><td>' + nlapiEscapeXML(lines[i].description) + '</td><td>' + lines[i].qty + '</td></tr>';
  }
  var xml = '<?xml version="1.0"?>\n<pdf><head></head><body><table>' + rows + '</table></body></pdf>';
  var slip = nlapiXMLToPDF(xml);
  slip.setFolder(folderId);
  var fileId = nlapiSubmitFile(slip);
  nlapiSendEmail(senderId, customerId, 'Your packing slip', 'The packing slip for your shipment is attached.',
    null, null, { transaction: fulfillmentId }, slip);
  return fileId;
}
```

Refusing to continue without a required value, with the notification email suppressed:

```js
function requireCostCenter(costCenterId) {
  if (!costCenterId) {
    throw nlapiCreateError('CUSTOM_COST_CENTER_MISSING', 'Set a cost center before approving this expense report.', true);
  }
  return costCenterId;
}
```
