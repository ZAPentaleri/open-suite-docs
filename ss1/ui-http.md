# UI, HTTP, URL and portlet functions (SuiteScript 1.0)

> **Legacy API.** This page exists so that tools and engineers can read, convert and minimally repair
> existing SuiteScript 1.0 scripts. It is not a guide for writing new 1.0 code; new work belongs in
> SuiteScript 2.1. Each section names the 2.x replacement where the mapping gives one.

1.0 functions take **positional** arguments, so the parameter tables list them in the order the call
must use. A `**Since:**` line appears where the source states the release that introduced a
function.

Related reading:

- Concepts: [SuiteScript 1.0 API overview](../topics/ss1_api.md),
  [1.0 Suitelets](../topics/ss1_api.suitelets.md), [1.0 portlets](../topics/ss1_api.portlets.md),
  [1.0 RESTlets](../topics/ss1_api.restlets.md), [1.0 user events](../topics/ss1_api.user_events.md),
  [1.0 client scripts](../topics/ss1_api.client_scripts.md).
- UI objects returned here (`nlobjForm`, `nlobjList`, `nlobjAssistant`, `nlobjTemplateRenderer`):
  [1.0 form objects](objects-forms.md) and [other 1.0 UI objects](objects-ui-other.md); report
  objects, `nlobjResponse` and credential objects: [1.0 service objects](objects-services.md).
- Converting: [SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).

## Building pages

### nlapiCreateAssistant(title, hideHeader)
<!-- id: ss1:nlapiCreateAssistant -->

Starts a custom assistant: a multi-step wizard page that a Suitelet assembles step by step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |
| `hideHeader` | boolean | No | `false` |

- `hideHeader`: `true` hides the page header. Keep in mind that the header is where the "Add to
  Shortcuts" link lives, so hiding the header removes that link as well.

**Returns:** `nlobjAssistant`, the builder object for the wizard.

**Governance:** 0 units.

**Availability:** Suitelets only.

**Since:** 2009.2

**2.x:** `serverWidget.createAssistant(options)` in [N/ui/serverWidget](../modules/N_ui_serverWidget.md)
(options object; the two positional arguments become named options).

### nlapiCreateForm(title, hideNavbar)
<!-- id: ss1:nlapiCreateForm -->

Creates a form page that a Suitelet fills with fields, sublists and buttons before writing it to the
response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |
| `hideNavbar` | boolean | No | |

- `hideNavbar`: meant for pages shown as pop-ups. The links in the navigation bar only work for a
  logged-in user, so hiding the bar suits pages where that may not be the case.

**Returns:** `nlobjForm`

**Governance:** 0 units.

**Availability:** Suitelets only.

**2.x:** `serverWidget.createForm(options)` in [N/ui/serverWidget](../modules/N_ui_serverWidget.md).

### nlapiCreateList(title, hideNavbar)
<!-- id: ss1:nlapiCreateList -->

Creates a standalone list page for a Suitelet to populate with columns and rows.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |
| `hideNavbar` | boolean | No | |

- `hideNavbar`: as for `nlapiCreateForm`, intended for pop-up pages, because the navigation bar's
  links need a logged-in user.

**Returns:** `nlobjList`, a standalone list page.

**Governance:** 0 units.

**Availability:** Suitelets only.

**2.x:** `serverWidget.createList(options)` in [N/ui/serverWidget](../modules/N_ui_serverWidget.md).

### nlapiCreateTemplateRenderer()
<!-- id: ss1:nlapiCreateTemplateRenderer -->

Creates a renderer for templates written in FreeMarker syntax. The rendered output can leave the
script in one of two forms: as HTML written to an `nlobjResponse`, or as an XML string that you then
hand to [`nlapiXMLToPDF`](utilities.md#nlapixmltopdfxmlstring) to get a PDF.

**Returns:** `nlobjTemplateRenderer`

**Governance:** 0 units.

**Availability:** requires the feature "Advanced PDF/HTML Templates" to be enabled. The source does
not state which script types may call this function.

**Since:** 2013.1

**2.x:** `render.create()` in [N/render](../modules/N_render.md) (the mapping calls it a rename). See
also [advanced templates](../topics/advanced_templates.md).

## HTTP requests

### nlapiRequestURL(url, postdata, headers, callback, httpMethod)
<!-- id: ss1:nlapiRequestURL -->

Sends an HTTP or HTTPS request and returns the response. The argument list depends on where the
code runs:

- In **client** scripts the order is `url, postdata, headers, callback, httpMethod`.
- In **server** scripts there is no `callback`, so the order is `url, postdata, headers, httpMethod`,
  and `httpMethod` is the fourth argument.

When converting or repairing a call, establish which side it runs on before you interpret the fourth
argument.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `url` | string | Yes | |
| `postdata` | string \| Object | No | |
| `headers` | Object | No | |
| `callback` | Function | No | |
| `httpMethod` | string | No | |

- `url`: must not contain white space; encode parameter values with `encodeURIComponent`.
- `postdata`: either a string body or an object of form parameters. Passing `null` makes the request
  a GET. It is ignored when the method is DELETE.
- `headers`: an object of request headers.
- `callback`: client side only. When you supply it the request runs asynchronously. Leave it out
  whenever the script needs the result immediately, for example inside a validation function.
- `httpMethod`: `HEAD`, `DELETE` or `PUT`. When it is omitted, the request is a POST if `postdata` is
  given and a GET otherwise. An explicit method takes precedence over what `postdata` implies, and a
  GET ignores `postdata`. On the server side this is the fourth argument.

> [!SOURCE-ERROR] The source's list of supported values for `httpMethod` names only HEAD, DELETE and
> PUT, yet the same description talks about GET. It does not settle whether `GET` is accepted as an
> explicit value; to get a GET, omit both the method and `postdata`.
<!-- source-error: ss1:nlapiRequestURL | httpMethod | supported_list_omits_get_yet_get_described -->

**Returns:** `nlobjResponse | void`. When a `callback` is given the call itself returns `undefined`
and the response goes to the callback. Binary response content arrives base64-encoded.

**Errors:**

- `SSS_CONNECTION_CLOSED`: the connection is in an invalid (closed) state.
- `SSS_CONNECTION_TIME_OUT`: the request timed out.
- `SSS_INVALID_HOST_CERT`: the remote side failed; the remote host must present a valid certificate.
- `SSS_INVALID_URL`: raised for an invalid state; by its name, the URL is not usable.
- `SSS_REQUEST_TIME_EXCEEDED`: the request ran past its time limit.
- `SSS_UNKNOWN_HOST`: the host could not be found.
- `SSS_UNSUPPORTED_ENCODING`: the encoding is not supported in this context.

**Governance:** 10 units.

**Availability:** client, user event, scheduled, portlet and Suitelet scripts.

- Timeouts: 5 seconds to establish the connection and 45 seconds to receive the response.
- Server-side calls carry no user session. Consequently, the only Suitelets a server script can
  reach this way are those available without login. Client-side calls keep the user's session.
- TLS 1.0, 1.1 and 1.2 are accepted; 1.2 is the recommended version.
- Only these response content types are honoured: `text/*`, `application/json`,
  `application/vnd.maxmind.com-country+json`, `application/xml`, `application/soap+xml`,
  `application/xhtml+xml` and `application/atom+xml`. Any other content type is replaced by a
  default.

**2.x:** split into `http.get`, `http.post`, `http.put`, `http.delete` and `http.request` in
[N/http](../modules/N_http.md) (and the matching [N/https](../modules/N_https.md) calls); the HTTP
method decides which one.

### nlapiRequestURLWithCredentials(credentials, url, postdata, headers, httpsMethod)
<!-- id: ss1:nlapiRequestURLWithCredentials -->

A variant of `nlapiRequestURL` for requests that must carry stored credentials, such as values a
user typed into a credential field on a Suitelet form. Note that the credentials come first in the
argument list.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `credentials` | Array | Yes | |
| `url` | string \| nlobjCredentialBuilder | Yes | |
| `postdata` | string \| nlobjCredentialBuilder \| Object | No | |
| `headers` | nlobjCredentialBuilder \| Object | No | |
| `httpsMethod` | string | No | |

- `credentials`: an array of credential handles (GUIDs). It may be `null` when you pass an
  `nlobjCredentialBuilder` for the URL, body or headers instead.
- `url`: when it is an `nlobjCredentialBuilder`, pass the builder as it is; do not encrypt or encode
  it yourself.
- `httpsMethod`: `HEAD`, `DELETE` or `PUT`. When omitted, the request is a POST if `postdata` is given
  and a GET otherwise; an explicit method takes precedence over `postdata`, and a GET ignores
  `postdata`.

> [!SOURCE-ERROR] The fifth parameter is called `httpsMethod` in the 1.0 reference but `httpMethod`
> in the source's API mapping (the name `nlapiRequestURL` uses). Only the name differs; in both it is
> the fifth positional argument.
<!-- source-error: ss1:nlapiRequestURLWithCredentials | - | param_httpsmethod_in_ss1_ref_httpmethod_in_api_map -->

**Returns:** `nlobjResponse`

**Governance:** 10 units.

**Availability:** user event, scheduled, portlet and Suitelet scripts.

**Since:** 2012.1

- Credential handles come from `nlobjForm.addCredentialField`, which cannot be used in Suitelets that
  are available without login.
- The URL, body and headers can each be supplied as an `nlobjCredentialBuilder`.
- Inside the URL, the body or a header value, the inline syntax `$base64(text)` base64-encodes the
  enclosed text when the request goes out.
- Timeouts: 5 seconds to connect, 45 seconds for the response.
- Only the `application/json` and `application/soap+xml` response content types are honoured; others
  are replaced by a default.

**2.x:** `https.request(options)` in [N/https](../modules/N_https.md).

## URLs and redirects

### nlapiResolveURL(type, identifier, id, displayMode)
<!-- id: ss1:nlapiResolveURL -->

Builds the URL of a record, a task link, a Suitelet or a RESTlet. Depending on the arguments, the
result is an internal URL or an external one.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `identifier` | string | Yes | |
| `id` | string | No | |
| `displayMode` | string \| boolean | No | |

- `type`: `RECORD`, `TASKLINK`, `SUITELET` or `RESTLET`.
- `identifier`: its meaning follows `type`: the record type for `RECORD`, the script ID for
  `SUITELET`, the task ID for `TASKLINK`. The source does not state what to pass when `type` is
  `RESTLET`.
- `id`: the record's internal ID for `RECORD`, the deployment ID for `SUITELET`. It is needed when a
  `RECORD` URL should point at one specific record. The source does not state its meaning for
  `RESTLET`.
- `displayMode`: for `RECORD`, `VIEW` or `EDIT`, with `VIEW` as the default. For `SUITELET` and
  `RESTLET` it is a boolean: `true` gives the external URL, `false` (the default) the internal one.

**Returns:** `string`, the resolved internal or external URL.

**Errors:**

- `SSS_INVALID_URL_CATEGORY`: `type` is not a valid URL category.
- `SSS_CATEGORY_ARG_REQD`: the `type` argument is missing.
- `SSS_INVALID_TASK_ID`: the task ID passed is not valid.
- `SSS_TASK_ID_REQD`: a task ID is required but missing.
- `SSS_INVALID_INTERNAL_ID`: the internal ID passed is not valid.
- `SSS_INVALID_EDITMODE_ARG`: the display mode value is not valid.

**Governance:** 0 units.

**Availability:** client, user event, scheduled, portlet, Suitelet and RESTlet scripts.

**2.x:** split into `url.resolveRecord`, `url.resolveScript` and `url.resolveTaskLink` in
[N/url](../modules/N_url.md); the 1.0 `type` decides which one.

### nlapiSetRedirectURL(type, identifier, id, editmode, parameters)
<!-- id: ss1:nlapiSetRedirectURL -->

Tells NetSuite where to send the browser once the current request finishes: to a record, a task
link, a Suitelet, or an external URL.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `identifier` | string | Yes | |
| `id` | string | No | |
| `editmode` | boolean | No | |
| `parameters` | Object | No | |

- `type`: `RECORD`, `TASKLINK`, `SUITELET` or `EXTERNAL`. `EXTERNAL` is only allowed in Suitelets
  that are called externally.
- `identifier`: the record type for `RECORD`, the script ID for `SUITELET`, the task ID for
  `TASKLINK`, or the URL itself for `EXTERNAL`.
- `id`: the record's internal ID for `RECORD`, the deployment ID for `SUITELET`. With `RECORD` and a
  `null` ID, the user lands on a page for creating a new record of that type.
- `editmode`: `true` opens the record in edit mode.
- `parameters`: extra URL parameters. Every key must start with `custparam`; any other key raises
  `SSS_INVALID_ARG`.

**Returns:** `void` (the call returns `undefined`).

**Errors:**

- `SSS_INVALID_ARG`: an argument value is not valid (for example a parameter key without the
  `custparam` prefix).
- `SSS_INVALID_URL_CATEGORY`: `type` is not a valid URL category.
- `SSS_CATEGORY_ARG_REQD`: the `type` argument is missing.
- `SSS_INVALID_TASK_ID`: the task ID passed is not valid.
- `SSS_TASK_ID_REQD`: a task ID is required but missing.
- `SSS_INVALID_INTERNAL_ID`: the internal ID passed is not valid.
- `SSS_INVALID_EDITMODE_ARG`: the `editmode` value is not valid.

**Governance:** 0 units.

**Availability:** user event before load, user event after submit when it runs synchronously, and
Suitelets.

- In before submit, and in after submit executions that run asynchronously, the call is ignored.
- The target record must already exist. To send the user to a record you are creating, submit it
  first and then redirect to the new ID.

**2.x:** split into `redirect.redirect`, `redirect.toRecord`, `redirect.toSuitelet` and
`redirect.toTaskLink` in [N/redirect](../modules/N_redirect.md); the 1.0 `type` decides which one.

## Portlets

### nlapiRefreshPortlet()
<!-- id: ss1:nlapiRefreshPortlet -->

Reloads the current portlet straight away. It applies to form-type portlets.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** only from the client script attached to a custom form portlet, or from event
handlers on that portlet's elements. The form portlet script itself cannot call it directly.

**Since:** 2011.1

**2.x:** `portlet.refresh()` in [N/portlet](../modules/N_portlet.md) (the mapping calls it a rename).

### nlapiResizePortlet()
<!-- id: ss1:nlapiResizePortlet -->

Resizes a custom form portlet to fit its content. This is necessary because form portlets are
rendered inside an iframe that does not size itself automatically.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** only from the client script attached to a custom form portlet, or from event
handlers on that portlet's elements; the form portlet script itself cannot call it directly.

**Since:** 2011.1

**2.x:** `portlet.resize()` in [N/portlet](../modules/N_portlet.md) (the mapping calls it a rename).

## Reports

### nlapiCreateReportDefinition()
<!-- id: ss1:nlapiCreateReportDefinition -->

Creates a pivot report definition. You can either display it through a report form
([`nlapiCreateReportForm`](#nlapicreatereportformtitle)) or read the values of the resulting pivot
table in script.

**Returns:** `nlobjReportDefinition` (see [1.0 service objects](objects-services.md)).

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2012.2

**2.x:** the mapping has no entry for this function, and the spec names no closest approach.

### nlapiCreateReportForm(title)
<!-- id: ss1:nlapiCreateReportForm -->

Creates the form that displays an `nlobjReportDefinition`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |

**Returns:** `nlobjReportForm` (described with the other report objects in
[1.0 service objects](objects-services.md)).

**Governance:** 0 units.

**Availability:** the source does not state which script types may call this function.

**Since:** 2012.2

**2.x:** the mapping has no entry for this function, and the spec names no closest approach.

## Examples

These 1.0 snippets are plain functions, written to show how legacy code is shaped so it can be read
and converted.

A Suitelet that asks for a warehouse bin code on GET and, on POST, sends the user back to the
item record the correction concerns:

```js
function binCorrectionPage(request, response) {
  if (request.getMethod() === 'GET') {
    var form = nlapiCreateForm('Correct bin assignment', true);
    form.addField('custpage_item', 'integer', 'Item ID');
    form.addField('custpage_bin', 'text', 'New bin code');
    form.addSubmitButton('Save');
    response.writePage(form);
    return;
  }
  var itemId = request.getParameter('custpage_item');
  nlapiSetRedirectURL('RECORD', 'inventoryitem', itemId, false, {
    custparam_bin_note: request.getParameter('custpage_bin')
  });
}
```

A server-side GET (no `postdata`, no method) to a carrier's rate service, with the query value
encoded so the URL contains no white space:

```js
function fetchFreightQuote(destinationZip) {
  var url = 'https://rates.example.invalid/quote?zip=' + encodeURIComponent(destinationZip);
  var reply = nlapiRequestURL(url, null, { Accept: 'application/json' });
  if (reply.getCode() !== '200') {
    nlapiLogExecution('ERROR', 'Quote service', 'HTTP ' + reply.getCode());
    return null;
  }
  return JSON.parse(reply.getBody());
}
```

Building an external link to a Suitelet deployment and storing it on a support case for a partner
to use:

```js
function publishReturnFormLink(caseId) {
  var link = nlapiResolveURL('SUITELET', 'customscript_rma_intake', 'customdeploy_rma_public', true);
  nlapiSubmitField('supportcase', caseId, 'custevent_rma_link', link + '&case=' + caseId);
  return link;
}
```
