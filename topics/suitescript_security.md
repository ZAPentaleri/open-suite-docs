# SuiteScript Security Considerations
<!-- id: topic:suitescript_security -->

Part of: [Developer guide](developer_guide.md)

See also: [SuiteScript Best Practices](suitescript_best_practices.md), [N/auth](../modules/N_auth.md),
[N/https](../modules/N_https.md), [N/query](../modules/N_query.md).

Security guidance for scripts: how to handle credentials, what external users' data a script can
and cannot see, and which features widen your exposure.

## Credentials and secrets

- Never hard-code passwords in a script.
- Prefer OAuth 2.0 to user name and password credentials.
- Where an API accepts a credential reference (a GUID), use it rather than a plain-text secret.
- Do not store confidential data in script parameters. Parameter values can end up indexed by search
  engines.
- Two API areas can expose secrets in plain text if you are careless:
  - the [N/auth](../modules/N_auth.md) functions for changing an email address or password accept
    the password as plain text;
  - an [N/https](../modules/N_https.md) request can carry credentials in plain text.
- Some HTTP headers cannot be set by scripts; see
  [catalog:http_blocked_headers](../gen/catalogs/http_blocked_headers.md).

## Running with elevated rights

Run a script as Administrator only when it truly needs that role.

## Transport

RESTlets require TLS.

## External users and sensitive fields

External-role users are shoppers, people filling in online forms, and Customer Center users. When
such a user saves a record, a `beforeSubmit` script cannot read these fields:

- `password`
- `password2`
- `ccnumber`
- `ccsecuritycode`

> [!SOURCE-ERROR] The source prints the card number field in this list as `ccunumber`, which looks
> like a typo for `ccnumber`. Use `ccnumber`, and confirm the field ID on the record if it matters.
<!-- source-error: topic:suitescript_security | - | card_number_field_printed_ccunumber -->

A query can return at most the encrypted form of a card number, never the clear value.

## Custom pages

- If inline HTML loads external content, protect it with Subresource Integrity.
- Mark a Suitelet as available without login only when that is really needed.
