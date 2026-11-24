# TBA errors
<!-- id: topic:restlet_auth.tba_errors -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [TBA request signing](restlet_auth.tba_signing.md), [TBA three-step authorization flow](restlet_auth.tba_flow.md),
[RESTlet HTTP status codes](../gen/catalogs/restlet_http_status_codes.md),
[RESTlet SuiteScript errors](../gen/catalogs/restlet_suitescript_errors.md).

## Where to look

When a TBA call is refused, open the login audit trail and read its **Detail** column; that is where
the reason is recorded. Add the token name and application name columns to the results so you can
tell which integration and which token the failed attempt came from.

## General rules behind most failures

- **Clock skew.** `oauth_timestamp` may differ from the server's time by at most 5 minutes.
- **One header per request.** Build a fresh header (new nonce, new timestamp, new signature) for every
  request. A header that has already been sent cannot be sent again.
- **Sandbox refresh.** Tokens are not copied into sandbox or release preview accounts; after a refresh,
  create the tokens again.
- **Lockout.** Five failed logins lock the user out for 30 minutes.
- **Resetting credentials.** Resetting an integration's credentials to fix one integration breaks every
  other integration that relies on them, so check what else uses them first.

## Errors during the authorization flow

| Message | Cause | Fix |
|---|---|---|
| `UnknownIntegration` | The consumer key is wrong, or no integration record has it | Check the consumer key against the integration record |
| `IntegrationBlocked` | The integration record's state is blocked | Have an administrator change the state |
| `AuthorizationFlowRequired` | The integration record does not have the authorization flow box checked | Enable the authorization flow on the integration record |
| `InvalidTimestamp` | The timestamp is more than 5 minutes off the server time | Synchronize the client clock |
| `InvalidSignature` | The signature does not match | Recheck the base string, key and encoding ([signing](restlet_auth.tba_signing.md)) |
| `InvalidCallback` | The callback is not the one on the integration record | Send exactly the callback URL configured on the record |
| `MissingRequiredParameter` | A required parameter is absent | Add the missing parameter |
| `NonceRejected` | The nonce is too short | Use a longer nonce |
| `NonceUsed` | The same nonce and timestamp pair was used before | Generate a new nonce for each request |
| `TokenRejected` | The token is unknown or inactive | Use a valid, active token |
| `EntityOrRoleDisabled` | The user or role cannot be used | Check that the entity and role are active and allowed |
| `FeatureDisabled` | Token-based authentication is not enabled in the account | Enable the feature |
| `UnknownAlgorithm` | The signature method is not HMAC-SHA256 | Sign with HMAC-SHA256 |
| `VersionRejected` | `oauth_version` is something other than `1.0` | Send `1.0` or leave the parameter out |
| `InvalidVerifier` | The verifier is not the one returned in step two | Pass the `oauth_verifier` from the step-two redirect unchanged |

## Errors when calling a RESTlet

The spec records these messages in the source's own spelling and letter case.

| Message | Cause | Fix |
|---|---|---|
| `consumer_key_refused` | The integration is blocked | Have an administrator unblock the integration |
| `consumer_key_unknown` | No integration matches the consumer key | Check the consumer key |
| `FeatureDisabled` | Token-based authentication is not enabled | Enable the feature |
| `MissingTokenPassportRequiredFields` | A required parameter is missing | Add the missing header parameter |
| `nonce_rejected` | The nonce is too short | Use a longer nonce |
| `nonce_used` | The nonce and timestamp were already used together | Generate a new nonce per request |
| `parameter_rejected` | A parameter is duplicated, malformed or empty | Send each parameter once, well formed and with a value |
| `permission_denied` | The user or role cannot be used | Check the entity, the role and its permissions |
| `InvalidSignature` | The signature does not match | Recheck the base string, key and encoding |
| `UnknownAlgorithm` | The signature method is not HMAC-SHA256 | Sign with HMAC-SHA256 |
| `temporary_locked` | The user is locked out after repeated failures | Wait out the 30-minute lockout, then fix the cause |
| `InvalidTimestamp` | The timestamp is more than 5 minutes off | Synchronize the client clock |
| `token_rejected` | The token is unknown or inactive | Use a valid, active token |
| `VersionRejected` | `oauth_version` is not `1.0` | Send `1.0` or omit it |

For errors that the RESTlet script returns once authentication has succeeded, see the
[RESTlet HTTP status codes](../gen/catalogs/restlet_http_status_codes.md) and
[RESTlet SuiteScript errors](../gen/catalogs/restlet_suitescript_errors.md) catalogs.
