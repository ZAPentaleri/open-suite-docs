# N/encode
<!-- id: N/encode -->

`N/encode` re-expresses a string from one encoding to another: plain UTF-8 text to Base64 or hex,
Base64 back to text, and so on. Its [`Encoding`](#encodeencoding) enum is also the vocabulary that
[N/crypto](N_crypto.md) uses for every input and output encoding option.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/encode'], (encode) => { ... })`
- Related: [N/crypto](N_crypto.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`encode.convert(options)`](#encodeconvertoptions) | function | Convert a string between encodings |
| [`encode.Encoding`](#encodeencoding) | enum | Supported encodings |

## Functions

### encode.convert(options)
<!-- id: N/encode.convert -->

Takes a string written in one encoding and returns the same content written in another.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.inputEncoding` | string | Yes | |
| `options.outputEncoding` | string | Yes | |
| `options.string` | string | Yes | |

- `options.inputEncoding`: how `options.string` is currently encoded; a value from
  [`encode.Encoding`](#encodeencoding).
- `options.outputEncoding`: the encoding you want back; a value from
  [`encode.Encoding`](#encodeencoding). If you ask for UTF-8 output but the decoded bytes are not
  valid UTF-8, the result contains replacement characters rather than failing.
- `options.string`: the text to convert.

**Returns:** `string`, the re-encoded text.

**Errors:**
- `FAILED_TO_DECODE_STRING_ENCODED_BINARY_DATA_USING_1_ENCODING`: `options.string` cannot be
  decoded with the stated `options.inputEncoding` (malformed input).
- `SSS_MISSING_REQD_ARGUMENT`: `options.string`, `options.inputEncoding` or
  `options.outputEncoding` was not supplied.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.1

## Enums

### encode.Encoding
<!-- id: N/encode.Encoding -->

The encodings understood by `N/encode` and by the encoding options of `N/crypto`. Since 2015.1.

| Value | Notes |
|---|---|
| `UTF_8` | Ordinary text |
| `BASE_16` | |
| `BASE_32` | |
| `BASE_64` | Standard Base64 |
| `BASE_64_URL_SAFE` | Base64 with URL-friendly characters (see below) |
| `HEX` | |

`BASE_64_URL_SAFE` differs from `BASE_64` in its alphabet: `+` becomes `-` and `/` becomes `_`.
The `=` padding characters are **kept**, though. If the receiver wants unpadded output, or the
value goes into a URL where `=` is a problem, strip the padding with a string replace or
URL-encode it yourself.

## Examples

Building a URL-safe token from a JSON payload and stripping the trailing padding, which
`BASE_64_URL_SAFE` keeps:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/encode'], (encode) => {
  const toUrlToken = (payload) => {
    const b64 = encode.convert({
      string: JSON.stringify(payload),
      inputEncoding: encode.Encoding.UTF_8,
      outputEncoding: encode.Encoding.BASE_64_URL_SAFE
    });
    return b64.replace(/=+$/, '');
  };

  return { toUrlToken };
});
```

A RESTlet that receives a Base64-encoded CSV line from a scanner integration and decodes it to text
before splitting it:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/encode'], (encode) => {
  const post = (body) => {
    const csvLine = encode.convert({
      string: body.data,
      inputEncoding: encode.Encoding.BASE_64,
      outputEncoding: encode.Encoding.UTF_8
    });
    const [sku, qty, bin] = csvLine.split(',');
    return { sku, qty: Number(qty), bin };
  };

  return { post };
});
```
