# Machine-to-machine token flow
<!-- id: topic:restlet_auth.oauth2_m2m -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [OAuth 2.0 for SuiteScript](oauth2.md), [Code grant flow for RESTlet callers](restlet_auth.oauth2_code_grant.md),
[OAuth 2.0 errors](restlet_auth.oauth2_errors.md).

This is the OAuth 2.0 client credentials flow, meant for one server talking to another: no person signs
in and no consent screen appears. The integration identifies itself with a JWT that it signs using the
private key of a certificate registered with NetSuite. The exchange has two steps: post the signed
assertion to the token endpoint, and receive an access token in return.

Everything in angle brackets below (`<ClientID>` and so on) is a placeholder. Keep real keys, secrets
and tokens out of code and logs.

## Setting up certificates

A certificate has to be mapped to the integration before any token can be requested. Uploading and
revoking certificates in the UI requires the permission for managing OAuth 2.0 authorized applications.

The certificates can also be handled over REST. The three endpoints share the base path
`/services/rest/auth/oauth2/v1/clients`, take `ClientID` (and, for revocation, also `CertificateID`) as
path parameters, and use the content type `application/json`:

| Purpose | Request | Inputs |
|---|---|---|
| List the certificates of a client | `GET` `.../clients/<ClientID>/certificates` | `ClientID` |
| Register a new certificate | `POST` `.../clients/<ClientID>/certificates` | `ClientID`, plus `fileContent`, `role` and `entity` |
| Revoke a certificate | `POST` `.../clients/<ClientID>/certificates/<CertificateID>/revoke` | `ClientID` and `CertificateID`; send an empty body |

To call these endpoints you need the permission to manage your own OAuth 2.0 client credentials
certificates, and the access token you present must include the `rest_webservices` scope.

Registering a certificate returns its certificate ID. Put that ID into the `kid` header of each assertion
you sign with the matching private key.

> [!SOURCE-ERROR] The source writes the response field holding this ID as `ceritificate_id` [sic], which
> looks like a typo. Look at an actual response to confirm the field name before relying on it.
<!-- source-error: topic:restlet_auth.oauth2_m2m | - | response_field_written_ceritificate_id -->

> [!SOURCE-ERROR] The source shows the certificate endpoints on the RESTlet domain, although the token
> endpoint of the same flow lives on the SuiteTalk (REST web services) domain. Which host the certificate
> endpoints really use is not settled by the source; check it in your account.
<!-- source-error: topic:restlet_auth.oauth2_m2m | - | certificate_endpoints_shown_on_restlets_domain -->

## Requesting a token

Send a `POST` to `/services/rest/auth/oauth2/v1/token` on the account-specific SuiteTalk domain, with the
parameters form-urlencoded:

| Parameter | Value |
|---|---|
| `grant_type` | `client_credentials` |
| `client_assertion_type` | The JWT bearer assertion type defined by RFC 7523 |
| `client_assertion` | Your JWT, signed with the private key of the mapped certificate |

Use a JWT library to build and sign the assertion instead of assembling it by hand.

### The assertion

| Part | Claim | Content |
|---|---|---|
| Header | `typ` | `JWT` |
| Header | `alg` | One of `PS256`, `PS384`, `PS512`, `ES256`, `ES384`, `ES512` |
| Header | `kid` | The certificate ID returned when the certificate was registered |
| Payload | `iss` | The client ID of the integration record |
| Payload | `scope` | One or more of `restlets`, `rest_webservices`, `suite_analytics`, comma-separated |
| Payload | `aud` | The token endpoint URL |
| Payload | `exp` | Expiry time |
| Payload | `iat` | Issue time |

`exp` minus `iat` must not exceed 60 minutes.

> [!SOURCE-ERROR] One sentence in the source says the private key is used in `kid`. Everywhere else the
> material treats `kid` as the certificate ID and the private key as what signs the JWT; follow that, and
> never put key material into a token header.
<!-- source-error: topic:restlet_auth.oauth2_m2m | - | says_private_key_used_in_kid -->

### The response

- The access token is itself a JWT and is valid for 60 minutes: `expires_in` is `3600` and `token_type`
  is `bearer`.
- After the token expires, calls fail with `invalid_grant`. Start the flow again with a new assertion to
  get a fresh token.

How the access token is then sent with a RESTlet request (as a bearer token in the `Authorization`
header) is described on the parent page, [RESTlet authentication](restlet_auth.md).
