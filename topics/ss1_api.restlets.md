# SuiteScript 1.0 RESTlets
<!-- id: topic:ss1_api.restlets -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is [Restlet](../script-types/Restlet.md),
> and current authentication options are described in [RESTlet authentication](restlet_auth.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## Calling a 1.0 RESTlet

A caller can authenticate in one of three ways: an NLAuth authorization header, OAuth 1.0, or an
existing NetSuite session. A RESTlet cannot log that session out.

## Methods and functions

The script record names one function per HTTP method. Each function gets its input differently:

| HTTP method | What the function receives | Notes |
|---|---|---|
| GET | An object of the request's parameters | |
| POST | The request body | |
| PUT | The request body | |
| DELETE | An object of the request's parameters | Returns nothing; the response has no content |

## Content types and payloads

- Only `application/json` and `text/plain` are accepted. Any other content type is rejected with
  HTTP 415.
- Input arrives either as a string or as an object deserialized from JSON; output can be a string or
  an object that NetSuite serializes.
- The response uses the same content type as the request, and so do error responses.
- An input or output string can be at most 10 MB.
- Scripts can build their own errors with [`nlapiCreateError`](../ss1/utilities.md).

**Governance:** 5,000 usage units per script execution.

## HTTP status codes

| Code | Meaning |
|---|---|
| 200 | Success |
| 302 | The account lives in another data center; sign the request again for that host |
| 400 | User error |
| 401 | No valid session |
| 403 | Invalid domain |
| 404 | Script not found |
| 405 | Method not allowed (no function for that method) |
| 415 | Unsupported media type |
| 500 | Unexpected error |
| 503 | Database unavailable |

An error the script does not catch comes back as 400. If the script catches the error and returns
something, the caller sees 200.

## OAuth errors

| Code | Cause |
|---|---|
| `INVALID_REQUEST` | The authorization header is malformed |
| `INVALID_LOGIN_ATTEMPT` | The nonce, consumer key, token or signature is wrong |

## Converting

In 2.1 the four functions become the `get`, `post`, `put` and `delete` entry points of
[Restlet](../script-types/Restlet.md). See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

A GET function that returns a phone call's subject and phone number. A request without an ID gets
a named error (which, being uncaught, reaches the caller as a 400) instead of a confusing load
failure.

```js
function getPhoneCall(params) {
  var id = params.id;
  if (!id) {
    throw nlapiCreateError('MISSING_ID', 'Pass the phone call id as ?id=', true);
  }
  var call = nlapiLoadRecord('phonecall', id);
  return {
    id: id,
    title: call.getFieldValue('title'),
    phone: call.getFieldValue('phone')
  };
}
```
