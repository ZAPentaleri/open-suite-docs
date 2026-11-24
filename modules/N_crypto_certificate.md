# N/crypto/certificate
<!-- id: N/crypto/certificate -->

`N/crypto/certificate` signs data with a digital certificate stored in NetSuite and checks
signatures made with a certificate. It relies on asymmetric (public/private key) cryptography and
handles two kinds of input: whole XML documents (`signXml`, `verifyXmlSignature`) and plain strings
(through a [`Signer`](#signer-object) or a [`Verifier`](#verifier-object)). Every function refers to
a certificate by its script ID (`certId`); see [N/certificateControl](N_certificateControl.md) for
managing certificates.

- **Availability:** server scripts only.
- **Permission:** the executing role needs the certificate access permission (the source names it
  only as certificate access).
- **Load it as:** `define(['N/crypto/certificate'], (certificate) => { ... })`
- Every `algorithm` option takes a value from [`certificate.HashAlg`](#certificatehashalg), where
  the allowed values are listed.
- XML signed by `signXml` comes back with line breaks disabled; do not expect pretty-printed output.
- The XML result object, [`SignedXml`](#signedxml-object), offers `asFile`, `asString` and `asXml`.
- [`Signer.sign`](#signersignoptions) returns the signature as a string.

> [!SOURCE-ERROR] The source's module member table lists `Signer.sign` as returning nothing (void),
> while the method's own entry gives a string signature. Rely on the string return.
<!-- source-error: N/crypto/certificate | - | module_table_says_signer_sign_returns_void -->

- Related: [N/certificateControl](N_certificateControl.md), [N/crypto](N_crypto.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`certificate.createSigner(options)`](#certificatecreatesigneroptions) | function | Start signing a plain string |
| [`certificate.createVerifier(options)`](#certificatecreateverifieroptions) | function | Start checking a plain-string signature |
| [`certificate.signXml(options)`](#certificatesignxmloptions) | function | Sign an XML document |
| [`certificate.verifyXmlSignature(options)`](#certificateverifyxmlsignatureoptions) | function | Check the signature inside an XML document |
| [`SignedXml`](#signedxml-object) | object | Signed XML in string, file or DOM form |
| [`Signer`](#signer-object) | object | Signs plain strings |
| [`Verifier`](#verifier-object) | object | Verifies plain-string signatures |
| [`certificate.HashAlg`](#certificatehashalg) | enum | Digest algorithms |

## Functions

### certificate.createSigner(options)
<!-- id: N/crypto/certificate.createSigner -->

Builds a [`Signer`](#signer-object) for the given certificate and digest algorithm.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.certId` | string | Yes | |

- `options.algorithm`: a value from [`certificate.HashAlg`](#certificatehashalg).
- `options.certId`: the script ID of the digital certificate to sign with.

**Returns:** `certificate.Signer`.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.1

### certificate.createVerifier(options)
<!-- id: N/crypto/certificate.createVerifier -->

Builds a [`Verifier`](#verifier-object) that checks signatures against the given certificate and
digest algorithm.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.certId` | string | Yes | |

- `options.algorithm`: a value from [`certificate.HashAlg`](#certificatehashalg).
- `options.certId`: the script ID of the digital certificate whose signatures you are checking.

**Returns:** the source types the result as `Parameters`; what it describes handing back is a
verifier.

> [!SOURCE-ERROR] The source describes the return value as a "Parameters" object, and the return
> type repeats that. The function's purpose is to produce a verifier, so treat the result as a
> [`Verifier`](#verifier-object), and confirm in your account if unsure.
<!-- source-error: N/crypto/certificate.createVerifier | - | returns_described_as_parameters_object -->

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.1

### certificate.signXml(options)
<!-- id: N/crypto/certificate.signXml -->

Signs an XML document with a certificate and returns the signed document. Four inputs are needed:
the XML text, the certificate ID, the digest algorithm and the root tag. Line breaks are disabled in
the signed output.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.algorithm` | string | Yes | |
| `options.insertionTag` | string | No | |
| `options.certId` | string | Yes | |
| `options.xmlString` | string | Yes | |
| `options.rootTag` | string | Yes | |

- `options.algorithm`: a value from [`certificate.HashAlg`](#certificatehashalg).
- `options.insertionTag`: the tag where the signature is inserted.
- `options.certId`: the certificate's script ID, in the form `custcertificate1`.

**Returns:** `certificate.SignedXml`, the signed XML.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.1

### certificate.verifyXmlSignature(options)
<!-- id: N/crypto/certificate.verifyXmlSignature -->

Verifies the signature contained in a signed XML document.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.certId` | string | No | |
| `options.rootTag` | string | Yes | |
| `options.signedXml` | string | Yes | |

- `options.certId`: the script ID of the digital certificate to verify against.

**Returns:** `void`

**Errors:** the source does not state how a failed verification is reported; no error codes are
listed for this function.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.1

## Objects

### SignedXml (object)
<!-- id: N/crypto/certificate.SignedXml -->

The result of [`certificate.signXml`](#certificatesignxmloptions). Take whichever form you need.
Server scripts; since 2019.1.

#### SignedXml.asString()
<!-- id: N/crypto/certificate.SignedXml#asString -->

Gives the signed XML as a string.

**Returns:** `string`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

#### SignedXml.asFile()
<!-- id: N/crypto/certificate.SignedXml#asFile -->

Gives the signed XML as a file object.

**Returns:** `file.File`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.2

#### SignedXml.asXml()
<!-- id: N/crypto/certificate.SignedXml#asXml -->

Gives the signed XML as a parsed XML document.

**Returns:** `xml.Document`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.2

### Signer (object)
<!-- id: N/crypto/certificate.Signer -->

Signs plain strings (not XML). Returned by
[`certificate.createSigner`](#certificatecreatesigneroptions). Feed it text with `update`, then
call `sign`. Server scripts; since 2019.1.

#### Signer.update(options)
<!-- id: N/crypto/certificate.Signer#update -->

Adds a string to the data being signed.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.input` | string \| https.SecureString | Yes | |
| `options.inputEncoding` | string | No | `UTF-8` |

- `options.input`: either a plain string or an `https.SecureString`.
- `options.inputEncoding`: a text encoding name written as a string, such as `UTF-8`,
  `ISO_8859_1` or `ASCII`. Unlike the [N/crypto](N_crypto.md) methods, it cannot take an
  `encode.Encoding` enum value.

**Returns:** `void`

**Errors:**

- `SSS_UNSUPPORTED_ENCODING`: `options.inputEncoding` is malformed, which includes passing an
  `encode.Encoding` enum value instead of an encoding name.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

#### Signer.sign(options)
<!-- id: N/crypto/certificate.Signer#sign -->

Produces the signature over everything passed to `update`, as a string with line breaks disabled.
For ECDSA keys the signature is ASN.1 DER encoded by default; set `useRawFormatForECDSA` when the
consumer expects the raw signature instead (for example, an `es256` signature).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.useRawFormatForECDSA` | boolean | No | `false` |
| `options.outputEncoding` | string | No | |

- `options.useRawFormatForECDSA`: `true` returns a raw ECDSA signature instead of DER.
- `options.outputEncoding`: the output is base64. The source does not state a default or which
  other values are accepted.

**Returns:** `string`, the signature.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

### Verifier (object)
<!-- id: N/crypto/certificate.Verifier -->

Checks signatures over plain strings. Returned by
[`certificate.createVerifier`](#certificatecreateverifieroptions). Pass the signed text with
`update`, then call `verify` with the signature. Server scripts; since 2019.1.

#### Verifier.update(options)
<!-- id: N/crypto/certificate.Verifier#update -->

Adds a string to the data whose signature is being checked.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.inputEncoding` | string | No | `UTF-8` |
| `options.input` | string | Yes | |

> [!SOURCE-ERROR] In the source, the row naming this method's parent object says "Parameters"
> instead of `Verifier`. The method belongs to the `Verifier` object.
<!-- source-error: N/crypto/certificate.Verifier#update | - | parent_object_row_says_parameters -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

#### Verifier.verify(options)
<!-- id: N/crypto/certificate.Verifier#verify -->

Checks the supplied signature against the data passed to `update`. It returns nothing; a signature
that does not check out raises an error, so wrap the call in `try`/`catch`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.signatureEncoding` | string | No | |
| `options.signature` | string | Yes | |

- `options.signatureEncoding`: the signature is base64. The source does not state a default or
  which other values are accepted.

**Returns:** `void`

**Errors:**

- `INVALID_SIGNATURE`: an argument value is invalid, that is, the certificate, the algorithm or
  the signature itself is wrong.

> [!SOURCE-ERROR] As with `update`, the source's parent-object row for this method reads
> "Parameters" rather than `Verifier`.
<!-- source-error: N/crypto/certificate.Verifier#verify | - | parent_object_row_says_parameters -->

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

## Enums

### certificate.HashAlg
<!-- id: N/crypto/certificate.HashAlg -->

Digest algorithms for signing and verifying. Since 2019.1. Which digests work depends on the
certificate's key type: RSA and ECDSA keys support SHA256, SHA384 and SHA512; DSA keys support
SHA256 only.

| Value |
|---|
| `SHA256` |
| `SHA384` |
| `SHA512` |

## Examples

A RESTlet that signs an outgoing payment-instruction XML document before it is handed to a bank
connector:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/crypto/certificate'], (certificate) => {
  const post = (body) => {
    const signed = certificate.signXml({
      algorithm: certificate.HashAlg.SHA256,
      certId: 'custcertificate_treasury_signing',
      rootTag: 'PaymentBatch',
      xmlString: body.batchXml
    });
    return { signedXml: signed.asString() };
  };

  return { post };
});
```

Checking a detached signature that a supplier attached to a price-list update. A bad signature
raises an error, so the check is wrapped:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/crypto/certificate', 'N/log'], (certificate, log) => {
  const isAuthentic = (priceListText, signatureB64) => {
    const verifier = certificate.createVerifier({
      algorithm: certificate.HashAlg.SHA384,
      certId: 'custcertificate_supplier_pricing'
    });
    verifier.update({ input: priceListText, inputEncoding: 'UTF-8' });
    try {
      verifier.verify({ signature: signatureB64 });
      return true;
    } catch (e) {
      log.audit({ title: 'Price list signature rejected', details: e.name });
      return false;
    }
  };

  return { isAuthentic };
});
```

Producing a raw ECDSA signature for a token whose consumer cannot read DER:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/crypto/certificate'], (certificate) => {
  const signToken = (headerAndClaims) => {
    const signer = certificate.createSigner({
      algorithm: certificate.HashAlg.SHA256,
      certId: 'custcertificate_device_gateway'
    });
    signer.update({ input: headerAndClaims });
    return signer.sign({ useRawFormatForECDSA: true });
  };

  return { signToken };
});
```
