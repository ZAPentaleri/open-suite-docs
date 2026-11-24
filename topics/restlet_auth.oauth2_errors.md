# OAuth 2.0 errors
<!-- id: topic:restlet_auth.oauth2_errors -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [Code grant flow for RESTlet callers](restlet_auth.oauth2_code_grant.md),
[Machine-to-machine token flow](restlet_auth.oauth2_m2m.md), [OAuth 2.0 for SuiteScript](oauth2.md),
[RESTlet HTTP status codes](../gen/catalogs/restlet_http_status_codes.md),
[RESTlet SuiteScript errors](../gen/catalogs/restlet_suitescript_errors.md).

OAuth 2.0 failures show up in two places. The client sees an error in the HTTP exchange (an `error`
parameter on a redirect, a JSON body, or a `WWW-Authenticate` header). The underlying reason is recorded
in the login audit trail, in its **Detail** column; add the application name column to the audit trail
results so you can tell which integration failed.

Where the source gives no cause or fix for an entry, the tables below say "not stated".

## Errors on the authorize step

These reach the client as the `error` parameter on the redirect:

| `error` value | Cause |
|---|---|
| `invalid_request` | A required parameter is missing |
| `unauthorized_client` | The client is not known |
| `access_denied` | The user refused consent |
| `unsupported_response_type` | Not stated |
| `invalid_scope` | Not stated |

No redirect happens at all when the redirect URI is not one registered on the integration record, or when
the client is unknown.

> [!SOURCE-ERROR] Where the source discusses cross-site request forgery it writes the abbreviation as
> "CSFR" instead of CSRF. Only the spelling is affected.
<!-- source-error: topic:restlet_auth.oauth2_errors | - | csrf_written_csfr -->

## Errors from the token endpoint

The response body is JSON with an `error` field.

| `error` value | HTTP status | Cause |
|---|---|---|
| `invalid_request` | 400 | Not stated beyond the status |
| `invalid_client` | 401, with a Basic challenge whose realm is the account | Not stated beyond the status |
| `invalid_grant` | 400 | The authorization code or token is invalid, or the PKCE verifier does not match |
| `unauthorized_client` | Not stated | The client is not allowed to use this grant type |
| `unsupported_grant_type` | 400 | Not stated beyond the status |
| `invalid_scope` | 400 | Not stated beyond the status |

> [!SOURCE-ERROR] The source's description of `unsupported_grant_type` names only the authorization code
> and refresh token grant types, leaving out client credentials, which the
> [machine-to-machine flow](restlet_auth.oauth2_m2m.md) uses. Treat that list as incomplete.
<!-- source-error: topic:restlet_auth.oauth2_errors | - | unsupported_grant_type_omits_client_credentials -->

## Errors when calling the RESTlet

A rejected bearer token comes back with a `WWW-Authenticate` header whose realm is the account ID:

| Error | HTTP status | Cause |
|---|---|---|
| `invalid_request` | 400 | The request is malformed |
| `invalid_token` | 401 | The access token has expired or is invalid |

The login audit trail then shows one of these details:

| Detail | Cause | Fix |
|---|---|---|
| `AccessTokenExpired` | The access token has expired | Get a new one with the refresh token; if that is not possible, run the code grant again |
| `EntityOrRoleDisabled` | The entity, contact or role is invalid or inactive | Not stated |
| `InvalidSignature` | The signature is invalid | Use the correct public key |
| `TokenRejected` | The call tried to log in with a refresh token | Use the refresh token only in a refresh request |
| `InvalidIntegration` | The integration's application ID is invalid | Not stated |
| `ScopeMismatched` | The scope is empty, or the token's scope is not enabled on the integration record | Check at least one scope box on the integration record |
| `AuthorizationCodeGrantRequired` | The integration is not set up for OAuth 2.0 | Check the authorization code grant box on the integration record |
| `InvalidScope` | The token carries an empty scope | Not stated |
| `FeatureDisabled` | The OAuth 2.0 feature is not enabled | Not stated |
| `IntegrationBlocked` | The integration record is blocked | Set the integration's state to enabled |
| `UnsupportedAlgo` | The signing algorithm is not supported | Not stated |
| `TokenValidityTooBroad` | The validity end is too far in the future | Keep machine-to-machine validity within two hours (see the callout below) |
| `TokenBlocked` | The access or refresh token is blocked | Start again with fresh tokens |
| `InvalidTokenType` | Access and refresh tokens were swapped | Not stated |
| `SignatureValidation` | The client credentials signature is incorrect | Not stated |
| `ClientIdMismatch` | The client ID differs between the steps of the grant | Not stated |
| `TokenNotAcceptable` | The nonce is wrong, or the token claims to be issued in the future | Not stated |
| `TokenExpired` | A token from any of the flows has expired | Start again with fresh tokens |
| `InvalidIssuer` | The token's issuer is incorrect | Not stated |
| `MissingKeyId` | The token header has no `kid` | Not stated |
| `MissingKey` | The `kid` matches no registered certificate | Not stated |

> [!SOURCE-ERROR] The fix for `TokenValidityTooBroad` speaks of a machine-to-machine validity of up to two
> hours, while the assertion rules allow at most 60 minutes between issue and expiry. Keep assertions within
> 60 minutes; the source does not settle which figure is right.
<!-- source-error: topic:restlet_auth.oauth2_errors | - | m2m_token_validity_two_hours_vs_60_minute_assertion_limit -->

## Audit-trail details during the code grant

| Detail | When it happens |
|---|---|
| `ScopeMismatched` | Not stated |
| `AuthorizationCodeGrantRequired` | Not stated |
| `EntityOrRoleDisabled` | At step two |
| `InvalidState` | At step one |
| `UnknownIntegration` | At step one, when the client is wrong |
| `ClientAuthenticationFailed` | At step two, when the client is wrong |
| `InvalidRedirectURI` | Not stated |
| `UnsupportedResponseType` | Not stated |
| `AuthorizationExplicitlyDenied` | Not stated |
| `InvalidGrantType` | Not stated |
| `FeatureDisabled` | Not stated |
| `IntegrationBlocked` | Not stated |
| `InvalidRequest` | The PKCE parameters are malformed |
| `InvalidGrant` | The code verifier does not match |
| `ClientIdMismatch` | Not stated |

> [!SOURCE-ERROR] The code grant error material also spells cross-site request forgery as "CSFR"; read it
> as CSRF.
<!-- source-error: topic:restlet_auth.oauth2_errors | - | csrf_written_csfr -->

## Audit-trail details during a refresh

| Detail | Cause and fix |
|---|---|
| `RefreshTokenExpired` | The refresh token has expired; start the code grant again |
| `EntityOrRoleDisabled` | Not stated |
| `InvalidSignature` | Not stated |
| `InvalidIntegration` | Not stated |
| `ScopeMismatched` | Not stated |
| `AuthorizationCodeGrantRequired` | Not stated |
| `InvalidScope` | Not stated |
| `ClientAuthenticationFailed` | Not stated |
| `InvalidGrantType` | Not stated |
| `InvalidRefreshToken` | An access token was sent instead of the refresh token |
| `FeatureDisabled` | Not stated |
| `IntegrationBlocked` | Not stated |
| `MissingKeyId` | Not stated |
| `MissingKey` | Not stated |
| `TokenNotAcceptable` | Not stated |

> [!SOURCE-ERROR] For an expired refresh token the source's suggested fix is to use the refresh token,
> which looks copied from the expired-access-token row. An expired refresh token cannot be reused; start the
> authorization code grant again.
<!-- source-error: topic:restlet_auth.oauth2_errors | - | refresh_expired_resolution_says_use_refresh_token -->
