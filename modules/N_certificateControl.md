# N/certificateControl
<!-- id: N/certificateControl -->

`N/certificateControl` manages the account's list of digital certificates (the list kept under
Setup > Company > Certificates): add new ones, load and change them, delete them, lock them against
edits, search them by metadata, and read a time-stamped audit trail of the operations performed
with them. Certificates cannot be reached through N/record; this module is the scripted route.

- **Availability:** server scripts only.
- **Permissions:** the module requires the certificate management permission, and the script must
  execute as an administrator or under a role that has certificate access. (The source names these
  permissions only in that generic form.)
- **Load it as:** `define(['N/certificateControl'], (certificateControl) => { ... })`
- A new certificate's content always comes from a file in the File Cabinet.
- A certificate's password can be given directly or taken from a secret key field or an API secret
  (by its script ID). Referencing a stored secret keeps the password out of your code.
- `restrictions` carries the employee IDs of the certificate's "Restrict to Employees" field, and
  `notifications` the employee IDs of its "Copy Employees" field.
- Expiry reminders can be switched on for one week, one month and three months before expiry.
- Locking a certificate blocks edits to it until it is unlocked.
- Related: [N/crypto/certificate](N_crypto_certificate.md),
  [N/https/clientCertificate](N_https_clientCertificate.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`certificateControl.createCertificate(options)`](#certificatecontrolcreatecertificateoptions) | function | Add a certificate from a File Cabinet file |
| [`certificateControl.loadCertificate(options)`](#certificatecontrolloadcertificateoptions) | function | Load an existing certificate |
| [`certificateControl.deleteCertificate(options)`](#certificatecontroldeletecertificateoptions) | function | Delete a certificate |
| [`certificateControl.findCertificates(options)`](#certificatecontrolfindcertificatesoptions) | function | Search certificate metadata |
| [`certificateControl.findUsages(options)`](#certificatecontrolfindusagesoptions) | function | Audit trail of certificate use |
| [`certificateControl.lock(options)`](#certificatecontrollockoptions) | function | Make a certificate non-editable |
| [`certificateControl.unlock(options)`](#certificatecontrolunlockoptions) | function | Lift a lock |
| [`Certificate`](#certificate-object) | object | A certificate record with its file and settings |
| [`certificateControl.Type`](#certificatecontroltype) | enum | Certificate file formats |
| [`certificateControl.Operator`](#certificatecontroloperator) | enum | Text-matching operators for searches |
| [`certificateControl.Operation`](#certificatecontroloperation) | enum | Operations recorded in the usage trail |

## Functions

### certificateControl.createCertificate(options)
<!-- id: N/certificateControl.createCertificate -->

Creates a certificate entry, the kind listed on the Certificates page, from a certificate file in
the File Cabinet. Only `file` and `name` are mandatory; subsidiaries, employee restrictions,
notification recipients and the three expiry reminders are optional.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file | Yes | |
| `options.password` | string | No | |
| `options.scriptId` | string | No | |
| `options.description` | string | No | |
| `options.subsidiaries` | number[] \| string[] | No | |
| `options.restrictions` | number[] \| string[] | No | |
| `options.notifications` | number[] \| string[] | No | |
| `options.name` | string | Yes | |
| `options.weekReminder` | boolean | No | |
| `options.monthReminder` | boolean | No | |
| `options.threeMonthsReminder` | boolean | No | |

- `options.file`: must be a file stored in the File Cabinet.
- `options.password`: either the certificate's password or the script ID of an API secret that
  holds it.
- `options.scriptId`: certificate script IDs start with `custcertificate`; that prefix is added
  automatically.
- All option fields exist since 2019.2.

**Returns:** `certificateControl.Certificate`, the new certificate.

**Governance:** 10 units.

**Availability:** server scripts. The role needs certificate access at Create, Edit or Full level.

**Since:** 2019.2

### certificateControl.loadCertificate(options)
<!-- id: N/certificateControl.loadCertificate -->

Loads a certificate so you can read or change it. To find the certificate's ID, look at Setup >
Company > Certificates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |

**Returns:** `certificateControl.Certificate`, the certificate object.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

### certificateControl.deleteCertificate(options)
<!-- id: N/certificateControl.deleteCertificate -->

Deletes a certificate and returns its script ID. The certificate's history is kept after deletion.
To find the certificate's ID, look at Setup > Company > Certificates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |

**Returns:** `string`, the script ID of the deleted certificate.

**Governance:** 10 units.

**Availability:** server scripts. The role needs certificate access at Edit or Full level.

**Since:** 2019.2

### certificateControl.findCertificates(options)
<!-- id: N/certificateControl.findCertificates -->

Returns metadata for the certificates that match the filters you give. With no filters, every
certificate is returned.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.name` | string | No | |
| `options.type` | string | No | |
| `options.description` | string | No | |
| `options.notification` | number | No | |
| `options.restriction` | number | No | |
| `options.scriptRestriction` | string | No | |
| `options.subsidiary` | number | No | |

- `options.name`, `options.description`: text filters. Pass either a plain string, or an object
  with `value`, `operator` and `ignoreCase` members, where `operator` is a value from
  [`certificateControl.Operator`](#certificatecontroloperator). Since 2019.2.
- `options.type`: a value from [`certificateControl.Type`](#certificatecontroltype). Since 2019.1.
- `options.notification`: an employee ID from the certificate's "Copy Employees" list. Since 2019.2.
- `options.restriction`: an employee ID from the certificate's "Restrict to Employees" list. Since
  2019.2.
- `options.scriptRestriction`: the name of a script the certificate is restricted to. Since 2022.1.
- `options.subsidiary`: a subsidiary to filter on. Since 2019.1.

**Returns:** `Object[]`, the metadata of each matching certificate. The source does not state which
members these metadata objects have.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.1

### certificateControl.findUsages(options)
<!-- id: N/certificateControl.findUsages -->

Returns an audit trail of operations performed with certificates, each with a time stamp. At most
1000 rows can come back; narrow the filters when you would exceed that.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.operation` | string | No | |
| `options.id` | string | No | |
| `options.script` | number | No | |
| `options.deploy` | number | No | |
| `options.entity` | number | No | |
| `options.from` | date | No | |
| `options.to` | date | No | |

- `options.operation`: a value from [`certificateControl.Operation`](#certificatecontroloperation).
- `options.id`: the script ID of the certificate record.
- `options.script`: the script ID of the script record.
- `options.deploy`: the script ID of the script deployment.
- `options.entity`: the internal ID of the employee who performed the operation.
- `options.from`, `options.to`: the date range to search. The source does not say whether the
  bounds are inclusive.
- All option fields exist since 2019.2.

> [!SOURCE-ERROR] `options.script` and `options.deploy` are typed `number`, yet described as script
> IDs, which are normally strings. The source does not settle which is right; test both forms in
> your account before relying on either.
<!-- source-error: N/certificateControl.findUsages | - | script_and_deploy_typed_number_but_described_as_script_id -->

**Returns:** `Array` of usage entries. The source does not state the members of each entry.

**Errors:**

- `TOO_MANY_RESULTS`: the search would return more than 1000 rows.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

### certificateControl.lock(options)
<!-- id: N/certificateControl.lock -->

Locks a certificate so it cannot be edited until it is unlocked again with
[`unlock`](#certificatecontrolunlockoptions). To find the certificate's ID, look at
Setup > Company > Certificates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `void`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.1

### certificateControl.unlock(options)
<!-- id: N/certificateControl.unlock -->

Removes the lock from a locked certificate. To find the certificate's ID, look at Setup > Company >
Certificates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `void`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.1

## Objects

### Certificate (object)
<!-- id: N/certificateControl.Certificate -->

One certificate entry: its uploaded file plus its settings (name, description, reminders, who may
use it, who is told about expiry). Returned by `createCertificate`, `loadCertificate` and `save`.
Server scripts; since 2019.2.

#### Certificate.save()
<!-- id: N/certificateControl.Certificate#save -->

Saves the certificate with the changes made to its properties.

**Returns:** `certificateControl.Certificate`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Certificate.description
<!-- id: N/certificateControl.Certificate#description -->

`string`. The certificate's description. Since 2019.2.

#### Certificate.file
<!-- id: N/certificateControl.Certificate#file -->

`file.File`. The uploaded certificate file. Since 2019.2.

#### Certificate.name
<!-- id: N/certificateControl.Certificate#name -->

`string`. The certificate's name. Since 2019.2.

#### Certificate.monthReminder
<!-- id: N/certificateControl.Certificate#monthReminder -->

`boolean`. When `true`, administrators are emailed one month before the certificate expires; the
employees in the copy list are copied when that option is checked. Since 2019.2.

#### Certificate.notifications
<!-- id: N/certificateControl.Certificate#notifications -->

`number[]`. IDs of employees copied on the expiry emails. Setting it also checks the "Copy
Employees" option. Since 2019.2.

#### Certificate.password
<!-- id: N/certificateControl.Certificate#password -->

`string`, write-only. Set it to the certificate password, to an empty string, or to the script ID
of an API secret that holds the password; it cannot be read back. Since 2019.2.

#### Certificate.restrictions
<!-- id: N/certificateControl.Certificate#restrictions -->

`number[]`. IDs of employees allowed to use the certificate. Once set, only administrators and the
selected employees can access it, and the "Restrict to Employees" option is checked. Working with it
requires the certificate management and certificate access permissions. Since 2019.2.

#### Certificate.scriptId
<!-- id: N/certificateControl.Certificate#scriptId -->

`string`. The certificate's script ID, which must start with `custcertificate`. Since 2019.2.

#### Certificate.subsidiaries
<!-- id: N/certificateControl.Certificate#subsidiaries -->

`number[]`. IDs of the subsidiaries associated with the certificate. The association does not
change who can access the certificate. Since 2019.2.

#### Certificate.threeMonthsReminder
<!-- id: N/certificateControl.Certificate#threeMonthsReminder -->

`boolean`. When `true`, administrators are emailed three months before expiry; copied employees
are included when that option is checked. Since 2019.2.

#### Certificate.weekReminder
<!-- id: N/certificateControl.Certificate#weekReminder -->

`boolean`. When `true`, administrators are emailed one week before expiry; copied employees are
included when that option is checked. Since 2019.2.

## Enums

### certificateControl.Type
<!-- id: N/certificateControl.Type -->

Certificate file formats, used by the `type` filter of `findCertificates`. Since 2019.1.

| Value |
|---|
| `PFX` |
| `P12` |
| `PEM` |

### certificateControl.Operator
<!-- id: N/certificateControl.Operator -->

Text-matching operators for the `name` and `description` filters of `findCertificates`. Since
2019.2.

| Value |
|---|
| `CONTAINS` |
| `ENDS_WITH` |
| `EQUALS` |
| `STARTS_WITH` |

### certificateControl.Operation
<!-- id: N/certificateControl.Operation -->

Kinds of operation recorded in the usage trail; used by the `operation` filter of `findUsages`.
Since 2019.2.

| Value |
|---|
| `CONNECT` |
| `DELETE` |
| `FIND` |
| `GET` |
| `HEAD` |
| `POST` |
| `PUT` |
| `SIGN_STRING` |
| `SIGN_XML` |
| `VERIFY_STRING` |
| `VERIFY_XML` |

## Examples

A scheduled script that registers a renewed PEM certificate dropped into the File Cabinet, turns on
the week and month reminders, and locks it. The password is referenced through an API secret
script ID read from a script parameter, so no password appears in the code.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/certificateControl', 'N/file', 'N/runtime', 'N/log'], (certificateControl, file, runtime, log) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const pemFile = file.load({ id: script.getParameter({ name: 'custscript_renewed_cert_file' }) });

    const cert = certificateControl.createCertificate({
      file: pemFile,
      name: 'EDI gateway client cert (renewed)',
      scriptId: '_edi_gateway_2026',
      password: script.getParameter({ name: 'custscript_cert_secret_id' }),
      weekReminder: true,
      monthReminder: true
    });

    certificateControl.lock({ id: cert.scriptId });
    log.audit({ title: 'Certificate registered', details: cert.scriptId });
  };

  return { execute };
});
```

Finding certificates whose name starts with a vendor prefix, ignoring case:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/certificateControl'], (certificateControl) => {
  const bankCertificates = () => certificateControl.findCertificates({
    name: {
      value: 'harborbank',
      operator: certificateControl.Operator.STARTS_WITH,
      ignoreCase: true
    },
    type: certificateControl.Type.PEM
  });

  return { bankCertificates };
});
```

Reviewing last month's XML signing activity, staying within the 1000-row limit by filtering on one
operation and catching the overflow error:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/certificateControl', 'N/log'], (certificateControl, log) => {
  const signingActivity = () => {
    const to = new Date();
    const from = new Date(to.getFullYear(), to.getMonth() - 1, to.getDate());
    try {
      return certificateControl.findUsages({
        operation: certificateControl.Operation.SIGN_XML,
        from,
        to
      });
    } catch (e) {
      if (e.name === 'TOO_MANY_RESULTS') {
        log.audit({ title: 'Narrow the date range', details: e.message });
        return [];
      }
      throw e;
    }
  };

  return { signingActivity };
});
```
