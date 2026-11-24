# TBA three-step authorization flow
<!-- id: topic:restlet_auth.tba_flow -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [TBA request signing](restlet_auth.tba_signing.md), [TBA issuetoken endpoint](restlet_auth.tba_issuetoken.md),
[TBA token REST endpoints](restlet_auth.token_endpoints.md), [TBA errors](restlet_auth.tba_errors.md).

The three-step flow, available since release 2019.2, gives an application a TBA access token without
the application ever seeing the user's password. The user signs in to NetSuite in a browser and
approves the application; the application then trades the outcome for a token.

Values in angle brackets (`<consumer_key>` and so on) are placeholders; this page names parameters
only and never shows real keys, secrets or tokens.

## Before you start

- Every application needs its own integration record.
- That integration record must hold the callback URL the application will use.
- The client must be able to open a browser and to receive the redirect to its callback.

When the client cannot open a browser or cannot handle a callback, use the
[issuetoken endpoint](restlet_auth.tba_issuetoken.md) instead.

During step two the user can sign in through the NetSuite login form, through a SAML single sign-on
identity provider, or through an OpenID Connect provider.

## Step 1: get a request token

Send a signed `POST` to `/rest/requesttoken` on the account's RESTlet domain. Since release 2020.1 the
system domain is accepted for this call too.

Header parameters:

| Parameter | Required | Notes |
|---|---|---|
| `oauth_consumer_key` | Yes | From the integration record |
| `oauth_signature_method` | Yes | See [TBA request signing](restlet_auth.tba_signing.md) |
| `oauth_signature` | Yes | Signed with the consumer secret only |
| `oauth_timestamp` | Yes | Seconds since the Unix epoch |
| `oauth_nonce` | Yes | See the nonce rules below |
| `oauth_callback` | Yes | Must match the callback URL on the integration record |
| `oauth_version` | No | When sent, the value is `1.0` |
| `realm` | No | The account ID |
| `role` | No | The role to authorize |

The callback you send has to match the one stored on the integration record. Since 2020.1 a wildcard is
allowed, but only for `localhost` with any port number.

The response returns `oauth_token`, `oauth_token_secret`, `oauth_callback_confirmed` and `role`. Keep
the token secret on the server side; step three needs it for signing.

### Nonce and timestamp rules

- `oauth_nonce` must be at least 6 characters long; 20 characters is the recommended length.
- Do not repeat a nonce with the same timestamp.
- `oauth_timestamp` counts seconds since the Unix epoch and must not be lower than the timestamps of
  your earlier requests.

> [!SOURCE-ERROR] The source's general nonce rule says the nonce should be 20 characters long, while its
> step-one description sets a minimum of 6. Treat 6 as the floor and 20 as the recommendation, and check
> in your account if a shorter nonce is rejected.
<!-- source-error: topic:restlet_auth.tba_flow | - | nonce_rule_says_length_should_be_20 -->

## Step 2: user consent in the browser

Send the user's browser with a `GET` to `authorizetoken.nl` on the account's application domain, with
these query parameters:

- `oauth_token`: the request token from step one.
- `state` (optional): up to 512 characters, letters and digits only, returned to you unchanged.

The user signs in and sees a consent page, where they may also switch to a different role. NetSuite then
redirects to your callback with `oauth_token`, `oauth_verifier`, `company`, `role`, `entity` and `state`.

- If the `state` on the redirect is not the one you sent, reject the redirect.
- If the user declines, `oauth_verifier` comes back empty.

## Step 3: exchange for an access token

Send a signed `POST` to `/rest/accesstoken` on the account's RESTlet domain, with these header
parameters: `oauth_consumer_key`, `oauth_token`, `oauth_signature_method`, `oauth_timestamp`,
`oauth_nonce`, `oauth_version`, `oauth_verifier`, `oauth_signature` and `realm`. Sign it with the
consumer secret together with the token secret from step one.

The response holds the access token (`oauth_token`) and its secret (`oauth_token_secret`). Use that pair
from now on to [sign RESTlet calls](restlet_auth.tba_signing.md).

## Integration record installation

Granting the token installs the integration record in the account automatically. If the account's
preference requires approval of auto-installed integrations, the record is put in the waiting-for-approval
state, and calls stay blocked until the integration's state is set to enabled. The preference and the
two possible states are described in [TBA token REST endpoints](restlet_auth.token_endpoints.md#integration-record-installation).

## Looking up who a token belongs to

A related endpoint returns the user information behind an access token: `GET /rest/tokeninfo`, callable
on the RESTlet domain or the system domain. Its JSON response includes the company name, company ID
(the account ID), role name, role ID and entity ID; see
[TBA token REST endpoints](restlet_auth.token_endpoints.md#tokeninfo). The source does not state which
parameters or header `tokeninfo` expects.
