# Code grant flow for RESTlet callers
<!-- id: topic:restlet_auth.oauth2_code_grant -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [OAuth 2.0](oauth2.md), [OAuth 2.0 errors](restlet_auth.oauth2_errors.md),
[Machine-to-machine token flow](restlet_auth.oauth2_m2m.md), [Restlet script type](../script-types/Restlet.md).

The authorization code grant (RFC 6749) is the OAuth 2.0 flow for an application acting on behalf of
a person: the user signs in to NetSuite in a browser and consents, the application receives a short
code, and it trades that code for tokens. The resulting access token works for RESTlets, REST web
services, SuiteAnalytics Connect and the AI connector, depending on the scopes granted.

Everything below names parameters only; `<client_id>`, `<client_secret>` and similar are placeholders,
not values to copy.

## Before you start

- One integration record per application.
- The client must be able to open a browser for the user.
- The scopes you will request must be enabled on the integration record.
- The redirect URI you will send must exactly match one of the redirect URIs on the record.

## Overview

| Step | Request | Required? |
|---|---|---|
| 1 | `GET` the authorize endpoint in the user's browser | Yes |
| 2 | `POST` the code to the token endpoint | Yes |
| 3 | `POST` a refresh token to the token endpoint | Optional |
| 4 | `POST` a token to the revoke endpoint | Optional |

All `POST` bodies are sent as `application/x-www-form-urlencoded`.

## Step 1: authorize

Open `/app/login/oauth2/authorize.nl` on the account's application domain. If you do not know the
account yet, use the system domain instead.

| Parameter | Meaning |
|---|---|
| `response_type` | Always `code` |
| `client_id` | Client ID of the integration record |
| `redirect_uri` | Must exactly match a redirect URI on the record |
| `scope` | Space-separated list of `restlets`, `rest_webservices`, `suite_analytics`, `mcp` |
| `state` | 22 to 1,024 printable ASCII characters, unique for each run of the flow |
| `code_challenge` | PKCE challenge (see below) |
| `code_challenge_method` | `S256` |
| `prompt` | Controls login and consent screens (see below) |

Scope rules: every scope you request must be enabled on the integration record, and `mcp` can only be
requested on its own, not combined with other scopes.

### PKCE

- Generate a `code_verifier` of 43 to 128 characters and keep it.
- The `code_challenge` is the SHA-256 hash of the verifier, and `code_challenge_method` is `S256`.
- The `plain` method has not been supported since release 2020.2.
- A challenge is mandatory for public clients, for any request with the `mcp` scope, and for every
  integration created from release 2027.1 onwards.

> [!SOURCE-ERROR] The source describes the challenge method value as SHA256, whereas the value that the
> rest of the material gives for `code_challenge_method` is `S256`. Send `S256`.
<!-- source-error: topic:restlet_auth.oauth2_code_grant | - | challenge_method_described_as_sha256 -->

### The prompt parameter and consent policy

`prompt` accepts `none`, `login`, `consent`, or `login` and `consent` together.

- `none`: never show a consent screen; if the user has no session, the request ends in an error.
- `login`: make the user authenticate again; this only works on the account-specific domain.
- `consent`: show the consent screen every time.

The integration record also carries a consent policy: always ask, ask the first time, or never ask.
Together with `prompt` it decides whether the consent screen appears:

| Consent policy | Without `prompt=consent` | With `prompt=consent` |
|---|---|---|
| Always ask | Shown | Shown |
| Ask the first time | Shown the first time, and again when the scope or role is new | Shown |
| Never ask | Not shown | Shown |

`prompt=login` forces re-authentication whichever policy is set. The never-ask policy cannot be used
with the `mcp` scope.

### The redirect

NetSuite redirects to your `redirect_uri` with `state`, `code`, `role`, `entity` and `company`, or with
`error` when something went wrong (see [OAuth 2.0 errors](restlet_auth.oauth2_errors.md)). The code is
short-lived: exchange it straight away.

## Step 2: exchange the code for tokens

`POST` to `/auth/oauth2/v1/token` on the account's SuiteTalk (REST web services) domain, with:

| Parameter | Value |
|---|---|
| `code` | The code from the redirect |
| `redirect_uri` | The same redirect URI as in step one |
| `grant_type` | `authorization_code` |
| `code_verifier` | The verifier behind the challenge from step one |

If the verifier does not match the challenge, the call fails with HTTP 400.

> [!SOURCE-ERROR] The source names HTTP 400 "Bad Response"; the standard reason phrase for 400 is Bad
> Request. Handle the status code, not the phrase.
<!-- source-error: topic:restlet_auth.oauth2_code_grant | - | http_400_called_bad_response -->

### Client authentication at the token endpoint

- **Confidential clients** use HTTP Basic authentication: the client ID and client secret joined by a
  colon, encoded, in the `Authorization` header (`Authorization: Basic <encoded client_id:client_secret>`).
- **Public clients** either send `client_id` in the body and no `Authorization` header, or send an
  `Authorization` header that carries only the client ID.

> [!SOURCE-ERROR] The source says the Basic credentials are Base64url-encoded, but HTTP Basic
> authentication (RFC 7617) uses standard Base64. Use standard Base64 and test against your account.
<!-- source-error: topic:restlet_auth.oauth2_code_grant | - | basic_credentials_described_as_base64url -->

### The response

- The access token is a JWT and lives 60 minutes; `expires_in` is `3600` and `token_type` is `bearer`.
- The refresh token lives 7 days.
- For public clients the refresh token behaves differently: by default it lasts 2 days (configurable
  from 1 to 720 hours), and it can be used only once.
- When the token is granted, the integration is installed in the account automatically, but only if
  the integration record's require-approval box is clear.

> [!SOURCE-ERROR] The source refers to a `token_id` [sic] where an OpenID Connect `id_token` appears to
> be meant. The spec does not describe this response member further; inspect the actual response.
<!-- source-error: topic:restlet_auth.oauth2_code_grant | - | id_token_called_token_id -->

## Step 3: refresh

`POST` to the same token endpoint with `grant_type` set to `refresh_token` and `refresh_token` set to
the refresh token. Client authentication works as in step two (Basic for confidential clients; client
ID only for public clients).

> [!SOURCE-ERROR] Here too the source calls the Basic credentials Base64url; RFC 7617 specifies
> standard Base64.
<!-- source-error: topic:restlet_auth.oauth2_code_grant | - | basic_credentials_described_as_base64url -->

- The new access token is again a 60-minute JWT (`expires_in` `3600`, `token_type` `bearer`).
- A public client receives a new refresh token along with the access token, because its old one is
  single-use.
- Once the refresh token itself has expired, the call fails with `invalid_grant`; start over at step
  one.
- Tokens already issued keep working even if the user gets locked out.

## Step 4: revoke

`POST` to `/auth/oauth2/v1/revoke` on the SuiteTalk domain with one parameter, `token`, holding the
refresh token. This revokes the refresh token and every access token issued from it. Client
authentication is the same as for the token endpoint.

> [!SOURCE-ERROR] The revoke description repeats the Base64url wording for Basic credentials; use
> standard Base64 as RFC 7617 defines.
<!-- source-error: topic:restlet_auth.oauth2_code_grant | - | basic_credentials_described_as_base64url -->

## Inside the access token

The JWT has three Base64url parts separated by dots: header, payload and signature.

| Part | Claim | Content |
|---|---|---|
| Header | `kid` | ID of the signing key |
| Header | `typ` | `JWT` |
| Header | `alg` | `PS256` |
| Payload | `sub` | Role and entity, separated by a semicolon |
| Payload | `aud` | The application ID, the company and the client ID |
| Payload | `scope` | Granted scopes |
| Payload | `iss` | Issuer |
| Payload | `oit` | Present for public clients only |
| Payload | `exp` | Expiry time |
| Payload | `iat` | Issue time |
| Payload | `jti` | Unique token ID |

To verify a token, fetch the public keys from `/auth/oauth2/v1/keys` on the SuiteTalk domain. Since
2021.1 the signing certificates are company-specific, valid for 90 days, and rotated 30 days before
they expire, so refresh your cached keys rather than pinning one.

## Calling the RESTlet

Send the access token as `Authorization: Bearer <access_token>`; OAuth 2.0 calls are not signed.
