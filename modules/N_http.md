# N/http
<!-- id: N/http -->

`N/http` makes plain-HTTP calls from server or client scripts, and it also defines the request and
response objects that a Suitelet or RESTlet handles. It supports every HTTP content type. It does
not do HTTPS at all; when you need HTTPS, use [`N/https`](N_https.md), which has the same call
functions plus secure-string and in-account call helpers.

- **Availability:** client and server scripts at module level; the request and response objects
  are server only (see each section).
- **Load it as:** `define(['N/http'], (http) => { ... })`
- In client scripts, cross-domain requests work because NetSuite proxies them.
- Related: [N/https](N_https.md), [N/url](N_url.md), [N/redirect](N_redirect.md),
  [blocked headers catalog](../gen/catalogs/http_blocked_headers.md).

**Headers**

- Header names are not case-sensitive.
- Custom header names must not contain underscores.
- Some headers are blocked (see the [blocked headers catalog](../gen/catalogs/http_blocked_headers.md)).
  Values you give for the general blocked headers are simply discarded. A Suitelet response that
  sets one of the Suitelet-response blocked headers fails with `SSS_INVALID_HEADER`.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`http.get(options)`](#httpgetoptions) | function | HTTP GET |
| [`http.get.promise(options)`](#httpgetpromiseoptions) | function | Promise form of `get` |
| [`http.post(options)`](#httppostoptions) | function | HTTP POST |
| [`http.post.promise(options)`](#httppostpromiseoptions) | function | Promise form of `post` |
| [`http.put(options)`](#httpputoptions) | function | HTTP PUT |
| [`http.put.promise(options)`](#httpputpromiseoptions) | function | Promise form of `put` |
| [`http.delete(options)`](#httpdeleteoptions) | function | HTTP DELETE |
| [`http.delete.promise(options)`](#httpdeletepromiseoptions) | function | Promise form of `delete` |
| [`http.request(options)`](#httprequestoptions) | function | HTTP call with any method from `http.Method` |
| [`http.request.promise(options)`](#httprequestpromiseoptions) | function | Promise form of `request` |
| [`ClientResponse`](#clientresponse-object) | object | Response to an outbound call |
| [`ServerRequest`](#serverrequest-object) | object | Request received by a Suitelet or RESTlet |
| [`ServerResponse`](#serverresponse-object) | object | Response a Suitelet or RESTlet sends back |
| [`http.CacheDuration`](#httpcacheduration) | enum | CDN cache lifetimes |
| [`http.Method`](#httpmethod) | enum | HTTP methods |
| [`http.RedirectType`](#httpredirecttype) | enum | Targets for `sendRedirect` |

## Functions

All five call functions share these rules:

- They are not available in client contexts where the user is not authenticated.
- Called from a client script they return a `ClientResponse`; called from a server script (a user
  event script, for example), a `ServerResponse`.
- The host name must be at most 63 characters, use only letters, digits and hyphens, start with a
  letter and end with a letter or digit. An untrusted certificate or an invalid domain name raises
  `SSS_INVALID_HOST_CERT`.
- The URL must be fully qualified, or `SSS_INVALID_URL` is raised.

### http.get(options)
<!-- id: N/http.get -->

Sends a GET request.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `http.ClientResponse | http.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: the certificate is not trusted or the domain in `options.url` is
  invalid.
- `SSS_INVALID_URL`: `options.url` is not fully qualified.
- `SSS_MISSING_REQD_ARGUMENT`: `options.url` is missing.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- The source does not state connect or send time limits for `get` (the other call functions state
  5 and 45 seconds).

### http.get.promise(options)
<!-- id: N/http.get.promise -->

Promise form of [`http.get`](#httpgetoptions): same parameters and errors, but it returns a
Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`http.get`](#httpgetoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### http.post(options)
<!-- id: N/http.post -->

Sends a POST request with a body.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | Yes | |
| `options.headers` | Object | No | |

- `options.body`: multipart `form-data` file uploads are not supported.

**Returns:** `http.ClientResponse | http.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: untrusted certificate or invalid domain in `options.url`.
- `SSS_INVALID_URL`: `options.url` is not fully qualified.
- `SSS_MISSING_REQD_ARGUMENT`: `options.body` or `options.url` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script is calling itself recursively over HTTP.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- Connecting may take at most 5 seconds and sending at most 45 seconds.

### http.post.promise(options)
<!-- id: N/http.post.promise -->

Promise form of [`http.post`](#httppostoptions), with the same parameters and errors. A Suitelet
reached this way waits for its own pending promises before it finishes.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | Yes | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`http.post`](#httppostoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### http.put(options)
<!-- id: N/http.put -->

Sends a PUT request with a body.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | Yes | |
| `options.headers` | Object | No | |

**Returns:** `http.ClientResponse | http.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: untrusted certificate or invalid domain in `options.url`.
- `SSS_INVALID_URL`: `options.url` is not fully qualified.
- `SSS_MISSING_REQD_ARGUMENT`: `options.body` or `options.url` is missing.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- 5-second connect limit, 45-second send limit.

### http.put.promise(options)
<!-- id: N/http.put.promise -->

Promise form of [`http.put`](#httpputoptions); same parameters and errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | Yes | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`http.put`](#httpputoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### http.delete(options)
<!-- id: N/http.delete -->

Sends a DELETE request. There is no body option.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `http.ClientResponse | http.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: untrusted certificate or invalid domain in `options.url`.
- `SSS_INVALID_URL`: `options.url` is not fully qualified.
- `SSS_MISSING_REQD_ARGUMENT`: `options.url` is missing.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- 5-second connect limit, 45-second send limit.

### http.delete.promise(options)
<!-- id: N/http.delete.promise -->

Promise form of [`http.delete`](#httpdeleteoptions); same parameters and errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`http.delete`](#httpdeleteoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### http.request(options)
<!-- id: N/http.request -->

General-purpose HTTP call; you choose the method from [`http.Method`](#httpmethod).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.method` | enum | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |

- `options.method`: a value from [`http.Method`](#httpmethod).
- `options.body`: used only for PUT, POST and PATCH; ignored for other methods.

**Returns:** `http.ClientResponse | http.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: untrusted certificate or invalid domain in `options.url`.
- `SSS_INVALID_URL`: `options.url` is not fully qualified.
- `SSS_MISSING_REQD_ARGUMENT`: `options.method` or `options.url` is missing.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- 5-second connect limit, 45-second send limit.

### http.request.promise(options)
<!-- id: N/http.request.promise -->

Promise form of [`http.request`](#httprequestoptions); same parameters and errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.method` | enum | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`http.request`](#httprequestoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

## Objects

### ClientResponse (object)
<!-- id: N/http.ClientResponse -->

The read-only response you get back when the HTTP call functions are invoked from a client
script. The spec records its availability as server scripts; since 2015.2.

> [!SOURCE-ERROR] The source says this object comes back from calls made in client scripts, yet
> lists server scripts as where it is available. The spec keeps both statements; check in your own
> script type which object the call returns.
<!-- source-error: N/http.ClientResponse | - | says_client_calls_but_lists_server_scripts -->

#### ClientResponse.body
<!-- id: N/http.ClientResponse#body -->

`string`, read-only. The response body. Assigning to it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ClientResponse.code
<!-- id: N/http.ClientResponse#code -->

`number`, read-only. The HTTP status code. Assigning to it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ClientResponse.headers
<!-- id: N/http.ClientResponse#headers -->

`Object.<String, String[]>`, read-only. The response headers. Assigning to it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

- On the client each header name maps to a list of strings; on the server you get only the first
  value as a string, so when a header is repeated, only its first occurrence can be read server
  side.
- Every header shows up two or three times under different letter cases. Read them by their
  lower-case names.

### ServerRequest (object)
<!-- id: N/http.ServerRequest -->

The request a Suitelet or RESTlet receives. Everything on it is read-only. Server scripts; since
2015.2.

#### ServerRequest.body
<!-- id: N/http.ServerRequest#body -->

`string`, read-only. The request body. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ServerRequest.clientIpAddress
<!-- id: N/http.ServerRequest#clientIpAddress -->

`string`, read-only. The IP address of the remote client. Setting it throws `READ_ONLY_PROPERTY`.
Since 2015.2.

#### ServerRequest.files
<!-- id: N/http.ServerRequest#files -->

`Object`, read-only. The files sent with the request. Setting it throws `READ_ONLY_PROPERTY`.
Since 2015.2.

#### ServerRequest.headers
<!-- id: N/http.ServerRequest#headers -->

`Object`, read-only. Header name/value pairs. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

- Names are present in lower case and in title case, but title case is not guaranteed. Read them
  in lower case.
- The `Authorization` header is reserved for OAuth. For an authentication scheme of your own,
  send the value in an `X-Authorization` header instead.
- Custom header names must not contain underscores.
- Some headers are filtered out before a Suitelet sees them, and the set of headers can change;
  test any header you depend on.

#### ServerRequest.method
<!-- id: N/http.ServerRequest#method -->

`http.Method`, read-only. The request's HTTP method. Setting it throws `READ_ONLY_PROPERTY`. Since
2015.2.

#### ServerRequest.parameters
<!-- id: N/http.ServerRequest#parameters -->

`Object`, read-only. Parameter name/value pairs: from the URL for GET, from the request body for
POST. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

- Values cannot be arrays. Send a list as `JSON.stringify` text and `JSON.parse` it on arrival.
- Validate parameters to guard against cross-site scripting.

#### ServerRequest.url
<!-- id: N/http.ServerRequest#url -->

`string`, read-only. The request URL. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ServerRequest.getLineCount(options)
<!-- id: N/http.ServerRequest#getLineCount -->

Returns how many lines a sublist in the request has.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.group` | string | Yes | |

- `options.group`: the sublist's internal ID.

**Returns:** `number`, the line count.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.group` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerRequest.getSublistValue(options)
<!-- id: N/http.ServerRequest#getSublistValue -->

Returns the value of one field on one line of a sublist in the request.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.group` | string | Yes | |
| `options.name` | string | Yes | |
| `options.line` | number | Yes | |

- `options.name`: the field name.
- `options.line`: zero-based line index. (The `N/https` version of this method types it as a
  string; see [`N/https` ServerRequest.getSublistValue](N_https.md#serverrequestgetsublistvalueoptions).)

**Returns:** `string`, the field's value on that line.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.group` or `options.line` is missing. The error list does
  not mention `options.name`, although it is required.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### ServerResponse (object)
<!-- id: N/http.ServerResponse -->

The response a Suitelet or RESTlet sends back. It is also the object the call functions return when
the request was sent from a server script, such as a user event script. Server scripts; since
2015.2.

#### ServerResponse.headers
<!-- id: N/http.ServerResponse#headers -->

`Object`, read-only. The response headers. A header with several values is returned as an array
(for example a `Vary` header naming both `Accept-Language` and `Accept-Encoding`). Setting the
property throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ServerResponse.addHeader(options)
<!-- id: N/http.ServerResponse#addHeader -->

Adds a header. If one with the same name is already there, an additional line is added; this is
how a header such as `Vary` ends up carrying both `Accept-Language` and `Accept-Encoding`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.value` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_INVALID_HEADER`: the header name or value is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.name` or `options.value` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.getHeader(options)
<!-- id: N/http.ServerResponse#getHeader -->

Reads a response header.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

**Returns:** `string | string[]`: a string for a single value, an array for multiple values.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.name` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.renderPdf(options)
<!-- id: N/http.ServerResponse#renderPdf -->

Turns XML describing a PDF document into a PDF and writes it to the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.xmlString` | string | Yes | |

- `options.xmlString`: the XML content of the PDF.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.xmlString` is missing.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.sendRedirect(options)
<!-- id: N/http.ServerResponse#sendRedirect -->

Redirects the caller to a NetSuite resource: a file, a record, a RESTlet, a Suitelet or a task page.
For instance, you could send the user to a new sales order form for a given customer by
redirecting to the record type and passing the customer in `parameters`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | http.RedirectType | Yes | |
| `options.identifier` | number \| string | Yes | |
| `options.id` | number \| string | No | |
| `options.editMode` | boolean | No | `false` |
| `options.parameters` | Object | No | |

- `options.type`: a value from [`http.RedirectType`](#httpredirecttype).
- `options.identifier`: depends on `type`: file internal ID for `MEDIA_ITEM`, record type for
  `RECORD`, script ID for `RESTLET` and `SUITELET`, task ID for `TASK_LINK`.
- `options.id`: the deployment ID for `SUITELET` and `RESTLET`, the record's internal ID for
  `RECORD`.
- `options.editMode`: applies to record redirects; `true` opens the record in edit mode.
- `options.parameters`: extra URL parameters to append.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_RECORD_TYPE`: a `RECORD` redirect names a record type that is not valid
  (`options.identifier`).
- `SSS_INVALID_SCRIPT_ID_1`: the Suitelet or RESTlet IDs (`identifier`, `id`) are invalid.
- `SSS_INVALID_TASK_ID`: the task ID in `options.identifier` is invalid.
- `SSS_INVALID_URL_CATEGORY`: `options.type` is not one of the `RedirectType` values.
- `SSS_MISSING_REQD_ARGUMENT`: `options.identifier` or `options.type` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

- Watch the enum spelling: a mistyped value such as `TASKLINK` instead of `TASK_LINK` resolves to
  `undefined`, so you get `SSS_MISSING_REQD_ARGUMENT` rather than `SSS_INVALID_URL_CATEGORY`.
- These error codes differ from those of
  [`N/https` ServerResponse.sendRedirect](N_https.md#serverresponsesendredirectoptions).

#### ServerResponse.setCdnCacheable(options)
<!-- id: N/http.ServerResponse#setCdnCacheable -->

Sets how long a CDN may cache the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | enum | Yes | |

- `options.type`: a value from [`http.CacheDuration`](#httpcacheduration). With `UNIQUE` the
  Suitelet is never cached.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.type` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.setHeader(options)
<!-- id: N/http.ServerResponse#setHeader -->

Sets a response header. Blocked headers (see the
[catalog](../gen/catalogs/http_blocked_headers.md)) cannot be set.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.value` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_INVALID_HEADER`: the header name or value is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.name` or `options.value` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.write(options)
<!-- id: N/http.ServerResponse#write -->

Writes text to the response. It takes strings only; to send a file, use
[`writeFile`](#serverresponsewritefileoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.output` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.output` is missing.
- `WRONG_PARAMETER_TYPE`: `options.output` is not a string.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.writeFile(options)
<!-- id: N/http.ServerResponse#writeFile -->

Writes a [`file.File`](N_file.md) to the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.isInline` | boolean | No | `false` |

- `options.isInline`: `true` shows the file inline.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.file` is missing.
- `WRONG_PARAMETER_TYPE`: `options.file` is not a file object.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.writeLine(options)
<!-- id: N/http.ServerResponse#writeLine -->

Writes one line of text to the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.output` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.output` is missing.
- `WRONG_PARAMETER_TYPE`: `options.output` is not a string.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.writePage(options)
<!-- id: N/http.ServerResponse#writePage -->

Renders a page object from [`N/ui/serverWidget`](N_ui_serverWidget.md) as the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.pageObject` | serverWidget.Assistant \| serverWidget.Form \| serverWidget.List | Yes | |

- `options.pageObject`: a `serverWidget.Assistant`, `serverWidget.Form` or `serverWidget.List`.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.pageObject` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Enums

### http.CacheDuration
<!-- id: N/http.CacheDuration -->

CDN cache lifetimes for [`ServerResponse.setCdnCacheable`](#serverresponsesetcdncacheableoptions).
Since 2015.2.

| Value | Cached for |
|---|---|
| `LONG` | Days |
| `MEDIUM` | Hours |
| `SHORT` | Minutes |
| `UNIQUE` | Never cached |

### http.Method
<!-- id: N/http.Method -->

HTTP methods for [`http.request`](#httprequestoptions). The source lists the value names only and
does not describe them individually. Since 2015.2.

| Value |
|---|
| `DELETE` |
| `GET` |
| `HEAD` |
| `PUT` |
| `POST` |
| `PATCH` |

### http.RedirectType
<!-- id: N/http.RedirectType -->

Redirect targets for [`ServerResponse.sendRedirect`](#serverresponsesendredirectoptions). The
source lists the values without separate descriptions; the `identifier` rules of `sendRedirect`
show what each one points to. Since 2015.2.

| Value | `identifier` holds |
|---|---|
| `MEDIA_ITEM` | File internal ID |
| `RECORD` | Record type |
| `RESTLET` | Script ID |
| `SUITELET` | Script ID |
| `TASK_LINK` | Task ID |

## Examples

A scheduled script sends a partial stock correction to an older on-premises inventory service that
only speaks plain HTTP, using `request` with `PATCH`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/http'], (http) => {
  const execute = () => {
    const correction = { sku: 'PLT-0442', adjustBy: -3, reason: 'cycle count' };
    const response = http.request({
      method: http.Method.PATCH,
      url: 'http://inventory-bridge.example.com/api/stock/PLT-0442',
      headers: { 'content-type': 'application/json' },
      body: JSON.stringify(correction)
    });
    const level = response.code < 300 ? 'audit' : 'error';
    log[level]({ title: 'Stock correction', details: `HTTP ${response.code}` });
  };

  return { execute };
});
```

A Suitelet that receives a posted list of receiving lines, totals the quantities that were
rejected, and then sends the user to the related vendor bill in view mode.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/http'], (http) => {
  const onRequest = (context) => {
    const { request, response } = context;
    if (request.method !== http.Method.POST) {
      response.write({ output: 'Submit the receiving form to continue.' });
      return;
    }

    const lineCount = request.getLineCount({ group: 'custpage_receipts' });
    let rejected = 0;
    for (let line = 0; line < lineCount; line += 1) {
      const qty = request.getSublistValue({ group: 'custpage_receipts', name: 'custpage_rejected_qty', line });
      rejected += Number(qty) || 0;
    }
    log.audit({ title: 'Rejected quantity', details: `${rejected} across ${lineCount} lines` });

    response.sendRedirect({
      type: http.RedirectType.RECORD,
      identifier: 'vendorbill',
      id: request.parameters.custpage_bill_id,
      editMode: false
    });
  };

  return { onRequest };
});
```

A Suitelet that streams a stored file inline and allows hour-scale CDN caching.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/http', 'N/file'], (http, file) => {
  const onRequest = (context) => {
    // Internal ID of the price sheet, passed by the link that opens this Suitelet.
    const sheetId = Number(context.request.parameters.sheet);
    const sheet = file.load({ id: sheetId });
    context.response.setCdnCacheable({ type: http.CacheDuration.MEDIUM });
    context.response.writeFile({ file: sheet, isInline: true });
  };

  return { onRequest };
});
```
