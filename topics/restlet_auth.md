# RESTlet authentication
<!-- id: topic:restlet_auth -->

Sub-topics:

- [TBA request signing](restlet_auth.tba_signing.md)
- [TBA three-step authorization flow](restlet_auth.tba_flow.md)
- [TBA issuetoken endpoint](restlet_auth.tba_issuetoken.md)
- [TBA token REST endpoints](restlet_auth.token_endpoints.md)
- [TBA errors](restlet_auth.tba_errors.md)
- [Code grant flow for RESTlet callers](restlet_auth.oauth2_code_grant.md)
- [Machine-to-machine token flow](restlet_auth.oauth2_m2m.md)
- [OAuth 2.0 errors](restlet_auth.oauth2_errors.md)
- [RESTlet user-credential (NLAuth) authentication](restlet_auth.nlauth.md)

See also: [Restlet script type](../script-types/Restlet.md), [OAuth 2.0 for SuiteScript](oauth2.md),
[RESTlet HTTP status codes](../gen/catalogs/restlet_http_status_codes.md),
[RESTlet SuiteScript errors](../gen/catalogs/restlet_suitescript_errors.md).

A [RESTlet](../script-types/Restlet.md) is reached over HTTP, so whoever calls it from outside has to
prove its identity on each request. This page is the overview; each sub-topic covers one method in
detail.

## The supported methods

Two families are available, token-based authentication (TBA) and OAuth 2.0, and each comes in two
variants:

| Family | Variant | Typical fit | Details |
|---|---|---|---|
| TBA | Three-step authorization flow | A client that can open a browser so a user can approve it | [TBA three-step authorization flow](restlet_auth.tba_flow.md) |
| TBA | `issuetoken` endpoint | A client that creates its token programmatically from user credentials | [TBA issuetoken endpoint](restlet_auth.tba_issuetoken.md), [TBA token REST endpoints](restlet_auth.token_endpoints.md) |
| OAuth 2.0 | Authorization code grant | An application working on behalf of a user who signs in and consents | [Code grant flow for RESTlet callers](restlet_auth.oauth2_code_grant.md) |
| OAuth 2.0 | Client credentials (machine to machine) | A server-to-server integration with nobody signing in | [Machine-to-machine token flow](restlet_auth.oauth2_m2m.md) |

TBA is based on the OAuth 1.0 specification: the caller computes a signature for every request and
places it in the `Authorization` header (see [TBA request signing](restlet_auth.tba_signing.md)).

OAuth 2.0 requests carry no signature. The caller first obtains an access token, then presents it as a
bearer token: `Authorization: Bearer <access_token>`, where the angle-bracket part is a placeholder for
the token the flow returned. Script-side OAuth 2.0 (a SuiteScript calling NetSuite endpoints) is
covered in [OAuth 2.0 for SuiteScript](oauth2.md).

There is also an older scheme that sends the user's own login details in the header,
[NLAuth](restlet_auth.nlauth.md). It no longer works for new RESTlets and is being retired; external
callers are meant to use OAuth 2.0 or TBA.

## TBA is being phased out

TBA carries a deprecation from release 2027.1. From that release no new TBA integrations can be
created, and new RESTlet integrations should be built on OAuth 2.0 instead. For integrations that keep
using TBA in the meantime, the recommendation is to move those that rely on the `issuetoken` endpoint
over to the three-step authorization flow.

## Rules that apply to every method

- **Roles.** A role limited to web services only cannot be used to call a RESTlet.
- **Sandbox and release preview accounts.** TBA tokens are not carried over when a sandbox or release
  preview account is created or refreshed, and neither are OAuth 2.0 authorizations. After a sandbox
  refresh, create the TBA tokens again and repeat the OAuth 2.0 authorization in that account.

## The RESTlet URL

RESTlets are called on the account's RESTlet domain, at `/app/site/hosting/restlet.nl`, with the script
and deployment chosen by the `script` and `deploy` query parameters (details in
[TBA request signing](restlet_auth.tba_signing.md#what-a-restlet-request-looks-like)).

> [!SOURCE-ERROR] One example RESTlet URL in the source is built on the account's application (UI)
> domain, while the rest of the material uses the RESTlet domain. Address RESTlets on the RESTlet
> domain, and verify in your account before relying on the other host.
<!-- source-error: topic:restlet_auth | - | restlet_url_example_uses_app_domain -->

## Diagnosing failures

Causes of failed authentication attempts are described in [TBA errors](restlet_auth.tba_errors.md) and
[OAuth 2.0 errors](restlet_auth.oauth2_errors.md). Status codes and errors that the RESTlet itself
returns are listed in the [RESTlet HTTP status codes](../gen/catalogs/restlet_http_status_codes.md) and
[RESTlet SuiteScript errors](../gen/catalogs/restlet_suitescript_errors.md) catalogs.
