# TBA request signing
<!-- id: topic:restlet_auth.tba_signing -->

Part of: [RESTlet authentication](restlet_auth.md)

Related: [TBA three-step authorization flow](restlet_auth.tba_flow.md), [TBA errors](restlet_auth.tba_errors.md),
[Restlet script type](../script-types/Restlet.md).

Token-based authentication (TBA) is built on OAuth 1.0. The caller computes a signature from the request
itself and sends it in the `Authorization` header; NetSuite computes the same signature on its side and
refuses the call if the two disagree. This page walks through the pieces in the order you put them
together.

Anything in angle brackets, such as `<consumer_key>`, is a placeholder for your own value. Never write real
keys, secrets or tokens into code, logs or documentation.

## Algorithm

- The signature method is **HMAC-SHA256**. HMAC-SHA1 is no longer supported, starting with release
  2023.1.
- The signature value is the HMAC-SHA256 digest encoded in Base64. That Base64 text is then
  percent-encoded once more before it goes into the header.

## Percent-encoding

"Encode" on this page always means RFC 3986 percent-encoding of the UTF-8 bytes:

- unreserved characters are left unchanged;
- every other byte is written as `%` plus two **upper-case** hexadecimal digits;
- a space turns into `%20`, never into `+`.

## Building the signature base string

The base string consists of three parts, in this order, joined by `&`:

1. the HTTP method, upper-cased;
2. the base URI, encoded;
3. the normalized parameter string, encoded.

### The base URI

Take the request URL, drop its query string, and lower-case the scheme and the host.

### Collecting the parameters

The normalized parameters come from three sources: the OAuth values carried in the `Authorization`
header, the request body, and the URL's query string. Two values are excluded: `realm` and
`oauth_signature`. In particular, `realm` never becomes part of the base string. Query-string parameters
are decoded first and then handled like all the others.

A RESTlet cannot receive a POST body sent as `application/x-www-form-urlencoded`, and body parameters
only enter the base string for that content type. In practice, a RESTlet base string is built from the
OAuth values plus the URL's query parameters (`script`, `deploy` and any others you append).

### Normalizing

1. Encode each parameter name and each value.
2. Sort the pairs by name; when two names match, sort those pairs by value. Compare by byte order.
3. Join each name and value with `=` (keep the `=` even if the value is empty), then join the pairs with
   `&`.
4. Encode the resulting string again. This is the third part of the base string.

### Which OAuth values go in

| Request | OAuth parameters in the base string (besides query parameters) |
|---|---|
| A RESTlet call | `oauth_consumer_key`, `oauth_nonce`, `oauth_signature_method`, `oauth_timestamp`, `oauth_token`, `oauth_version` |
| Step one of the authorization flow (request token) | `oauth_callback`, `oauth_consumer_key`, `oauth_nonce`, `oauth_signature_method`, `oauth_timestamp`, `oauth_version`, `role` |
| Step three of the authorization flow (access token) | `oauth_consumer_key`, `oauth_token`, `oauth_signature_method`, `oauth_timestamp`, `oauth_nonce`, `oauth_version`, `oauth_verifier` |

The steps of the authorization flow are explained on
[TBA three-step authorization flow](restlet_auth.tba_flow.md).

## The signing key

The key is the encoded consumer secret, an `&`, and the encoded token secret:
`<encoded consumer_secret>&<encoded token_secret>`.

- A RESTlet call and step three of the authorization flow use both secrets.
- In step one no token exists yet, so the token secret is empty and the key is effectively built from the
  consumer secret alone.

## Same recipe, different channels

Signing works the same way for every NetSuite API channel. The only thing that changes from one channel
to another is the base string, because it is derived from the particular request being sent.

> [!SOURCE-ERROR] One worked example in the source is labelled as a RESTlet request but looks as if it was
> copied from the REST web services material. For RESTlets, build the base string from the RESTlet URL and
> parameters described on this page rather than from that example.
<!-- source-error: topic:restlet_auth.tba_signing | - | rest_web_services_example_labelled_restlets -->

## What a RESTlet request looks like

The URL is made of:

| Part | Value |
|---|---|
| Host | The account's RESTlet domain |
| Path | `/app/site/hosting/restlet.nl` |
| Query parameters | `script` and `deploy`, which identify the script and its deployment |

The `Authorization` header uses the `OAuth` scheme and carries the parameters below. Each value is
percent-encoded. Write each parameter as `name="value"` (the value in double quotes) and separate the
pairs with a comma followed by a space.

| Header parameter | Content |
|---|---|
| `realm` | The account ID, also called the company ID |
| `oauth_consumer_key` | Consumer key of the integration record |
| `oauth_token` | The token ID |
| `oauth_nonce` | A nonce; its length and uniqueness rules are on [TBA three-step authorization flow](restlet_auth.tba_flow.md) |
| `oauth_timestamp` | Seconds since the Unix epoch (see [TBA three-step authorization flow](restlet_auth.tba_flow.md)) |
| `oauth_signature_method` | `HMAC-SHA256` |
| `oauth_version` | `1.0` (see [TBA three-step authorization flow](restlet_auth.tba_flow.md)) |
| `oauth_signature` | The computed signature, Base64-encoded and then percent-encoded |

Shape of the header, placeholders only:

```text
Authorization: OAuth realm="<account_id>", oauth_consumer_key="<consumer_key>", oauth_token="<token_id>", oauth_nonce="<nonce>", oauth_timestamp="<epoch_seconds>", oauth_signature_method="HMAC-SHA256", oauth_version="1.0", oauth_signature="<encoded_signature>"
```
