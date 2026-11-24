# N/https
<!-- id: N/https -->

`N/https` is how SuiteScript talks to other systems over HTTPS. It covers four jobs: ordinary
outbound calls to third parties (`get`, `post`, `put`, `delete` and the general `request`), calls to
RESTlets, Suitelets and SuiteTalk REST endpoints of your own account without signing in a second
time, the request and response objects that a Suitelet or RESTlet works with, and `SecureString`
handling so that credentials and secrets stay out of script code. Functionally it matches
[`N/http`](N_http.md), except that it never uses plain HTTP.

- **Availability:** client and server scripts at module level; many members are server only (see
  each section).
- **Load it as:** `define(['N/https'], (https) => { ... })`
- **Side effect:** loading `N/https` also loads [`N/crypto`](N_crypto.md) and [`N/encode`](N_encode.md).
- Related: [N/http](N_http.md), [N/https/clientCertificate](N_https_clientCertificate.md),
  [N/url](N_url.md), [blocked headers catalog](../gen/catalogs/http_blocked_headers.md).

**What the module can do beyond plain calls**

- Encode binary content.
- Work with the handle of a credential field value (a GUID) instead of the secret itself.
- Call RESTlets and SuiteTalk REST services in the same account without authenticating again.
  [`requestRestlet`](#httpsrequestrestletoptions) runs the target RESTlet with the privileges of
  the calling script.

**Transport and security**

- The remote server must support TLS 1.2; otherwise the handshake fails.
- A server certificate is trusted when it chains to a CA on Mozilla's included CA certificate list.
  The remote server must send its full certificate chain, intermediate certificates included.
- When a third party needs authentication, prefer OAuth 2.0 over sending credentials as plain text.

**Headers**

- Header names are not case-sensitive.
- Custom header names must not contain underscores.
- Some headers are blocked; see the [blocked headers catalog](../gen/catalogs/http_blocked_headers.md).
  For the general group, any value you set is silently discarded. If a Suitelet's response sets one
  of the headers in the Suitelet-response group, the call fails with `SSS_INVALID_HEADER`.

**Content types and encoding**

- The module-level facts give the default `Content-Type` of a POST that does not set one as
  form-urlencoded UTF-8 for a `Uint8Array` body and `text/xml` UTF-8 for any other body.
- The module-level facts also say that a POST with an object body and no content type is sent as
  `application/octet-stream`, while [`https.post`](#httpspostoptions) says that a `Uint8Array` body
  defaults to `application/octet-stream`.

> [!SOURCE-ERROR] The module-level table of default POST content types appears to have its two
> rows swapped compared with the `https.post` page, so the defaults above contradict each other.
> Always set `Content-Type` yourself instead of relying on any default.
<!-- source-error: N/https | - | post_default_content_type_rows_swapped_vs_post_page -->

- Responses are decoded as UTF-8 by default.

**Failures**

- A synchronous call that fails throws a `SuiteScriptError`.
- A `.promise` call that fails rejects its Promise with the error.

> [!SOURCE-ERROR] The source describes `ClientResponse.headers` as though it held the response
> body. It holds the headers; read the body from `ClientResponse.body`.
<!-- source-error: N/https | - | clientresponse_headers_described_as_response_body -->

## Members

| Member | Kind | Summary |
|---|---|---|
| [`https.get(options)`](#httpsgetoptions) | function | HTTPS GET |
| [`https.get.promise(options)`](#httpsgetpromiseoptions) | function | Promise form of `get` |
| [`https.post(options)`](#httpspostoptions) | function | HTTPS POST |
| [`https.post.promise(options)`](#httpspostpromiseoptions) | function | Promise form of `post` |
| [`https.put(options)`](#httpsputoptions) | function | HTTPS PUT |
| [`https.put.promise(options)`](#httpsputpromiseoptions) | function | Promise form of `put` |
| [`https.delete(options)`](#httpsdeleteoptions) | function | HTTPS DELETE |
| [`https.delete.promise(options)`](#httpsdeletepromiseoptions) | function | Promise form of `delete` |
| [`https.request(options)`](#httpsrequestoptions) | function | HTTPS call with any `https.Method` |
| [`https.request.promise(options)`](#httpsrequestpromiseoptions) | function | Promise form of `request` |
| [`https.requestRestlet(options)`](#httpsrequestrestletoptions) | function | Call a RESTlet in this account without re-authenticating |
| [`https.requestRestlet.promise(options)`](#httpsrequestrestletpromiseoptions) | function | Promise form of `requestRestlet` |
| [`https.requestSuitelet(options)`](#httpsrequestsuiteletoptions) | function | Call a Suitelet in this account without re-authenticating |
| [`https.requestSuitelet.promise(options)`](#httpsrequestsuiteletpromiseoptions) | function | Promise form of `requestSuitelet` |
| [`https.requestSuiteTalkRest(options)`](#httpsrequestsuitetalkrestoptions) | function | Call a SuiteTalk REST endpoint without re-authenticating |
| [`https.createSecureString(options)`](#httpscreatesecurestringoptions) | function | Wrap a secret placeholder in a `SecureString` |
| [`https.createSecretKey(options)`](#httpscreatesecretkeyoptions) | function | Get a `crypto.SecretKey` for a credential GUID or API secret |
| [`SecureString`](#securestring-object) | object | Opaque string for sensitive data |
| [`ClientResponse`](#clientresponse-object) | object | Response to an outbound call |
| [`ServerRequest`](#serverrequest-object) | object | Request received by a Suitelet or RESTlet |
| [`ServerResponse`](#serverresponse-object) | object | Response a Suitelet or RESTlet sends back |
| [`https.CacheDuration`](#httpscacheduration) | enum | CDN cache lifetimes |
| [`https.Encoding`](#httpsencoding) | enum | Encodings for `SecureString` work |
| [`https.HashAlg`](#httpshashalg) | enum | Hash algorithms for `hash` and `hmac` |
| [`https.Method`](#httpsmethod) | enum | HTTP methods |
| [`https.RedirectType`](#httpsredirecttype) | enum | Targets for `sendRedirect` |

## Functions

### https.get(options)
<!-- id: N/https.get -->

Sends a GET request to an HTTPS URL and waits for the reply.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `https.ClientResponse | https.ServerResponse`. Called from a client script you get a
`ClientResponse`; called from a server script, a `ServerResponse`.

**Errors:**

- `SSS_INVALID_HOST_CERT`: the host in `options.url` is malformed or breaks the domain-name rules
  below.
- `SSS_INVALID_URL`: `options.url` is not a valid URL.
- `SSS_MISSING_REQD_ARGUMENT`: `options.url` was not supplied.
- `SSS_REQUEST_LOOP_DETECTED`: the script ends up calling itself recursively over HTTP (the error
  list files this under remote failures).

**Governance:** 10 units.

**Availability:** client and server scripts, but not in client contexts where the user is not
authenticated.

**Since:** 2015.2

- Host-name rules: at most 63 characters, only letters, digits and hyphens, starting with a letter
  and ending with a letter or digit. A host that breaks them is rejected with
  `SSS_INVALID_HOST_CERT`.
- The source does not state connect or send time limits for `get` (the sibling functions `post`,
  `put`, `delete` and `request` state 5 and 45 seconds).

### https.get.promise(options)
<!-- id: N/https.get.promise -->

Asynchronous form of [`https.get`](#httpsgetoptions). It takes the same parameters and raises the
same errors, but returns a Promise that resolves with the response, or rejects with the error.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`https.get`](#httpsgetoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### https.post(options)
<!-- id: N/https.post -->

Sends a POST request with a body to an HTTPS URL.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object \| Uint8Array | Yes | |
| `options.credentials` | string[] | No | |
| `options.headers` | Object | No | |

- `options.url`: a plain string or an [`https.SecureString`](#securestring-object). Reserved URL
  characters are not escaped for you. When the URL contains them, use
  [`SecureString.replaceString`](#securestringreplacestringoptions) to escape them.
- `options.body`: a string, an object or a `Uint8Array`. `Uint8Array` bodies work only in
  SuiteScript 2.1 and default to the `application/octet-stream` content type. An object body is only
  appropriate for `application/x-www-form-urlencoded`; for any other content type, serialize the
  data with `JSON.stringify` first. Multipart `form-data` file uploads are not supported.
- `options.credentials`: credential GUIDs. Each GUID that appears in the body is replaced by the
  decrypted password when the request is sent.

**Returns:** `https.ClientResponse | https.ServerResponse`, a `ClientResponse` when called client
side and a `ServerResponse` when called server side.

**Errors:**

- `SSS_INVALID_HOST_CERT`: security negotiation with the host failed, or the domain in
  `options.url` is malformed.
- `SSS_INVALID_URL`: `options.url` is not a fully qualified URL.
- `SSS_MISSING_REQD_ARGUMENT`: `options.body` or `options.url` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script is calling itself recursively over HTTP.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- The connection must be made within 5 seconds and the request sent within 45 seconds.
- Domain names: at most 63 characters of letters, digits and hyphens, starting with a letter and
  ending with a letter or digit.

### https.post.promise(options)
<!-- id: N/https.post.promise -->

Promise form of [`https.post`](#httpspostoptions), with the same parameters and errors. When the
target is a Suitelet, that Suitelet waits for its own pending promises before it completes.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object \| Uint8Array | Yes | |
| `options.credentials` | string[] | No | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`https.post`](#httpspostoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### https.put(options)
<!-- id: N/https.put -->

Sends a PUT request with a body to an HTTPS URL.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | Yes | |
| `options.credentials` | string[] | No | |
| `options.headers` | Object | No | |

- `options.url`: a string or an [`https.SecureString`](#securestring-object). Reserved characters
  are not escaped automatically; when the URL has them, escape them with
  [`SecureString.replaceString`](#securestringreplacestringoptions).
- `options.credentials`: credential GUIDs; each GUID found in the body is swapped for the
  decrypted password when the request goes out.

**Returns:** `https.ClientResponse | https.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: the host in `options.url` is malformed or breaks the domain-name rules.
- `SSS_INVALID_URL`: `options.url` is not a valid URL.
- `SSS_MISSING_REQD_ARGUMENT`: `options.body` or `options.url` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script calls itself recursively over HTTP.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- Connect time is capped at 5 seconds and send time at 45 seconds.
- Host names: at most 63 characters of letters, digits and hyphens, beginning with a letter and
  ending with a letter or digit; otherwise `SSS_INVALID_HOST_CERT`.

### https.put.promise(options)
<!-- id: N/https.put.promise -->

Promise form of [`https.put`](#httpsputoptions); same parameters, same errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | Yes | |
| `options.credentials` | string[] | No | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`https.put`](#httpsputoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### https.delete(options)
<!-- id: N/https.delete -->

Sends a DELETE request to an HTTPS URL. There is no `body` option, because a delete needs no
payload.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `https.ClientResponse | https.ServerResponse` (client side / server side).

**Errors:**

- `SSS_INVALID_HOST_CERT`: the host in `options.url` is malformed or breaks the domain-name rules.
- `SSS_INVALID_URL`: `options.url` is not a valid URL.
- `SSS_MISSING_REQD_ARGUMENT`: `options.url` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script calls itself recursively over HTTP.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- 5-second connect limit and 45-second send limit.
- Host names: at most 63 characters of letters, digits and hyphens, starting with a letter and
  ending with a letter or digit; otherwise `SSS_INVALID_HOST_CERT`.

### https.delete.promise(options)
<!-- id: N/https.delete.promise -->

Promise form of [`https.delete`](#httpsdeleteoptions); same parameters, same errors.

> [!SOURCE-ERROR] The source's description of this method speaks of HTTP rather than HTTPS. It
> belongs to `N/https` and sends an HTTPS DELETE; treat the HTTP wording as a copying slip.
<!-- source-error: N/https.delete.promise | - | description_says_http_not_https -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`https.delete`](#httpsdeleteoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### https.request(options)
<!-- id: N/https.request -->

General-purpose HTTPS call where you pick the method from [`https.Method`](#httpsmethod). A
[`SecureString`](#securestring-object) can serve as the credentials of this call.

> [!SOURCE-ERROR] The source's description of this function repeats one of its sentences. No
> behaviour depends on it, but the duplicated text is flagged for human review.
<!-- source-error: N/https.request | - | sentence_repeated_in_description -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.method` | enum | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | No | |
| `options.credentials` | string[] | No | |
| `options.headers` | Object | No | |

- `options.method`: a value from [`https.Method`](#httpsmethod).
- `options.body`: used only for PUT, POST and PATCH; ignored for any other method.
- `options.credentials`: credential GUIDs that are replaced by their decrypted passwords inside the
  body when the request is sent.

**Returns:** `https.ClientResponse | https.ServerResponse`, depending on whether the caller runs
client side or server side.

**Errors:**

- `SSS_INVALID_HOST_CERT`: the host in `options.url` is malformed or breaks the domain-name rules.
- `SSS_INVALID_URL`: `options.url` is not a valid URL.
- `SSS_MISSING_REQD_ARGUMENT`: `options.method` or `options.url` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script calls itself recursively over HTTP.
- `SSS_REQUEST_TIME_EXCEEDED`: connecting took more than 5 seconds or sending took more than 45.

**Governance:** 10 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

- Limits: 5 seconds to connect, 45 seconds to send.
- Host names: at most 63 characters of letters, digits and hyphens, starting with a letter and
  ending with a letter or digit; otherwise `SSS_INVALID_HOST_CERT`.

### https.request.promise(options)
<!-- id: N/https.request.promise -->

Promise form of [`https.request`](#httpsrequestoptions); same parameters, same errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.method` | enum | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string \| Object | No | |
| `options.credentials` | string[] | No | |
| `options.headers` | Object | No | |

**Returns:** `Promise`

**Errors:** same as [`https.request`](#httpsrequestoptions).

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### https.requestRestlet(options)
<!-- id: N/https.requestRestlet -->

Calls a RESTlet deployed in the same account. The authentication headers are added for you, so you
must not set an `Authorization` header yourself. The RESTlet runs with the privileges of the
calling script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string \| number | Yes | |
| `options.deploymentId` | string | Yes | |
| `options.method` | string | No | `GET` |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |
| `options.urlParams` | Object | No | |

- `options.scriptId`: the RESTlet's internal ID (a number) or its script ID (a string).
- `options.method`: one of `DELETE`, `GET`, `HEAD`, `POST`, `PUT`. The default depends on the body:
  GET when no body is given, POST when one is.
- `options.body`: conditionally required: needed for POST and PUT, ignored for other methods.
- `options.headers`: conditionally required: needed for POST and PUT, and it must then contain a
  `Content-Type` header.
- `options.urlParams`: query-string parameters added to the RESTlet URL.
- All options are documented since 2020.2.

**Returns:** `https.ClientResponse`

**Errors:**

- `INVALID_SCRIPT_DEPLOYMENT_ID_1`: `options.deploymentId` is not a valid deployment of the script.
- `SSS_AUTHORIZATION_HEADER_NOT_ALLOWED`: you set an `Authorization` header.
- `SSS_INVALID_HEADER`: `options.headers` contains an invalid header.
- `SSS_INVALID_SCRIPT_ID_1`: `options.scriptId` is invalid or the script is not a RESTlet.
- `SSS_INVALID_URL_PARAMS`: `options.urlParams` is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.scriptId`, `options.deploymentId`, or (for a method that
  needs one) `options.body` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script calls itself recursively over HTTP (listed as a limit
  being exceeded).

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

### https.requestRestlet.promise(options)
<!-- id: N/https.requestRestlet.promise -->

Promise form of [`https.requestRestlet`](#httpsrequestrestletoptions). It takes the same parameters
and raises the same errors, and returns a Promise for the response.

> [!SOURCE-ERROR] The source lists server scripts as where this promise method runs, while its
> description talks about calling it from the client. The spec records server-side availability;
> test in your target script type before relying on client use.
<!-- source-error: N/https.requestRestlet.promise | - | lists_server_scripts_but_describes_client_use -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string \| number | Yes | |
| `options.deploymentId` | string | Yes | |
| `options.method` | string | No | `GET` |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |
| `options.urlParams` | Object | No | |

- `options.method`: GET when there is no body, POST when there is.
- `options.body` and `options.headers`: conditionally required, under the same rules as
  `requestRestlet`.

**Returns:** `Promise`

**Errors:**

- `INVALID_SCRIPT_DEPLOYMENT_ID_1`: `options.deploymentId` is invalid.
- `SSS_AUTHORIZATION_HEADER_NOT_ALLOWED`: an `Authorization` header was set.
- `SSS_INVALID_HEADER`: `options.headers` is invalid.
- `SSS_INVALID_SCRIPT_ID_1`: `options.scriptId` is invalid.
- `SSS_INVALID_URL_PARAMS`: `options.urlParams` is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.body`, `options.deploymentId` or `options.scriptId` is
  missing.
- `SSS_REQUEST_LOOP_DETECTED`: recursive self-call detected.

**Governance:** 10 units.

**Availability:** server scripts (as recorded; see the callout).

**Since:** 2020.2

### https.requestSuitelet(options)
<!-- id: N/https.requestSuitelet -->

Calls a Suitelet deployed in the same account, adding the authentication headers itself. It works
only in trusted contexts with an authenticated user and cannot run in anonymous client contexts.
Since 2024.1 the former `options.external` parameter no longer exists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |
| `options.deploymentId` | string | Yes | |
| `options.method` | string | No | `GET` |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |
| `options.urlParams` | Object | No | |

- `options.method`: a value from [`https.Method`](#httpsmethod). When omitted it is GET if there is
  no body and POST if there is.
- `options.body`: used only with PUT, POST or PATCH; ignored otherwise.
- `options.urlParams`: query-string parameters for the Suitelet URL.
- All options are documented since 2023.1.

**Returns:** `https.ClientResponse`

**Errors:**

- `INVALID_SCRIPT_DEPLOYMENT_ID_1`: the deployment is not valid for the script.
- `SSS_AUTHORIZATION_HEADER_NOT_ALLOWED`: you set an `Authorization` header.
- `SSS_INVALID_HEADER`: `options.headers` is invalid.
- `SSS_INVALID_SCRIPT_ID_1`: `options.scriptId` is invalid or the script is not a Suitelet.
- `SSS_INVALID_URL_PARAMS`: `options.urlParams` is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.deploymentId` or `options.scriptId` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script calls itself recursively over HTTP.

**Governance:** 10 units.

**Availability:** client and server scripts (authenticated, trusted contexts only).

**Since:** 2023.1

### https.requestSuitelet.promise(options)
<!-- id: N/https.requestSuitelet.promise -->

Promise form of [`https.requestSuitelet`](#httpsrequestsuiteletoptions), with the same parameters
and errors. Its own list of methods is `DELETE`, `GET`, `HEAD`, `POST`, `PUT`.

> [!SOURCE-ERROR] For this promise form the source says the body is sent with POST and PUT, while
> the synchronous form says PUT, POST and PATCH (and the method list here leaves out PATCH). The
> spec does not say which is right; verify PATCH behaviour before relying on it.
<!-- source-error: N/https.requestSuitelet.promise | - | body_methods_post_put_vs_sync_put_post_patch -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |
| `options.deploymentId` | string | Yes | |
| `options.method` | string | No | `GET` |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |
| `options.urlParams` | Object | No | |

- `options.method`: the default depends on the body (GET without one, POST with one).

**Returns:** `Promise`

**Errors:**

- `INVALID_SCRIPT_DEPLOYMENT_ID_1`: `options.deploymentId` is invalid.
- `SSS_AUTHORIZATION_HEADER_NOT_ALLOWED`: an `Authorization` header was set.
- `SSS_INVALID_HEADER`: `options.headers` is invalid.
- `SSS_INVALID_SCRIPT_ID_1`: `options.scriptId` is invalid.
- `SSS_INVALID_URL_PARAMS`: `options.urlParams` is invalid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.deploymentId` or `options.scriptId` is missing.
- `SSS_REQUEST_LOOP_DETECTED`: recursive self-call detected.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2023.1

### https.requestSuiteTalkRest(options)
<!-- id: N/https.requestSuiteTalkRest -->

Calls a SuiteTalk REST web services endpoint of the same account, with the authentication headers
added for you; do not set `Authorization` yourself.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.method` | string | No | `GET` |
| `options.body` | string \| Object | No | |
| `options.headers` | Object | No | |

- `options.url`: a fully qualified URL, a relative URL, or a relative URL without the
  `/services/rest` prefix.
- `options.method`: one of `DELETE`, `GET`, `HEAD`, `POST`, `PUT`. Defaults to GET without a body
  and POST with one.
- `options.body`: conditionally required: needed for POST and PUT.
- All options are documented since 2020.2.

**Returns:** `https.ClientResponse`

**Errors:**

- `SSS_AUTHORIZATION_HEADER_NOT_ALLOWED`: an `Authorization` header was set.
- `SSS_INVALID_HEADER`: `options.headers` is invalid.
- `SSS_INVALID_URL`: `options.url` is not a SuiteTalk REST endpoint.
- `SSS_MISSING_REQD_ARGUMENT`: `options.url`, `options.method` or a needed `options.body` is
  missing.
- `SSS_REQUEST_LOOP_DETECTED`: the script calls itself recursively over HTTP.

> [!SOURCE-ERROR] The source lists `options.method` among the arguments whose absence raises
> `SSS_MISSING_REQD_ARGUMENT`, although the method is optional and defaults by body. Passing the
> method explicitly avoids depending on either reading.
<!-- source-error: N/https.requestSuiteTalkRest | - | optional_method_listed_under_missing_required_argument -->

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2020.2

### https.createSecureString(options)
<!-- id: N/https.createSecureString -->

Creates a [`SecureString`](#securestring-object) from a placeholder for sensitive data, either a
credential GUID or an API secret reference, so the real value never sits in your code.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.input` | string | Yes | |
| `options.inputEncoding` | https.Encoding | No | `UTF_8` |

- `options.input`: accepted forms are a 32-hex-character GUID in braces, an API secret reference
  in braces made of `custsecret` followed by 1 to 89 word characters (for example
  `{custsecret_freight_portal}`), or tokens from payment plug-ins.
- `options.inputEncoding`: a value from [`https.Encoding`](#httpsencoding).

**Returns:** `https.SecureString`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### https.createSecretKey(options)
<!-- id: N/https.createSecretKey -->

Returns a [`crypto.SecretKey`](N_crypto.md) for a credential GUID or an API secret. The key is only
decrypted when it is actually sent. A GUID here resolves to the stored data or to its metadata. The
key can also be placed in a `SecureString` operation, for example as the `key` of
[`SecureString.hmac`](#securestringhmacoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.guid` | string | No | |
| `options.secret` | string | No | |
| `options.encoding` | https.Encoding | No | |

- `options.guid`: a GUID generated by a credential field
  ([`serverWidget.Form.addCredentialField`](N_ui_serverWidget.md)). Conditionally required: needed
  when `secret` is not given, and it cannot be combined with `options.secret`. Since 2015.2.
- `options.secret`: the script ID of an API secret, stored under Setup > Company > API Secrets.
  Conditionally required: needed when `guid` is not given. Since 2021.1.
- `options.encoding`: a value from [`https.Encoding`](#httpsencoding). Since 2015.2.

**Returns:** `crypto.SecretKey`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Objects

### SecureString (object)
<!-- id: N/https.SecureString -->

An opaque string holding sensitive data that you need to send over HTTPS. You get one from
[`https.createSecureString`](#httpscreatesecurestringoptions), and `appendString`,
`appendSecureString` and `replaceString` each hand one back. It can be used as the credentials of
[`https.request`](#httpsrequestoptions). Server scripts only; since 2015.2.

The transforming methods (`appendString`, `appendSecureString`, `convertEncoding`, `replaceString`)
change the `SecureString` they are called on and return that same object, not a new copy.

#### SecureString.appendString(options)
<!-- id: N/https.SecureString#appendString -->

Appends text to the secure string. It is intended for appending UTF-8 strings only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.input` | string | Yes | |
| `options.inputEncoding` | string | No | `UTF_8` |
| `options.keepEncoding` | boolean | No | `false` |

- `options.input`: the same accepted forms as `createSecureString`: a braced 32-hex GUID, a braced
  `custsecret` reference (1 to 89 word characters after the prefix), or payment plug-in tokens.
- `options.inputEncoding`: from [`https.Encoding`](#httpsencoding), but encodings other than UTF-8
  are not supported here.
- `options.keepEncoding`: when `true`, the appended content keeps its original encoding, which
  prevents it from being re-encoded unexpectedly.

**Returns:** `https.SecureString`, the same object after the append.

**Errors:**

- `FAILED_TO_CONVERT_BINARY_DATA_TO_UTF_8`: the input is malformed and cannot be converted (tied
  to `options.keepEncoding`).
- `FAILED_TO_DECODE_STRING_ENCODED_BINARY_DATA_USING_1_ENCODING`: the input cannot be decoded with
  the given encoding (tied to `options.keepEncoding`).
- `INVALID_VALUE_1_FOR_PARAMETER_2`: `keepEncoding` is used together with an input encoding other
  than UTF-8.

> [!SOURCE-ERROR] The source words this error condition in terms of `options.InputEncoding` [sic],
> a capitalisation that does not match the real parameter `options.inputEncoding`. Use the
> lower-case name.
<!-- source-error: N/https.SecureString#appendString | - | error_condition_names_options_InputEncoding -->

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### SecureString.appendSecureString(options)
<!-- id: N/https.SecureString#appendSecureString -->

Appends another `SecureString` to this one.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.secureString` | https.SecureString | Yes | |
| `options.keepEncoding` | boolean | No | `false` |

- `options.keepEncoding`: when `true`, the appended content keeps its original encoding instead of
  being re-encoded unexpectedly.

**Returns:** `https.SecureString`, the same object, modified.

**Errors:**

- `FAILED_TO_CONVERT_BINARY_DATA_TO_UTF_8`: malformed input (related to `options.keepEncoding`).
- `FAILED_TO_DECODE_STRING_ENCODED_BINARY_DATA_USING_1_ENCODING`: malformed input (related to
  `options.keepEncoding`).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### SecureString.convertEncoding(options)
<!-- id: N/https.SecureString#convertEncoding -->

Re-encodes the content from one [`https.Encoding`](#httpsencoding) to another.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fromEncoding` | string | Yes | |
| `options.toEncoding` | string | Yes | |

- `options.fromEncoding` and `options.toEncoding`: values from [`https.Encoding`](#httpsencoding).

**Returns:** `https.SecureString`, the same object, converted.

**Errors:**

- `FAILED_TO_CONVERT_BINARY_DATA_TO_UTF_8`: the content is malformed for the conversion.
- `FAILED_TO_DECODE_STRING_ENCODED_BINARY_DATA_USING_1_ENCODING`: the content cannot be decoded
  using `options.fromEncoding`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### SecureString.hash(options)
<!-- id: N/https.SecureString#hash -->

Hashes the content with the chosen algorithm and gives the result back as a secure string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.contentEncoding` | string | No | |
| `options.resultEncoding` | string | No | `HEX` |

- `options.algorithm`: a value from [`https.HashAlg`](#httpshashalg).
- `options.contentEncoding`: from [`https.Encoding`](#httpsencoding); when omitted, the string's
  current internal encoding is assumed. Always set it explicitly.
- `options.resultEncoding`: from [`https.Encoding`](#httpsencoding). The `HEX` default only
  applies when `contentEncoding` is set; otherwise the current internal encoding is used. Always set
  it explicitly.

**Returns:** `https.SecureString` holding the hash.

**Errors:**

- `FAILED_TO_CONVERT_BINARY_DATA_TO_UTF_8`: malformed content.
- `FAILED_TO_DECODE_STRING_ENCODED_BINARY_DATA_USING_1_ENCODING`: the content cannot be decoded with
  `options.contentEncoding`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### SecureString.hmac(options)
<!-- id: N/https.SecureString#hmac -->

Computes a keyed hash (HMAC) of the content and returns it as a secure string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.key` | crypto.SecretKey \| https.SecureString | Yes | |
| `options.contentEncoding` | string | No | |
| `options.resultEncoding` | string | No | `HEX` |

- `options.algorithm`: a value from [`https.HashAlg`](#httpshashalg).
- `options.key`: a key returned by [`https.createSecretKey`](#httpscreatesecretkeyoptions) (or a
  `SecureString`).
- `options.contentEncoding`: from [`https.Encoding`](#httpsencoding); defaults to the current
  internal encoding. Always set it explicitly.
- `options.resultEncoding`: from [`https.Encoding`](#httpsencoding); `HEX` when `contentEncoding`
  is set, otherwise the current internal encoding. Always set it explicitly.

**Returns:** `https.SecureString` holding the HMAC.

**Errors:**

- `FAILED_TO_CONVERT_BINARY_DATA_TO_UTF_8`: malformed content.
- `FAILED_TO_DECODE_STRING_ENCODED_BINARY_DATA_USING_1_ENCODING`: the content cannot be decoded with
  `options.contentEncoding`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### SecureString.replaceString(options)
<!-- id: N/https.SecureString#replaceString -->

Replaces every occurrence of a pattern in the secure string. Its main use is escaping special
characters in a URL that is held in a `SecureString`, since the HTTPS call functions do not escape
reserved characters themselves; the source's own illustration percent-encodes a hash sign. It is
also the suggested follow-up for dropping or encoding the `=` padding of `BASE_64_URL_SAFE` output
(see [`https.Encoding`](#httpsencoding)).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.pattern` | string | Yes | |
| `options.replacement` | string | Yes | |

**Returns:** `https.SecureString`, the same object with the replacements applied (not a new one).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.2

### ClientResponse (object)
<!-- id: N/https.ClientResponse -->

The read-only result of an outbound HTTPS call (`get`, `post`, `requestRestlet` and so on). The
spec records it as server-side, since 2015.2.

> [!SOURCE-ERROR] The source marks this object as server-only, yet calls such as `https.get`
> return it in client scripts too, and its `headers` property describes client behaviour. Expect
> it on both sides and test in your script type.
<!-- source-error: N/https.ClientResponse | - | clientresponse_server_only_but_returned_in_client_scripts -->

#### ClientResponse.body
<!-- id: N/https.ClientResponse#body -->

`string`, read-only. The response body. Assigning to it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ClientResponse.code
<!-- id: N/https.ClientResponse#code -->

`number`, read-only. The HTTP status code. Assigning to it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ClientResponse.headers
<!-- id: N/https.ClientResponse#headers -->

`Object.<String, String[]>`, read-only. The response headers, comparable to
[`N/http` ClientResponse.headers](N_http.md#clientresponseheaders). Assigning to it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

- Client side, each header name maps to a list of strings; server side, to the first value only,
  as a string.
- Each header appears two or three times under different letter cases. Read names in lower case.

### ServerRequest (object)
<!-- id: N/https.ServerRequest -->

The request a Suitelet or RESTlet receives. The whole object is read-only. Server scripts; since
2015.2.

#### ServerRequest.body
<!-- id: N/https.ServerRequest#body -->

`string`, read-only. The request body. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ServerRequest.files
<!-- id: N/https.ServerRequest#files -->

`Object`, read-only. The files sent with the request. Setting it throws `READ_ONLY_PROPERTY`.
Since 2015.2.

#### ServerRequest.headers
<!-- id: N/https.ServerRequest#headers -->

`Object`, read-only. Header names paired with their values. Setting it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

- Names may arrive in lower case or in title case, and title case is not guaranteed; look headers
  up by their lower-case name.
- Custom header names must not contain underscores.
- Which headers arrive, and their values, can change; test before you depend on one.
- When a Suitelet is reached over HTTP, some headers are filtered out before it sees them. If the
  one you need is removed, carry the information in a different header.

#### ServerRequest.method
<!-- id: N/https.ServerRequest#method -->

`enum`, read-only. The HTTP method of the request. Setting it throws `READ_ONLY_PROPERTY`. Since
2015.2. The source types it only as `enum` without naming the enum; comparing it with
[`https.Method`](#httpsmethod) values is the natural reading.

#### ServerRequest.parameters
<!-- id: N/https.ServerRequest#parameters -->

`Object`, read-only. The request parameters: for GET they come from the URL, for POST from the
request body. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

- A parameter cannot hold an array. To pass a list, send it as `JSON.stringify` text and
  `JSON.parse` it on arrival.
- Validate parameters against cross-site scripting; never let `<script>` content through.

#### ServerRequest.url
<!-- id: N/https.ServerRequest#url -->

`string`, read-only. The URL of the request. Setting it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### ServerRequest.getLineCount(options)
<!-- id: N/https.ServerRequest#getLineCount -->

Counts the lines of a sublist that was posted with the request.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.group` | string | Yes | |

**Returns:** `number`, the number of lines in that sublist.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.group` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerRequest.getSublistValue(options)
<!-- id: N/https.ServerRequest#getSublistValue -->

Reads one field value from one line of a sublist posted with the request.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.group` | string | Yes | |
| `options.name` | string | Yes | |
| `options.line` | string | Yes | |

- `options.line`: the zero-based line index, typed as a string here.

> [!SOURCE-ERROR] Here `options.line` is typed as a string, while the same method in `N/http`
> types it as a number. The spec does not say which is right; a numeric index is what the
> zero-based description implies, so test which form your account accepts.
<!-- source-error: N/https.ServerRequest#getSublistValue | options.line | line_typed_string_vs_number_in_n_http -->

**Returns:** `string`, the value on that sublist line.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.group`, `options.line` or `options.name` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### ServerResponse (object)
<!-- id: N/https.ServerResponse -->

What a Suitelet or RESTlet sends back to its caller. By default, textual files written to it are
re-encoded to UTF-8. Server scripts; since 2015.2.

#### ServerResponse.headers
<!-- id: N/https.ServerResponse#headers -->

`Object`, read-only. The response headers. A header with several values comes back as an array (for
instance a `Vary` header naming both `Accept-Language` and `Accept-Encoding`). Setting the property
throws `READ_ONLY_PROPERTY`. Server scripts; since 2015.2.

#### ServerResponse.addHeader(options)
<!-- id: N/https.ServerResponse#addHeader -->

Adds a response header. If a header of that name already exists, another line is added rather than
the existing one being replaced.

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
<!-- id: N/https.ServerResponse#getHeader -->

Reads a response header by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

**Returns:** `string | string[]`: a string when the header has one value, an array when it has
several.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.name` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.renderPdf(options)
<!-- id: N/https.ServerResponse#renderPdf -->

Renders a PDF from an XML string and writes it to the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.xmlString` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.xmlString` is missing.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.sendRedirect(options)
<!-- id: N/https.ServerResponse#sendRedirect -->

Redirects the caller to a NetSuite resource: a File Cabinet file, a record, a RESTlet, a Suitelet,
or a page identified by task ID. Every custom parameter you pass must be prefixed with
`custparam`. The error codes for invalid IDs differ from those of
[`N/http` ServerResponse.sendRedirect](N_http.md#serverresponsesendredirectoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.identifier` | number \| string | Yes | |
| `options.id` | string | No | |
| `options.editMode` | boolean | No | `false` |
| `options.parameters` | object | No | |

- `options.type`: a value from [`https.RedirectType`](#httpsredirecttype).
- `options.identifier`: depends on `type`: the file's internal ID for `MEDIA_ITEM`, the record type
  for `RECORD`, the script ID for `RESTLET` and `SUITELET`, the task ID for `TASK_LINK`.

**Returns:** `void`

**Errors:**

- `INVALID_ID`: the Suitelet or RESTlet IDs are invalid (involves `type`, `identifier`, `id`).
- `INVALID_RCRD_TYPE`: a `RECORD` redirect names an invalid record type.
- `INVALID_TASK_ID`: the task ID is invalid.
- `SSS_INVALID_URL_CATEGORY`: `options.type` is not a `RedirectType` value.
- `SSS_MISSING_REQD_ARGUMENT`: `options.identifier` or `options.type` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.setCdnCacheable(options)
<!-- id: N/https.ServerResponse#setCdnCacheable -->

Sets how long a CDN may cache the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | https.CacheDuration | Yes | |

- `options.type`: a value from [`https.CacheDuration`](#httpscacheduration). `UNIQUE` means the
  Suitelet is never cached and is executed on every request.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.type` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.setHeader(options)
<!-- id: N/https.ServerResponse#setHeader -->

Sets a response header. Headers in the
[blocked headers catalog](../gen/catalogs/http_blocked_headers.md) cannot be set.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.value` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_INVALID_HEADER`: the header is invalid (for example a blocked Suitelet-response header).
- `SSS_MISSING_REQD_ARGUMENT`: `options.name` or `options.value` is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.write(options)
<!-- id: N/https.ServerResponse#write -->

Writes text to the response. Only strings are accepted.

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
<!-- id: N/https.ServerResponse#writeFile -->

Writes a [`file.File`](N_file.md) to the response. Textual files are re-encoded to UTF-8 by
default; to keep a file's original encoding, first set a `Content-Type` header whose charset
matches it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.isInline` | boolean | No | |

- `options.isInline`: whether the file is shown inline. The source does not state a default for
  this option in `N/https` (the `N/http` counterpart gives `false`).

> [!SOURCE-ERROR] The source describes `isInline` as though it applied to a field rather than to
> the file being written. It concerns the file; verify the inline behaviour in a test.
<!-- source-error: N/https.ServerResponse#writeFile | options.isInline | isinline_described_as_field_not_file -->

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.file` is missing.
- `WRONG_PARAMETER_TYPE`: `options.file` is not a file object.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### ServerResponse.writeLine(options)
<!-- id: N/https.ServerResponse#writeLine -->

Writes a line of text to the response.

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
<!-- id: N/https.ServerResponse#writePage -->

Renders a page object (a `serverWidget.Assistant`, `serverWidget.Form` or `serverWidget.List` from
[`N/ui/serverWidget`](N_ui_serverWidget.md)) as the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.pageObject` | serverWidget.Assistant \| serverWidget.Form \| serverWidget.List | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.pageObject` is missing.

**Governance:** 0 units. Another source gives 10 units; the spec does not say which is right.

> [!SOURCE-ERROR] The sources disagree on this method's governance cost (0 versus 10 units). If
> your usage budget is tight, plan for the higher figure until you have measured it.
<!-- source-error: N/https.ServerResponse#writePage | - | governance_sources_disagree -->

**Availability:** server scripts.

**Since:** 2015.2

## Enums

### https.CacheDuration
<!-- id: N/https.CacheDuration -->

CDN cache lifetimes for [`ServerResponse.setCdnCacheable`](#serverresponsesetcdncacheableoptions).
The same values as [`N/http`'s `CacheDuration`](N_http.md#httpcacheduration). Since 2020.2.

| Value | Cached for |
|---|---|
| `LONG` | Days |
| `MEDIUM` | Hours |
| `SHORT` | Minutes |
| `UNIQUE` | Never cached; the Suitelet runs on every request |

### https.Encoding
<!-- id: N/https.Encoding -->

Encodings used by `createSecureString`, `SecureString.appendString` and
`SecureString.convertEncoding` (and by the encoding options of `hash`, `hmac` and
`createSecretKey`). Apart from `BASE_64_URL_SAFE`, the source does not describe the values beyond
their names. Since 2020.2.

| Value | Notes |
|---|---|
| `UTF_8` | |
| `BASE_16` | |
| `BASE_32` | |
| `BASE_64` | |
| `BASE_64_URL_SAFE` | `+` becomes `-`, `/` becomes `_`, and `=` padding is kept. To drop or encode the padding, follow up with `SecureString.replaceString`. |
| `HEX` | |

### https.HashAlg
<!-- id: N/https.HashAlg -->

Hash algorithms for [`SecureString.hash`](#securestringhashoptions) and
[`SecureString.hmac`](#securestringhmacoptions). The source lists the names only. Since 2020.2.

| Value |
|---|
| `SHA256` |
| `SHA512` |

### https.Method
<!-- id: N/https.Method -->

HTTP methods for [`https.request`](#httpsrequestoptions) (also referenced by `requestSuitelet` and
by [`N/https/clientCertificate`](N_https_clientCertificate.md)). The source lists the names only.
Since 2020.2.

| Value |
|---|
| `DELETE` |
| `GET` |
| `HEAD` |
| `PUT` |
| `POST` |
| `PATCH` |

### https.RedirectType
<!-- id: N/https.RedirectType -->

Redirect targets for [`ServerResponse.sendRedirect`](#serverresponsesendredirectoptions). Since
2020.2.

| Value | Redirects to |
|---|---|
| `MEDIA_ITEM` | A file in the File Cabinet |
| `RECORD` | A NetSuite record |
| `RESTLET` | A deployed RESTlet |
| `SUITELET` | A deployed Suitelet |
| `TASK_LINK` | A NetSuite page identified by its task ID |

## Examples

A scheduled script pushes a dock schedule to a carrier. The carrier's endpoint (which embeds an
access key) is stored as an API secret, so the script only ever handles a `SecureString`, and the
content type is set explicitly rather than left to a default.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/https'], (https) => {
  const execute = () => {
    const endpoint = https.createSecureString({ input: '{custsecret_carrier_dock_feed}' });

    const slots = [
      { door: 4, opens: '07:30', trailer: 'TRL-118' },
      { door: 6, opens: '09:00', trailer: 'TRL-204' }
    ];
    const response = https.post({
      url: endpoint,
      body: JSON.stringify({ site: 'RENO-DC', slots }),
      headers: { 'content-type': 'application/json', accept: 'application/json' }
    });

    if (response.code >= 300) {
      log.error({ title: 'Dock feed rejected', details: `HTTP ${response.code}` });
      return;
    }
    log.audit({ title: 'Dock feed accepted', details: `${slots.length} slots sent` });
  };

  return { execute };
});
```

A Suitelet that answers with JSON. It reads a header in lower case, receives a list as JSON text
(parameters cannot hold arrays), and lets the CDN cache the answer for minutes.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/https'], (https) => {
  const onRequest = (context) => {
    const { request, response } = context;
    response.setHeader({ name: 'Content-Type', value: 'application/json' });

    if (request.method !== https.Method.GET) {
      response.write({ output: JSON.stringify({ error: 'GET only' }) });
      return;
    }

    const language = request.headers['accept-language'] || 'en';
    let zones = [];
    try {
      zones = JSON.parse(request.parameters.zones || '[]');
    } catch (e) {
      zones = [];
    }

    response.setCdnCacheable({ type: https.CacheDuration.SHORT });
    response.write({ output: JSON.stringify({ language, zoneCount: zones.length }) });
  };

  return { onRequest };
});
```

A user event script hands newly created vendor bills to an internal approval-routing RESTlet. No
`Authorization` header is set, because `requestRestlet` authenticates for you; a POST needs both a
body and a `Content-Type`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/https'], (https) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const reply = https.requestRestlet({
      scriptId: 'customscript_bill_routing',
      deploymentId: 'customdeploy_bill_routing',
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ billId: context.newRecord.id })
    });
    log.audit({ title: 'Bill routed', details: `status ${reply.code}` });
  };

  return { afterSubmit };
});
```

A client script asks a Suitelet for a shipping estimate without blocking the form, using the
promise form of `requestSuitelet` (an authenticated user is required).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/https'], (https) => {
  const fieldChanged = (context) => {
    if (context.fieldId !== 'shipmethod') {
      return;
    }
    const shipMethod = context.currentRecord.getValue({ fieldId: 'shipmethod' });
    https.requestSuitelet.promise({
      scriptId: 'customscript_ship_estimate',
      deploymentId: 'customdeploy_ship_estimate',
      urlParams: { method: shipMethod }
    }).then((response) => {
      const estimate = JSON.parse(response.body);
      log.debug({ title: 'Shipping estimate', details: estimate });
    }).catch((err) => {
      log.error({ title: 'Estimate failed', details: err.message });
    });
  };

  return { fieldChanged };
});
```
