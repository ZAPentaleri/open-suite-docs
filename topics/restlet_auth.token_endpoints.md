# TBA token REST endpoints
<!-- id: topic:restlet_auth.token_endpoints -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [TBA issuetoken endpoint](restlet_auth.tba_issuetoken.md),
[TBA three-step authorization flow](restlet_auth.tba_flow.md),
[RESTlet user-credential (NLAuth) authentication](restlet_auth.nlauth.md).

Three small REST resources let users look after their own token-based authentication (TBA) tokens
without opening the UI:

| Path | Purpose |
|---|---|
| `/rest/issuetoken` | Issue a token for yourself |
| `/rest/revoketoken` | Revoke one of your own tokens |
| `/rest/tokeninfo` | Read the user information behind an access token |

Every one of them is called with HTTP `GET`. They are reachable on the account's RESTlet domain and on its
system domain, and account-specific domains are supported for them.

Values in angle brackets on this page are placeholders. Keep real keys, tokens and passwords out of code,
logs and documentation.

## Endpoints versus the UI

Instead of calling `issuetoken`, you can also create a token by hand in the UI. The endpoints only work
on the caller's own tokens: issuing or revoking a token that belongs to another user is not possible
through them, and other users' tokens can only be handled in the UI.

Administrators are not an exception. Whether they use the `Manage Access Tokens` link in the Settings
portlet or the token endpoint, they can only create tokens for themselves. TBA remains usable for an
integration that has to run with the Administrator role.

## Query parameters

URL-encode the query parameter values; a token name with spaces in it is the usual case where this
matters.

### issuetoken

| Parameter | Required |
|---|---|
| `consumerKey` | Yes |
| `name` | No |

The call authenticates with an [NLAuth](restlet_auth.nlauth.md) header, and the token it returns is bound
to the role named in that NLAuth header. For roles that require two-factor authentication, the header can
also carry the optional `nlauth_otp` field; the rules for that one-time code are on
[TBA issuetoken endpoint](restlet_auth.tba_issuetoken.md).

Request shape, placeholders only (the NLAuth field syntax is described on
[RESTlet user-credential (NLAuth) authentication](restlet_auth.nlauth.md)):

```text
GET /rest/issuetoken?consumerKey=<consumer_key>&name=<url_encoded_token_name>
Authorization: NLAuth nlauth_account=<account_id>,nlauth_email=<encoded_login_email>,nlauth_signature=<password_from_secure_store>,nlauth_role=<role_internal_id>
```

### revoketoken

| Parameter | Required |
|---|---|
| `consumerKey` | Yes |
| `token` | Yes |

This endpoint accepts either an `NLAuth` header or an `OAuth` (TBA-signed) header; see
[TBA request signing](restlet_auth.tba_signing.md) for the latter.

### tokeninfo

Returns JSON describing the user that an access token belongs to. The response contains at least these
items (the source says the list is not complete):

- `Company Name`
- `Company ID` (the account ID)
- `Role Name`
- `Role ID`
- `Entity ID`

The source does not state which parameters `tokeninfo` takes or which authorization header it expects.

## Integration record installation

When a token is obtained through the [three-step flow](restlet_auth.tba_flow.md) or through the
`issuetoken` endpoint, NetSuite creates an integration record and installs it in the account
automatically. The state of that integration depends on the preference
`Require Approval during Auto-Installation of Integration`, which is on the
`SOAP Web Services Preferences` page (reached from the integration area of the Setup menu):

| `Require Approval during Auto-Installation of Integration` | State of the auto-installed integration |
|---|---|
| Not checked | `Enabled` |
| Checked | `Waiting for Approval` |

While the integration is `Waiting for Approval`, its requests are blocked. They go through only after
someone changes the state to `Enabled`.
