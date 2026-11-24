# OAuth 2.0 for SuiteScript
<!-- id: topic:oauth2 -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [N/crypto/certificate](../modules/N_crypto_certificate.md), [RESTlet authentication](restlet_auth.md),
[Machine-to-machine token flow](restlet_auth.oauth2_m2m.md).

A script can call NetSuite endpoints with an OAuth 2.0 access token that it obtains on its own, through
the client credentials (machine-to-machine) flow. The script builds a JWT, signs it with a certificate
stored in NetSuite, exchanges it for an access token, and then sends that token with its requests to
NetSuite endpoints. The setup draws on two account facilities: API secrets and certificate management.

## Prerequisites

Features that must be enabled:

- Server SuiteScript
- OAuth 2.0

In the account that will receive the requests (the target account):

- an integration record, created at **Setup > Integration > Manage Integrations > New**, and
- the public key of your certificate, uploaded at **Setup > Integration > OAuth 2.0 Client Credentials
  (M2M) Setup**.

The certificate used for signing is uploaded at **Setup > Company > Certificates**.

## Certificates and algorithms

Two kinds of certificate are supported: RSASSA-PSS and ECDSA.

| Certificate | Signature algorithms |
|---|---|
| RSASSA-PSS | `PS256`, `PS384`, `PS512` |
| ECDSA | `ES256` (curve P-256), `ES384` (curve P-384), `ES512` (curve P-512 as the source names it) |

> [!SOURCE-ERROR] The source pairs `ES512` with a curve called P-512. The curve normally used with ES512 is
> named P-521, so this looks like a slip; check the curve name your certificate tooling expects.
<!-- source-error: topic:oauth2 | - | es512_curve_written_p_512 -->

With ECDSA the curve fixes the hash function, so choose the algorithm that matches your certificate's
curve.

## The flow

1. Build the JWT.
2. Sign it with the certificate's key, using [`Signer.sign`](../modules/N_crypto_certificate.md) from
   [N/crypto/certificate](../modules/N_crypto_certificate.md).
3. POST the signed JWT to the token endpoint.
4. Send the access token you get back in the `Authorization` header of your requests.

For an ECDSA-signed JWT the signature must be in raw form rather than the default ASN.1 DER encoding:
set `useRawFormatForECDSA` when calling [`Signer.sign`](../modules/N_crypto_certificate.md).

## What goes into the JWT and the token request

The source illustrates the flow with an example; the pieces it shows are:

| Part | Item | Content |
|---|---|---|
| JWT header | `typ` | `JWT` |
| JWT header | `alg` | The algorithm for your certificate type and hash (see the table above) |
| JWT header | `kid` | The ID of the certificate in the target account |
| JWT claims | `iss` | The client ID of the integration record |
| JWT claims | `scope` | `restlets` |
| JWT claims | `aud` | The token endpoint URL |
| JWT claims | `iat` | Issue time, in seconds since the Unix epoch |
| JWT claims | `exp` | Expiry time, in seconds since the Unix epoch |
| Encoding | | Each part base64url-encoded without padding |
| Token request | `grant_type` | `client_credentials` |
| Token request | `client_assertion_type` | The JWT bearer URN |
| Token request | `client_assertion` | The signed JWT |
| Token request | URL | The same URL you put in `aud` |
| Token response | `access_token` | The token to use |
| Later requests | header | `Authorization: Bearer <access_token>` |

These items come from one worked example, so treat the `scope` value as that example's choice; the
[Machine-to-machine token flow](restlet_auth.oauth2_m2m.md) page lists all scope values and the other
token rules. Never put keys, secrets or tokens in script source; keep them in NetSuite's certificate and
secret stores.
