# N/pgp
<!-- id: N/pgp -->

`N/pgp` brings OpenPGP to server scripts: encrypt data for a partner, decrypt what a partner sends
you, sign content, and check the signatures on what you receive. Its purposes are secure message
exchange, file encryption and document signing.

- **Availability:** server scripts, and only in SuiteScript 2.1.
- **Load it as:** `define(['N/pgp'], (pgp) => { ... })`
- Related: [N/crypto/certificate](N_crypto_certificate.md) (the `Signer` returned by
  `createSigner`), [N/sftp](N_sftp.md) (moving encrypted files).

Things to know before you start:

- **Keys come from outside.** You need PGP keys generated elsewhere; the module cannot generate,
  modify or inspect PGP keys. Keys are kept in NetSuite's secrets management (Setup > Company > API
  Secrets) and loaded with [`pgp.loadKeyFromSecret`](#pgploadkeyfromsecretoptions).
- **Strings in memory.** The module works on strings only, and all the data you process must fit
  in memory.
- **The data flow.** Plain content becomes a [`MessageData`](#messagedata-object) (via
  [`createMessageData`](#pgpcreatemessagedataoptions)); encrypting it yields a
  [`Message`](#message-object), which you serialize with `asArmored()`. In the other direction,
  [`parseMessage`](#pgpparsemessageoptions) turns armored text into a `Message`, and `decrypt` gives
  back a `MessageData`. `Message.toMessageData()` works only on messages that are not encrypted.
- **Read-only results.** The properties of `Config`, `Message`, `MessageData`, `Verification` and
  `VerificationSignature` cannot be assigned: setting any of them raises `READ_ONLY_PROPERTY`.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`pgp.createConfig(options)`](#pgpcreateconfigoptions) | function | Decryption settings |
| [`pgp.createMessageData(options)`](#pgpcreatemessagedataoptions) | function | Wrap content for encryption |
| [`pgp.createSigner(options)`](#pgpcreatesigneroptions) | function | A string signer backed by a PGP key |
| [`pgp.createVerification()`](#pgpcreateverification) | function | Empty container for verification results |
| [`pgp.loadKeyFromSecret(options)`](#pgploadkeyfromsecretoptions) | function | Load a key stored as a secret |
| [`pgp.parseKey(options)`](#pgpparsekeyoptions) | function | Parse an armored key string |
| [`pgp.parseMessage(options)`](#pgpparsemessageoptions) | function | Parse an armored message |
| [`Config`](#config-object) | object | Decryption settings |
| [`Key`](#key-object) | object | A PGP key |
| [`KeyId`](#keyid-object) | object | A key or subkey ID |
| [`Message`](#message-object) | object | Processed PGP data |
| [`MessageData`](#messagedata-object) | object | Content plus metadata |
| [`Verification`](#verification-object) | object | Overall signature check result |
| [`VerificationSignature`](#verificationsignature-object) | object | Result for one signature |
| [`pgp.Format`](#pgpformat) | enum | Text or binary content |
| [`pgp.CompressionAlgorithm`](#pgpcompressionalgorithm) | enum | Compression for encryption |

## Functions

### pgp.createConfig(options)
<!-- id: N/pgp.createConfig -->

Builds a [`Config`](#config-object) holding decryption settings, to pass as `options.config` of
[`Message.decrypt`](#messagedecryptoptions). Each setting relaxes a safety check, and all are off by
default.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.allowInsecureDecryptionWithSigningKeys` | boolean | No | `false` |
| `options.allowMessagesWithoutIntegrityProtection` | boolean | No | `false` |
| `options.useRelaxedSignatureParsing` | boolean | No | `false` |

- `options.allowInsecureDecryptionWithSigningKeys`: `true` allows decryption without the security
  that normally applies to signing keys.
- `options.allowMessagesWithoutIntegrityProtection`: `true` accepts messages that carry no
  integrity protection.
- `options.useRelaxedSignatureParsing`: `true` parses signatures in relaxed mode.
- All option fields exist since 2022.2.

**Returns:** `pgp.Config`, the decryption configuration.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

### pgp.createMessageData(options)
<!-- id: N/pgp.createMessageData -->

Wraps content, with an optional file name and date, into a [`MessageData`](#messagedata-object)
that you can then encrypt.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filename` | string | No | |
| `options.date` | Date | No | |
| `options.format` | pgp.Format | No | `Format.UTF8` |
| `options.content` | string | Yes | |

- `options.filename`: an empty string when the message is not a file.
- `options.date`: the current date when omitted.
- `options.format`: a value from [`pgp.Format`](#pgpformat). The default depends on the content:
  `UTF8` when the content is a string, `BINARY` otherwise.
- `options.content`: the content itself.

**Returns:** `pgp.MessageData`, the content together with its metadata.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

### pgp.createSigner(options)
<!-- id: N/pgp.createSigner -->

Creates a string signer that uses a PGP key. It returns the same kind of
[`Signer`](N_crypto_certificate.md#signer-object) as N/crypto/certificate. From the key it uses the
most recently added valid subkey, the same choice
[`MessageData.encrypt`](#messagedataencryptoptions) makes.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.key` | pgp.Key | Yes | |

- `options.algorithm`: the hash algorithm. The source's syntax sample passes
  [`certificate.HashAlg.SHA256`](N_crypto_certificate.md#certificatehashalg); it does not list the
  accepted values for PGP keys. Since 2022.2.
- `options.key`: the private key to sign with. Since 2022.2.

**Returns:** `certificate.Signer`, a signer for strings.

**Errors:**

- `PGP_NO_SIGNING_KEY_FOUND_IN_KEY_PARAM_1`: no usable signing key is found in `options.key`.
- `UNSUPPORTED_COMBINATION_OF_KEY_AND_HASH_ALGORITHMS`: the key and the hash algorithm are not
  compatible.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2022.2

### pgp.createVerification()
<!-- id: N/pgp.createVerification -->

Returns an empty [`Verification`](#verification-object). Pass it as `options.verification` of
[`Message.decrypt`](#messagedecryptoptions); decrypt then writes the signature results into it
instead of raising verification errors.

**Returns:** `pgp.Verification`, initially empty.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

### pgp.loadKeyFromSecret(options)
<!-- id: N/pgp.loadKeyFromSecret -->

Loads a PGP key that is stored as a secret. This is the recommended way to bring a private key into
a script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.secret` | Secret | Yes | |
| `options.password` | Secret | No | |

- `options.secret`: the stored secret holding the key in ASCII-armored form. The source's syntax
  sample passes it as an object with a `scriptId` member (the secret's script ID); it does not
  define the `Secret` type further. Since 2022.2.
- `options.password`: the key's passphrase, also a secret given as an object with `scriptId`; only
  relevant for private keys. Since 2022.2.

**Returns:** `pgp.Key`, the key read from the secret.

**Errors:**

- `REFERENCED_SECRET_IS_NOT_AVAILABLE`: the secret does not exist or the script has no permission
  to use it.
- `PGP_NONSTANDARD_KEY_DOES_NOT_COMPLY_WITH_PGP_KEY_FORMAT`: the key could not be parsed.
- `PGP_YOU_CANNOT_PROVIDE_PASSWORD_FOR_A_PUBLIC_KEY`: a password was given for a public key.
- `PGP_INVALID_KEY_PASSWORD`: the password is wrong, or the key is not password-protected.
- `PGP_THIS_KEY_IS_PASSWORD_PROTECTED`: the key needs a password and none was given.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

### pgp.parseKey(options)
<!-- id: N/pgp.parseKey -->

Parses an ASCII-armored key string into a [`Key`](#key-object). For private keys, prefer
[`loadKeyFromSecret`](#pgploadkeyfromsecretoptions), which keeps the key text out of your script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | Yes | |
| `options.password` | string | No | |

- `options.value`: the key, ASCII-armored.
- `options.password`: the passphrase; only relevant for private keys.

**Returns:** `pgp.Key`, the parsed key.

**Errors:**

- `PGP_NONSTANDARD_KEY_DOES_NOT_COMPLY_WITH_PGP_KEY_FORMAT`: the key could not be parsed.
- `PGP_YOU_CANNOT_PROVIDE_PASSWORD_FOR_A_PUBLIC_KEY`: a password was given for a public key.
- `PGP_INVALID_KEY_PASSWORD`: the password is wrong, or the key is not password-protected.
- `PGP_THIS_KEY_IS_PASSWORD_PROTECTED`: the key needs a password and none was given.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

### pgp.parseMessage(options)
<!-- id: N/pgp.parseMessage -->

Parses an ASCII-armored PGP message, such as a file a partner sent, into a
[`Message`](#message-object).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | Yes | |

- `options.value`: the message, ASCII-armored. Since 2022.2.

**Returns:** `pgp.Message`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

## Objects

### Config (object)
<!-- id: N/pgp.Config -->

Decryption settings, created by [`pgp.createConfig(options)`](#pgpcreateconfigoptions) and passed as
`options.config` to [`Message.decrypt`](#messagedecryptoptions). Its properties reflect the options
given to `createConfig` and are read-only once the object exists: assigning to any of them, for
example to `allowInsecureDecryptionWithSigningKeys`, raises `READ_ONLY_PROPERTY`. To change a
setting, create a new `Config`. Server scripts, since 2022.2.

#### Config.allowInsecureDecryptionWithSigningKeys
<!-- id: N/pgp.Config#allowInsecureDecryptionWithSigningKeys -->

`boolean`, read-only. `true` means decryption is allowed without the security that normally
applies to signing keys. Since 2022.2.

**Errors:**

- `READ_ONLY_PROPERTY`: raised when a script sets this property.

#### Config.allowMessagesWithoutIntegrityProtection
<!-- id: N/pgp.Config#allowMessagesWithoutIntegrityProtection -->

`boolean`, read-only. `true` means messages without integrity protection are accepted. Since
2022.2.

**Errors:**

- `READ_ONLY_PROPERTY`: raised when a script sets this property.

#### Config.useRelaxedSignatureParsing
<!-- id: N/pgp.Config#useRelaxedSignatureParsing -->

`boolean`, read-only. `true` means signatures are parsed in relaxed mode. Since 2022.2.

**Errors:**

- `READ_ONLY_PROPERTY`: raised when a script sets this property.

### Key (object)
<!-- id: N/pgp.Key -->

A PGP key: the cryptographic keys plus their metadata. You obtain one from
[`loadKeyFromSecret`](#pgploadkeyfromsecretoptions) or [`parseKey`](#pgpparsekeyoptions) and pass it
to [`Message.decrypt`](#messagedecryptoptions), [`MessageData.encrypt`](#messagedataencryptoptions)
and [`createSigner`](#pgpcreatesigneroptions). The module cannot generate new keys. The source does
not state any properties or methods for this object, so treat it as an opaque handle. Server
scripts, since 2022.2.

### KeyId (object)
<!-- id: N/pgp.KeyId -->

The ID of a key or subkey, held as an octet scalar. You meet it as
[`VerificationSignature.keyId`](#verificationsignaturekeyid). Server scripts, since 2022.2.

#### KeyId.asHex()
<!-- id: N/pgp.KeyId#asHex -->

Returns the key ID as a hexadecimal string, useful for logging which key signed a message.

**Returns:** `string`, the key ID in hex.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

### Message (object)
<!-- id: N/pgp.Message -->

PGP data after processing (encrypted, signed, compressed, or none of these). You get one from
[`MessageData.encrypt`](#messagedataencryptoptions),
[`MessageData.toMessage`](#messagedatatomessage) or [`pgp.parseMessage`](#pgpparsemessageoptions).
Server scripts, since 2022.2.

#### Message.asArmored()
<!-- id: N/pgp.Message#asArmored -->

Serializes the message as ASCII-armored text, ready to write to a file or send.

**Returns:** `string`, the armored message.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

#### Message.decrypt(options)
<!-- id: N/pgp.Message#decrypt -->

Decrypts the message and, when you supply verification keys, checks its signatures.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.decryptionKeys` | pgp.Key \| pgp.Key[] | Yes | |
| `options.verificationKeys` | pgp.Key \| pgp.Key[] | No | `[]` |
| `options.verification` | pgp.Verification | No | `null` |
| `options.supressVerificationErrors` [sic] | boolean | No | `false` |
| `options.config` | pgp.Config | No | |

- `options.decryptionKeys`: one key or an array of keys.
- `options.verificationKeys`: one key or an array; an empty array when omitted. With no
  verification key, signatures are ignored. When keys are given, at least one signature must
  verify. A key that has since expired is still valid for a signature made before it expired.
- `options.verification`: a [`Verification`](#verification-object) from
  [`createVerification`](#pgpcreateverification). When given, the results are flushed into it
  instead of being raised as errors, which also means verification errors are suppressed.
- `options.supressVerificationErrors` [sic]: `true` stops verification errors from being thrown.

> [!SOURCE-ERROR] This option name is spelled `supressVerificationErrors` [sic], with one "p". The
> source does not confirm whether the runtime expects that spelling or the correctly spelled form;
> test which one takes effect.
<!-- source-error: N/pgp.Message#decrypt | options.supressVerificationErrors | parameter_name_misspelled_supress -->

- `options.config`: a [`Config`](#config-object); when omitted, the defaults of
  [`createConfig`](#pgpcreateconfigoptions) apply.
- All option fields exist since 2022.2.

**Returns:** `pgp.MessageData`, the decrypted content.

**Errors:**

- `PGP_NO_MATCHING_DECRYPTION_KEY_ANALYSIS_1`: none of the decryption keys matches the message.
- `PGP_MESSAGE_IS_NOT_INTEGRITY_PROTECTED`: the message has no integrity protection, and the
  content is rejected.
- `PGP_INTEGRITY_VERIFICATION_FAILED`: the message is corrupted.
- `PGP_VERIFICATION_FAILED_NO_SIGNATURE`: verification keys were given but the message is unsigned.
- `PGP_VERIFICATION_FAILED_1`: signature verification failed.
- A malformed message raises parse errors. The source does not state their error codes.

> [!SOURCE-ERROR] The source gives an error code for this method that reads like
> `PGP_EXPECTED_ENCRYPTED_MESSAGE` but contains a space, so the exact code is uncertain and it is
> not among the codes listed above. Catch errors generically and check the code your account
> actually returns.
<!-- source-error: N/pgp.Message#decrypt | - | error_code_pgp_expected_encrypted_message_has_space -->

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

#### Message.toMessageData()
<!-- id: N/pgp.Message#toMessageData -->

Converts a message that is not encrypted back into a [`MessageData`](#messagedata-object). Use
[`decrypt`](#messagedecryptoptions) for encrypted messages.

**Returns:** `pgp.MessageData`

**Errors:**

- `PGP_EXPECTED_UNENCRYPTED_MESSAGE`: the message is encrypted, which this method does not accept.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

#### Message.type
<!-- id: N/pgp.Message#type -->

Read-only. Tells you which processing method the message needs. The type is recorded as `boolean`,
and the source does not state the possible values. Setting it raises `READ_ONLY_PROPERTY`. Since
2022.2.

> [!SOURCE-ERROR] The source's type row says `boolean`, which does not fit a property used to pick a
> processing method. Inspect the actual value at runtime before branching on it.
<!-- source-error: N/pgp.Message#type | - | type_row_says_boolean -->

### MessageData (object)
<!-- id: N/pgp.MessageData -->

Message content plus metadata (file name, date, and whether the content is text or binary).
Created by [`pgp.createMessageData(options)`](#pgpcreatemessagedataoptions) and returned by
[`Message.decrypt`](#messagedecryptoptions). Its properties are read-only. Server scripts, since
2022.2.

#### MessageData.encrypt(options)
<!-- id: N/pgp.MessageData#encrypt -->

Encrypts the content for the given recipients and optionally signs it. From each key, the most
recently added valid subkey is used.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.encryptionKeys` | pgp.Key \| pgp.Key[] | Yes | |
| `options.signingKeys` | pgp.Key \| pgp.Key[] | No | `[]` |
| `options.compressionAlgorithm` | pgp.CompressionAlgorithm | No | `CompressionAlgorithm.ZLIB` |

- `options.encryptionKeys`: the recipients' keys, one or an array.
- `options.signingKeys`: the keys to sign with, one or an array; an empty array (no signature) when
  omitted.
- `options.compressionAlgorithm`: a value from
  [`pgp.CompressionAlgorithm`](#pgpcompressionalgorithm); ZLIB by default.
- All option fields exist since 2022.2.

**Returns:** `pgp.Message`, encrypted and, when signing keys were given, signed.

**Errors:**

- `PGP_NO_ENCRYPTION_KEY_FOUND_IN_KEY_PARAM_1`: no usable encryption key is found in the keys given.
- `PGP_NO_SIGNING_KEY_FOUND_IN_KEY_PARAM_1`: no usable signing key is found in the keys given.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

#### MessageData.getText()
<!-- id: N/pgp.MessageData#getText -->

Returns the message contents as text.

**Returns:** `string`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

#### MessageData.toMessage()
<!-- id: N/pgp.MessageData#toMessage -->

Wraps the content as a [`Message`](#message-object) without signature, compression or encryption.

**Returns:** `pgp.Message`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2022.2

#### MessageData.filename
<!-- id: N/pgp.MessageData#filename -->

`string`, read-only. The file name stored with the content. Setting it raises `READ_ONLY_PROPERTY`.
Since 2022.2.

#### MessageData.date
<!-- id: N/pgp.MessageData#date -->

`Date`, read-only. The date of the message, or the modification date of the file it carries.
Setting it raises `READ_ONLY_PROPERTY`. Since 2022.2.

#### MessageData.format
<!-- id: N/pgp.MessageData#format -->

`pgp.Format`, read-only. The literal data packet type, that is, whether the content is text or
binary (see [`pgp.Format`](#pgpformat)). Setting it raises `READ_ONLY_PROPERTY`. Since 2022.2.

### Verification (object)
<!-- id: N/pgp.Verification -->

Collects signature verification results. Created empty by
[`pgp.createVerification()`](#pgpcreateverification) and filled in by
[`Message.decrypt`](#messagedecryptoptions). Both properties are read-only. Server scripts, since
2022.2.

#### Verification.verified
<!-- id: N/pgp.Verification#verified -->

`null | boolean`, read-only. `true` when the message verified. Setting it raises
`READ_ONLY_PROPERTY`. Since 2022.2.

#### Verification.signatures
<!-- id: N/pgp.Verification#signatures -->

`null | Array<pgp.VerificationSignature>`, read-only. One
[`VerificationSignature`](#verificationsignature-object) per signature checked. Setting it raises
`READ_ONLY_PROPERTY`. Since 2022.2.

### VerificationSignature (object)
<!-- id: N/pgp.VerificationSignature -->

The outcome of checking one signature, found in
[`Verification.signatures`](#verificationsignatures). All its properties are read-only. Server
scripts, since 2022.2.

#### VerificationSignature.keyId
<!-- id: N/pgp.VerificationSignature#keyId -->

`pgp.KeyId`, read-only. The ID of the subkey that made the signature (see
[`KeyId`](#keyid-object)). Setting it raises `READ_ONLY_PROPERTY`. Since 2022.2.

#### VerificationSignature.dateSigned
<!-- id: N/pgp.VerificationSignature#dateSigned -->

`Date`, read-only. When the signature was made. Setting it raises `READ_ONLY_PROPERTY`. Since
2022.2.

#### VerificationSignature.verified
<!-- id: N/pgp.VerificationSignature#verified -->

`boolean`, read-only. `true` when this signature verified. Setting it raises `READ_ONLY_PROPERTY`.
Since 2022.2.

#### VerificationSignature.problems
<!-- id: N/pgp.VerificationSignature#problems -->

`string[]`, read-only. The problems found with this signature. Setting it raises
`READ_ONLY_PROPERTY`. Since 2022.2.

## Enums

### pgp.Format
<!-- id: N/pgp.Format -->

The literal data packet type: whether content is text or binary. Used by `options.format` of
[`createMessageData`](#pgpcreatemessagedataoptions) and reported by
[`MessageData.format`](#messagedataformat). Since 2022.2.

| Value | Meaning |
|---|---|
| `UTF8` | Text content (the default for string content) |
| `BINARY` | Binary content (the default for non-string content) |

### pgp.CompressionAlgorithm
<!-- id: N/pgp.CompressionAlgorithm -->

Compression applied by [`MessageData.encrypt`](#messagedataencryptoptions). Since 2022.2.

| Value | Meaning |
|---|---|
| `ZLIB` | ZLIB compression (the default) |

## Examples

A custom module that encrypts a remittance CSV for a bank and signs it with the company key. The
bank's public key is not sensitive, so it is read from a script parameter and parsed directly; the
company's private key and its passphrase are referenced only by secret script IDs.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/pgp', 'N/runtime'], (pgp, runtime) => {
  const sealForBank = (csvText) => {
    const bankKey = pgp.parseKey({
      value: runtime.getCurrentScript().getParameter({ name: 'custscript_bank_pgp_public_key' })
    });
    const companyKey = pgp.loadKeyFromSecret({
      secret: { scriptId: 'custsecret_company_pgp_private' },
      password: { scriptId: 'custsecret_company_pgp_passphrase' }
    });

    const payload = pgp.createMessageData({
      content: csvText,
      filename: 'remittance.csv',
      format: pgp.Format.UTF8
    });

    const sealed = payload.encrypt({
      encryptionKeys: bankKey,
      signingKeys: companyKey,
      compressionAlgorithm: pgp.CompressionAlgorithm.ZLIB
    });
    return sealed.asArmored();
  };

  return { sealForBank };
});
```

Opening a signed statement from a payment processor and collecting the verification results
instead of letting a bad signature throw, so the caller can decide whether to accept the file:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/pgp', 'N/log'], (pgp, log) => {
  const openStatement = (armoredText, processorPublicKeyText) => {
    const message = pgp.parseMessage({ value: armoredText });
    const results = pgp.createVerification();

    const contents = message.decrypt({
      decryptionKeys: pgp.loadKeyFromSecret({ secret: { scriptId: 'custsecret_ap_inbox_pgp' } }),
      verificationKeys: [pgp.parseKey({ value: processorPublicKeyText })],
      verification: results
    });

    (results.signatures || []).forEach((sig) => {
      log.debug({
        title: `Signature by ${sig.keyId.asHex()}`,
        details: sig.verified ? 'ok' : sig.problems.join('; ')
      });
    });

    return {
      trusted: results.verified === true,
      fileName: contents.filename,
      text: contents.getText()
    };
  };

  return { openStatement };
});
```

Signing a short delivery manifest string with a PGP key, using the hash value shown in the source's
syntax sample:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/pgp', 'N/crypto/certificate'], (pgp, certificate) => {
  const signManifest = (manifestText) => {
    const signer = pgp.createSigner({
      algorithm: certificate.HashAlg.SHA256,
      key: pgp.loadKeyFromSecret({
        secret: { scriptId: 'custsecret_dispatch_pgp_private' },
        password: { scriptId: 'custsecret_dispatch_pgp_passphrase' }
      })
    });
    signer.update({ input: manifestText });
    return signer.sign();
  };

  return { signManifest };
});
```
