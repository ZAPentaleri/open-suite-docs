# TBA issuetoken endpoint
<!-- id: topic:restlet_auth.tba_issuetoken -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [TBA token REST endpoints](restlet_auth.token_endpoints.md),
[RESTlet user-credential (NLAuth) authentication](restlet_auth.nlauth.md),
[TBA three-step authorization flow](restlet_auth.tba_flow.md), [TBA errors](restlet_auth.tba_errors.md).

The `issuetoken` endpoint lets an application create a TBA token programmatically, by authenticating
with a user's own credentials and receiving a token back. It is the route for clients that cannot run the
browser-based [three-step flow](restlet_auth.tba_flow.md). For new integrations, prefer the three-step
flow, and plan to move existing `issuetoken` integrations over to it (TBA as a whole is deprecated from
release 2027.1; see [RESTlet authentication](restlet_auth.md)).

> [!SOURCE-ERROR] In places the source spells the endpoint `issutoken` [sic]. Everywhere else it is
> `issuetoken`; use that spelling and treat the other as a typo.
<!-- source-error: topic:restlet_auth.tba_issuetoken | - | endpoint_name_written_issutoken -->

## Calling the endpoint

The request is `GET /rest/issuetoken` on the account's RESTlet domain or system domain, with the query
parameters `consumerKey` (required) and `name` (optional), URL-encoded. The new token is issued for the
role named in the NLAuth header. The full parameter list, the companion `revoketoken` and `tokeninfo`
endpoints, and what happens to the integration record are on
[TBA token REST endpoints](restlet_auth.token_endpoints.md).

## The NLAuth header fields

The request authenticates with an [NLAuth](restlet_auth.nlauth.md) header: the `Authorization` header,
whose value starts with `NLAuth` and a space and then lists comma-separated `name=value` pairs with no
spaces around the values. It can carry these fields:

| Field | What it carries |
|---|---|
| `nlauth_account` | The account ID |
| `nlauth_email` | The user's login email address |
| `nlauth_signature` | The user's password (supply it at run time from secure storage; never hard-code or log it) |
| `nlauth_role` | Internal ID of the role to use |
| `nlauth_application_id` | Application ID of the integration record |
| `nlauth_otp` | The current time-based verification code, when the role requires two-factor authentication |

Which fields are required and how the role is chosen when `nlauth_role` is absent are described on the
[NLAuth page](restlet_auth.nlauth.md).

Shape of the header, placeholders only:

```text
Authorization: NLAuth nlauth_account=<account_id>,nlauth_email=<encoded_login_email>,nlauth_signature=<password_from_secure_store>,nlauth_role=<role_internal_id>,nlauth_otp=<current_code>
```

Escape every field value with RFC 3986 percent-encoding; unescaped values make the login fail as an
invalid login attempt (`INVALID_LOGIN_ATTEMPT`).

`nlauth_otp` is accepted only by the `issuetoken` endpoint; do not send it with other requests.

> [!SOURCE-ERROR] The description of the header fields also uses the misspelt endpoint name
> `issutoken` [sic]. Rely on `issuetoken`.
<!-- source-error: topic:restlet_auth.tba_issuetoken | - | endpoint_name_written_issutoken -->

## Roles that require two-factor authentication

When the role requires 2FA, `nlauth_otp` becomes mandatory:

- The code is a time-based one-time password as defined in RFC 6238: 6 digits, with a 30-second period.
- It must come from an authenticator app. Codes sent by email, SMS or voice call, and backup codes, are
  not accepted.
- Each code can be used once. Two integrations running in parallel as the same user will collide over the
  same code. Give each integration its own user or role, or wait for the next code.
- Keeping an authenticator on the device that runs the integration undermines the protection that 2FA is
  meant to give.

> [!SOURCE-ERROR] The same source page says both that a code stays valid for about a minute and that the
> period is 30 seconds. Plan for 30 seconds and confirm the actual tolerance in your account.
<!-- source-error: topic:restlet_auth.tba_issuetoken | - | otp_validity_about_a_minute_vs_30_seconds -->

## Integration record installation

As with the three-step flow, granting a token installs the integration record automatically. If the
account requires approval of auto-installed integrations, the record is put in the waiting-for-approval
state and its requests are blocked until the state is set to enabled (see
[TBA token REST endpoints](restlet_auth.token_endpoints.md#integration-record-installation)).
