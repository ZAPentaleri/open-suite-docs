# N/crypto
<!-- id: N/crypto -->

`N/crypto` does three server-side jobs: hashing data, computing keyed HMACs, and symmetric (AES)
encryption and decryption. Key material never appears in your script: you work with a
[`SecretKey`](#secretkey-object) handle that points at a key or secret stored inside NetSuite. The
module also offers [`checkPasswordField`](#cryptocheckpasswordfieldoptions), which compares a value a
user typed with the password stored in a record's password field.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/crypto'], (crypto) => { ... })`
- Loading `N/crypto` also loads [N/encode](N_encode.md) as a side effect. The encoding options
  throughout this module take values from [`encode.Encoding`](N_encode.md#encodeencoding).
- API secrets that a `SecretKey` can refer to (through `options.secret` of `createSecretKey`) are
  managed in the UI on the company API secrets setup page (Setup, then Company, then API Secrets).
- Related: [N/crypto/certificate](N_crypto_certificate.md), [N/crypto/random](N_crypto_random.md),
  [N/keyControl](N_keyControl.md), [N/encode](N_encode.md).

> [!SOURCE-ERROR] The encoding parameters of this module are typed inconsistently in the source:
> some are typed `enum`, others `string`, although all take `encode.Encoding` values. Pass an
> `encode.Encoding` value in every case; the tables below keep each type exactly as the source gives it.
<!-- source-error: N/crypto | - | encoding_param_type_enum_vs_string_across_members -->

## Members

| Member | Kind | Summary |
|---|---|---|
| [`crypto.checkPasswordField(options)`](#cryptocheckpasswordfieldoptions) | function | Compare a typed value with a stored password field |
| [`crypto.createHash(options)`](#cryptocreatehashoptions) | function | Start a hash computation |
| [`crypto.createHmac(options)`](#cryptocreatehmacoptions) | function | Start an HMAC computation with a secret key |
| [`crypto.createCipher(options)`](#cryptocreatecipheroptions) | function | Start an AES encryption |
| [`crypto.createDecipher(options)`](#cryptocreatedecipheroptions) | function | Start an AES decryption |
| [`crypto.createSecretKey(options)`](#cryptocreatesecretkeyoptions) | function | Get a handle to a key stored in NetSuite |
| [`SecretKey`](#secretkey-object) | object | Handle to a stored key |
| [`Cipher`](#cipher-object) | object | Encrypts data |
| [`CipherPayload`](#cipherpayload-object) | object | Result of an encryption |
| [`Decipher`](#decipher-object) | object | Decrypts data |
| [`Hash`](#hash-object) | object | Computes a hash |
| [`Hmac`](#hmac-object) | object | Computes an HMAC |
| [`crypto.HashAlg`](#cryptohashalg) | enum | Hash algorithms |
| [`crypto.Padding`](#cryptopadding) | enum | Cipher padding modes |
| [`crypto.EncryptionAlg`](#cryptoencryptionalg) | enum | Encryption algorithms |

## Functions

### crypto.checkPasswordField(options)
<!-- id: N/crypto.checkPasswordField -->

Tells you whether a candidate value matches the password held in a password field of a saved
record. For a custom password field, call this function instead of reading the field with
[`Record.getValue`](N_record.md) or [`CurrentRecord.getValue`](N_currentRecord.md): the comparison
happens without your script handling the stored password, which is the more secure approach.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.line` | number | No | |
| `options.fieldId` | string | Yes | |
| `options.recordId` | number | Yes | |
| `options.recordType` | string | Yes | |
| `options.sublistId` | string | No | |
| `options.value` | string | Yes | |

- `options.line`: only used when the password field sits on a sublist line; line numbers start at 0.
- `options.sublistId`: the sublist that holds the password field, for that same sublist case.
- `options.value`: the text the user entered.
- Every option field exists since 2021.1.

**Returns:** `boolean`: `true` when the entered value matches the stored password, otherwise
`false`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2021.1

### crypto.createHash(options)
<!-- id: N/crypto.createHash -->

Creates a [`Hash`](#hash-object) object for the chosen algorithm. Feed it data with `update` and
read the result with `digest`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |

- `options.algorithm`: a value from [`crypto.HashAlg`](#cryptohashalg).

**Returns:** `crypto.Hash`, a new hash object.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### crypto.createHmac(options)
<!-- id: N/crypto.createHmac -->

Creates an [`Hmac`](#hmac-object) object that computes a keyed hash with the given algorithm and
key.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.key` | object | Yes | |

- `options.algorithm`: a value from [`crypto.HashAlg`](#cryptohashalg).
- `options.key`: a [`SecretKey`](#secretkey-object) object, as returned by
  [`crypto.createSecretKey`](#cryptocreatesecretkeyoptions).

**Returns:** `crypto.Hmac`, a new HMAC object.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### crypto.createCipher(options)
<!-- id: N/crypto.createCipher -->

Creates a [`Cipher`](#cipher-object) that encrypts data. The block cipher mode is always CBC; it is
applied automatically and you do not choose it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.key` | object | Yes | |
| `options.padding` | string | No | `PKCS5Padding` |

- `options.algorithm`: a value from [`crypto.EncryptionAlg`](#cryptoencryptionalg).
- `options.key`: a [`SecretKey`](#secretkey-object) object. For AES, the key text must be 16, 24
  or 32 characters long.
- `options.padding`: a value from [`crypto.Padding`](#cryptopadding).

**Returns:** `crypto.Cipher`, a new cipher object.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### crypto.createDecipher(options)
<!-- id: N/crypto.createDecipher -->

Creates a [`Decipher`](#decipher-object) that reverses an encryption made with `createCipher`.
Like the cipher, it always uses CBC mode, applied automatically.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.key` | object | Yes | |
| `options.iv` | string | Yes | |
| `options.padding` | object | No | |

- `options.algorithm`: a value from [`crypto.EncryptionAlg`](#cryptoencryptionalg).

> [!SOURCE-ERROR] The source describes this parameter as a hash algorithm, yet it takes a value from
> `crypto.EncryptionAlg`, the same as `createCipher`. Pass the encryption algorithm used to encrypt.
<!-- source-error: N/crypto.createDecipher | options.algorithm | described_as_hash_algorithm -->

- `options.key`: a [`SecretKey`](#secretkey-object) object, the same key used for encryption. For
  AES, the key text must be 16, 24 or 32 characters long.
- `options.iv`: the initialization vector, taken from the
  [`CipherPayload.iv`](#cipherpayloadiv) that the encryption produced.
- `options.padding`: a value from [`crypto.Padding`](#cryptopadding). No default is stated.

> [!SOURCE-ERROR] Here the source types `padding` as `object`, while `createCipher` types the same
> option as `string`. Both take a `crypto.Padding` value; verify which form the runtime expects, and
> use the same padding you encrypted with.
<!-- source-error: N/crypto.createDecipher | options.padding | padding_typed_object_vs_string_in_createCipher -->

**Returns:** `crypto.Decipher`, a new decipher object.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### crypto.createSecretKey(options)
<!-- id: N/crypto.createSecretKey -->

Returns a [`SecretKey`](#secretkey-object) handle pointing at key material stored in NetSuite. You
identify the key in one of two ways: by a GUID, or by the script ID of an API secret (the secrets
kept on the company API secrets setup page). A GUID is produced by a credential field added with
[`Form.addCredentialField`](N_ui_serverWidget.md) and resolves to the stored data or its metadata.
For AES use, the secret text must be 16, 24 or 32 characters long.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.guid` | string | No | |
| `options.secret` | string | No | |
| `options.encoding` | enum | No | `HEX` |

- `options.guid` and `options.secret` are each conditionally required: give one of them, and never
  both, because they are mutually exclusive. `options.secret` exists since 2021.1; the other fields
  since 2015.2.
- `options.secret`: the script ID of an API secret stored on the company API secrets setup page.
- `options.encoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding); when omitted,
  `encode.Encoding.HEX` is used.

> [!SOURCE-ERROR] The source's note on the secret parameter mentions a `passwordGuid`, which is not a
> parameter of this function. Use `options.guid` or `options.secret` as listed above.
<!-- source-error: N/crypto.createSecretKey | - | secret_param_note_mentions_passwordguid -->

**Returns:** `crypto.SecretKey`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Objects

### SecretKey (object)
<!-- id: N/crypto.SecretKey -->

A handle to a key stored inside NetSuite. It does not hold the key value itself; to locate the key
it needs either a GUID or an API secret script ID. Created by
[`crypto.createSecretKey(options)`](#cryptocreatesecretkeyoptions) and passed as `options.key` to
`createHmac`, `createCipher` and `createDecipher`. Server scripts; since 2015.2.

> [!SOURCE-ERROR] The source's page title spells the object `Secretkey` [sic] with a lowercase k,
> while the name used everywhere else is `SecretKey`. Use `SecretKey`; the source does not say which
> spelling the runtime reports.
<!-- source-error: N/crypto.SecretKey | - | page_title_spelled_secretkey_lowercase_k -->

#### SecretKey.secret
<!-- id: N/crypto.SecretKey#secret -->

`string`. The script ID of the API secret this handle refers to. Since 2021.1.

#### SecretKey.encoding
<!-- id: N/crypto.SecretKey#encoding -->

`string`. The encoding of the clear-text key. Since 2015.2.

#### SecretKey.guid
<!-- id: N/crypto.SecretKey#guid -->

`string`. The GUID that identifies the stored key. Since 2015.2.

### Cipher (object)
<!-- id: N/crypto.Cipher -->

Encrypts data. Obtain one from [`crypto.createCipher(options)`](#cryptocreatecipheroptions), add
clear text with `update`, then call `final` to get the encrypted result. Server scripts; since
2015.2.

#### Cipher.update(options)
<!-- id: N/crypto.Cipher#update -->

Adds clear data to the encryption.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.inputEncoding` | enum | No | `UTF_8` |
| `options.input` | string | Yes | |

- `options.inputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding) describing
  how `options.input` is encoded.

> [!SOURCE-ERROR] The source writes this default as `UTF-8` with a hyphen, which is not an
> `encode.Encoding` key; the key is `UTF_8`. Pass `encode.Encoding.UTF_8` explicitly to be safe.
<!-- source-error: N/crypto.Cipher#update | options.inputEncoding | default_written_utf_dash_8_not_enum_key_utf_8 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### Cipher.final(options)
<!-- id: N/crypto.Cipher#final -->

Finishes the encryption and returns a [`CipherPayload`](#cipherpayload-object) with the cipher text
and the initialization vector.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.outputEncoding` | enum | No | `HEX` |

- `options.outputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding) for the
  cipher text.

**Returns:** `crypto.CipherPayload`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### CipherPayload (object)
<!-- id: N/crypto.CipherPayload -->

The result of an encryption, returned by [`Cipher.final`](#cipherfinaloptions). Server scripts;
since 2015.2.

#### CipherPayload.ciphertext
<!-- id: N/crypto.CipherPayload#ciphertext -->

`string`. The encrypted text, ready to be stored or sent. Since 2015.2.

#### CipherPayload.iv
<!-- id: N/crypto.CipherPayload#iv -->

`string`. The initialization vector used for this encryption. Keep it with the cipher text: it is
what you pass as `options.iv` to [`crypto.createDecipher`](#cryptocreatedecipheroptions). It is
always HEX, whatever encoding options you chose. Since 2015.2.

> [!SOURCE-ERROR] The source ties this value to an `inputEncoding` option, but `Cipher.final` takes
> `outputEncoding`. Either way, expect the IV in HEX.
<!-- source-error: N/crypto.CipherPayload#iv | - | names_inputencoding_but_cipher_final_takes_outputencoding -->

### Decipher (object)
<!-- id: N/crypto.Decipher -->

Decrypts data. Obtain one from [`crypto.createDecipher(options)`](#cryptocreatedecipheroptions),
pass cipher text with `update`, then read the clear text from `final`. Server scripts; since 2015.2.

#### Decipher.update(options)
<!-- id: N/crypto.Decipher#update -->

Adds cipher data to be decrypted.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.inputEncoding` | string | No | `HEX` |
| `options.input` | string | Yes | |

- `options.inputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding).

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### Decipher.final(options)
<!-- id: N/crypto.Decipher#final -->

Completes the decryption and returns the clear data as a string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.outputEncoding` | string | No | `UTF_8` |

- `options.outputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding).

> [!SOURCE-ERROR] The source writes this default as `UTF-8` with a hyphen rather than the enum key
> `UTF_8`. Pass `encode.Encoding.UTF_8` explicitly.
<!-- source-error: N/crypto.Decipher#final | options.outputEncoding | default_written_utf_dash_8_not_enum_key_utf_8 -->

**Returns:** `string`, the decrypted clear data.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### Hash (object)
<!-- id: N/crypto.Hash -->

Computes a hash. Created by [`crypto.createHash(options)`](#cryptocreatehashoptions). Server
scripts; since 2015.2.

#### Hash.update(options)
<!-- id: N/crypto.Hash#update -->

Adds data to the hash computation.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.inputEncoding` | string | No | `UTF_8` |
| `options.input` | string | Yes | |

- `options.inputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding).

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### Hash.digest(options)
<!-- id: N/crypto.Hash#digest -->

Finishes the computation and returns the hash.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.outputEncoding` | string | No | `HEX` |

- `options.outputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding).

**Returns:** `string`, the hash.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### Hmac (object)
<!-- id: N/crypto.Hmac -->

Computes an HMAC. Created by [`crypto.createHmac(options)`](#cryptocreatehmacoptions). Server
scripts; since 2015.2.

#### Hmac.update(options)
<!-- id: N/crypto.Hmac#update -->

Adds data to the HMAC computation.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.inputEncoding` | enum | No | `UTF_8` |
| `options.input` | string | Yes | |

- `options.inputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding).

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### Hmac.digest(options)
<!-- id: N/crypto.Hmac#digest -->

Finishes the computation and returns the HMAC.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.outputEncoding` | string | No | `HEX` |

- `options.outputEncoding`: a value from [`encode.Encoding`](N_encode.md#encodeencoding).

**Returns:** `string`, the HMAC.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Enums

### crypto.HashAlg
<!-- id: N/crypto.HashAlg -->

Hash algorithms, used by `createHash` and `createHmac`. Since 2015.2.

| Value |
|---|
| `SHA256` |
| `SHA512` |

### crypto.Padding
<!-- id: N/crypto.Padding -->

Padding modes, used by `createCipher` and `createDecipher`. Since 2015.2.

| Value |
|---|
| `NoPadding` |
| `PKCS5Padding` |

### crypto.EncryptionAlg
<!-- id: N/crypto.EncryptionAlg -->

Encryption algorithms, used by `createCipher` and `createDecipher`. Since 2015.2.

| Value |
|---|
| `AES` |

## Examples

A RESTlet that checks an HMAC signature sent by a logistics partner. The shared key is an API
secret referenced only by its script ID, so no key text appears in the code.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/crypto', 'N/encode'], (crypto, encode) => {
  const post = (body) => {
    const key = crypto.createSecretKey({
      secret: 'custsecret_carrier_webhook',
      encoding: encode.Encoding.UTF_8
    });
    const mac = crypto.createHmac({ algorithm: crypto.HashAlg.SHA256, key });
    mac.update({ input: JSON.stringify(body.shipment), inputEncoding: encode.Encoding.UTF_8 });
    const expected = mac.digest({ outputEncoding: encode.Encoding.BASE_64 });
    return { accepted: expected === body.signature };
  };

  return { post };
});
```

Encrypting an archive note and decrypting it later. The IV from the payload is stored next to the
cipher text, because decryption needs it.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/crypto', 'N/encode'], (crypto, encode) => {
  const keyRef = () => crypto.createSecretKey({ secret: 'custsecret_archive_aes' });

  const seal = (plainText) => {
    const cipher = crypto.createCipher({
      algorithm: crypto.EncryptionAlg.AES,
      key: keyRef(),
      padding: crypto.Padding.PKCS5Padding
    });
    cipher.update({ input: plainText, inputEncoding: encode.Encoding.UTF_8 });
    const payload = cipher.final({ outputEncoding: encode.Encoding.BASE_64 });
    return { body: payload.ciphertext, iv: payload.iv };
  };

  const unseal = (sealed) => {
    const decipher = crypto.createDecipher({
      algorithm: crypto.EncryptionAlg.AES,
      key: keyRef(),
      iv: sealed.iv,
      padding: crypto.Padding.PKCS5Padding
    });
    decipher.update({ input: sealed.body, inputEncoding: encode.Encoding.BASE_64 });
    return decipher.final({ outputEncoding: encode.Encoding.UTF_8 });
  };

  return { seal, unseal };
});
```

A Suitelet that checks a PIN typed by a warehouse supervisor against the password field of a custom
record, without ever reading the stored value:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/crypto'], (crypto) => {
  const onRequest = (context) => {
    const params = context.request.parameters;
    const ok = crypto.checkPasswordField({
      recordType: 'customrecord_dock_override',
      recordId: Number(params.overrideId),
      fieldId: 'custrecord_dock_supervisor_pin',
      value: params.pin
    });
    context.response.write({ output: ok ? 'Override approved' : 'PIN rejected' });
  };

  return { onRequest };
});
```
