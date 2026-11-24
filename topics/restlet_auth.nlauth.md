# RESTlet user-credential (NLAuth) authentication
<!-- id: topic:restlet_auth.nlauth -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [TBA token REST endpoints](restlet_auth.token_endpoints.md),
[TBA issuetoken endpoint](restlet_auth.tba_issuetoken.md),
[Code grant flow for RESTlet callers](restlet_auth.oauth2_code_grant.md).

NLAuth is a NetSuite-specific scheme, used only with RESTlets, in which the caller puts a user's own
login details into the request header. You will mostly meet it in older integrations and as the
header for the [`issuetoken` endpoint](restlet_auth.token_endpoints.md).

## Status: legacy, ending in 2027.1

- Since release 2021.1, new RESTlets no longer accept NLAuth; a call that tries it gets an HTTP error.
- Support ends with release 2027.1. Integrations that still rely on NLAuth then stop working, so move
  them to OAuth 2.0.
- Build every new integration on OAuth 2.0 (see
  [Code grant flow for RESTlet callers](restlet_auth.oauth2_code_grant.md)). Callers outside NetSuite
  should authenticate with OAuth 2.0 or TBA.

## The header

NLAuth uses the standard `Authorization` header. Its value begins with the word `NLAuth` followed by a
space, and then lists the fields as `name=value` pairs separated by commas, with no spaces around the
values.

| Field | Required | Carries |
|---|---|---|
| `nlauth_account` | Yes | The ID of the account where the RESTlet is deployed |
| `nlauth_email` | Yes | The email address the user logs in with |
| `nlauth_signature` | Yes | The user's password |
| `nlauth_role` | No | The internal ID of the role to use |
| `nlauth_application_id` | No | The application ID of the integration record |
| `nlauth_otp` | No | The code from the user's two-factor authenticator app |

`nlauth_otp` is accepted only together with the `issuetoken` endpoint (see
[TBA token REST endpoints](restlet_auth.token_endpoints.md)).

Because the header contains a password, never hard-code it or write it to a log; the shape below uses
placeholders only:

```text
Authorization: NLAuth nlauth_account=<account_id>,nlauth_email=<encoded_login_email>,nlauth_signature=<password_from_secure_store>,nlauth_application_id=<application_id>
```

### Encoding

Special characters in the header values must be percent-encoded as RFC 5849 describes. If characters
that need escaping are sent unescaped, the login fails with `INVALID_LOGIN_ATTEMPT`.

## Always send the application ID

Although `nlauth_application_id` is optional, send it on every request. It links the request to its
integration record, which lets you:

- see the integration's details,
- block that application, and
- keep an execution log per application.

## Which role is used

If you leave out `nlauth_role`, the system chooses the role itself. Since release 2017.1 it tries these
in order and uses the first that applies:

1. The role given in the request.
2. The account's default role, if that role is not a Customer Center role.
3. The last role used that is not a Customer Center role.
4. The account's default role, if that role is a Customer Center role.
5. The last Customer Center role used.

The source notes that the step for a SOAP web services default role is not part of this sequence.
When the request should run under a Customer Center role, name that role explicitly in `nlauth_role`
rather than relying on the fallback order.
