# N/keyControl
<!-- id: N/keyControl -->

`N/keyControl` gives scripts access to the account's key storage. Its main use is SSH keys, which
[N/sftp](N_sftp.md) relies on to manage files over SFTP: you can register keys, load, find and
delete them, and lock or unlock them. In the UI the keys live on the company preferences keys page
(Setup, Company, Preferences, Keys).

- **Availability:** server scripts only.
- **Permission:** a key management permission (the source names it only in that generic form).
- **Load it as:** `define(['N/keyControl'], (keyControl) => { ... })`
- Locking a key blocks edits to it in the UI until it is unlocked.
- Deleting a key marks it as deleted; its history is kept.
- `findKeys` with no criteria returns every key.
- A key's password can be given as the script ID of an API secret (API secrets are stored on the
  company API secrets setup page), so the password itself never has to appear in a script.
- A key's restrictions are the employee IDs taken from the key's restrict-to-employees setting.
- Key script IDs carry the `custkey` prefix.
- Related: [N/sftp](N_sftp.md), [N/certificateControl](N_certificateControl.md),
  [N/crypto](N_crypto.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`keyControl.createKey(options)`](#keycontrolcreatekeyoptions) | function | Register a key from a PEM file in the File Cabinet |
| [`keyControl.loadKey(options)`](#keycontrolloadkeyoptions) | function | Load a key by script ID |
| [`keyControl.deleteKey(options)`](#keycontroldeletekeyoptions) | function | Delete a key |
| [`keyControl.findKeys(options)`](#keycontrolfindkeysoptions) | function | Search key metadata |
| [`keyControl.lock(options)`](#keycontrollockoptions) | function | Block UI edits of a key |
| [`keyControl.unlock(options)`](#keycontrolunlockoptions) | function | Undo a lock |
| [`Key`](#key-object) | object | A key record |
| [`keyControl.Operator`](#keycontroloperator) | enum | Text-matching operators for `findKeys` |

## Functions

### keyControl.createKey(options)
<!-- id: N/keyControl.createKey -->

Creates a key record from a key file that is already in the File Cabinet and returns it as a
[`Key`](#key-object). Call [`Key.save()`](#keysave) on the result to store it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | number | Yes | |
| `options.password` | string | No | |
| `options.scriptId` | string | No | |
| `options.restrictions` | number[] \| string[] | No | |
| `options.name` | string | Yes | |
| `options.description` | string | No | |

- `options.file`: the internal ID of the File Cabinet file holding the key, in PEM format.
- `options.password`: the key's password, or the script ID of an API secret that holds it.
- `options.scriptId`: the `custkey` prefix is added for you; when omitted, a script ID is generated
  automatically.
- `options.restrictions`: internal IDs of employees; only those employees can use the key.
- All option fields exist since 2019.2.

**Returns:** `keyControl.Key`.

**Governance:** 10 units.

**Availability:** the source does not state the supported script types for this function; the
module as a whole is server-only.

> [!SOURCE-ERROR] The source omits the row that lists which script types support this function, so
> no availability is recorded for it. Expect server scripts, like the rest of the module, and verify
> in your account.
<!-- source-error: N/keyControl.createKey | - | supported_script_types_row_missing -->

**Since:** 2019.2

### keyControl.loadKey(options)
<!-- id: N/keyControl.loadKey -->

Loads an existing key by its script ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |

- `options.scriptId`: the key's script ID, as held in [`Key.scriptId`](#keyscriptid). Since 2019.2.

**Returns:** `Object`.

> [!SOURCE-ERROR] The function's own entry gives the return type as a plain `Object`, while the
> source's module table says it returns a `keyControl.Key`. The source does not settle which is
> right; check the returned value before relying on `Key` members.
<!-- source-error: N/keyControl.loadKey | - | loadkey_returns_object_vs_keycontrol_key_in_module_table -->

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

### keyControl.deleteKey(options)
<!-- id: N/keyControl.deleteKey -->

Deletes a key. The key is marked as deleted and its history remains.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string | Yes | |

- `options.scriptId`: the key's script ID, as held in [`Key.scriptId`](#keyscriptid). Since 2019.2.

**Returns:** `Object`. The source does not describe its contents.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

### keyControl.findKeys(options)
<!-- id: N/keyControl.findKeys -->

Returns metadata for the keys available to the current user. Without any criteria, all keys are
returned.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.restriction` | number | No | |
| `options.name` | string \| object | No | |
| `options.description` | string \| object | No | |

- `options.restriction`: the internal ID of an employee.
- `options.name` and `options.description`: either a plain string, or a filter object with
  `value`, `operator` and `ignoreCase` members. `operator` takes a value from
  [`keyControl.Operator`](#keycontroloperator). In the object form, `operator` defaults to equals and
  `ignoreCase` defaults to `true`, so leaving them out gives a case-insensitive exact match.
- All option fields exist since 2019.2.

**Returns:** `Object[]`, the metadata of the matching keys. Each result carries the key's script ID;
the source lists no other members.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

### keyControl.lock(options)
<!-- id: N/keyControl.lock -->

Locks a key so it cannot be edited in the UI until [`unlock`](#keycontrolunlockoptions) reverses
it. To find a key's ID, look in the private keys list on the company keys setup page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: the key's script ID or internal ID. Since 2021.1.

**Returns:** `void`

> [!SOURCE-ERROR] The function's own entry says it returns nothing, while the source's module table
> gives a string. The source does not settle which is right; do not rely on a return value.
<!-- source-error: N/keyControl.lock | - | returns_void_on_member_page_vs_string_in_module_table -->

**Errors:**

- `KEY_NOT_FOUND`: no key with that ID exists in the account.
- `ACCESS_TO_KEY_RESTRICTED`: access is denied because the key is restricted to other scripts or
  employees.
- `KEY_ALREADY_LOCKED`: the key is locked already.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.1

### keyControl.unlock(options)
<!-- id: N/keyControl.unlock -->

Unlocks a key that was locked with [`lock`](#keycontrollockoptions); it only applies to a key in
that locked state. To find a key's ID, look in the private keys list on the company keys setup page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: the key's script ID or internal ID. Since 2021.1.

**Returns:** `void`

> [!SOURCE-ERROR] As with `lock`, the function's own entry says it returns nothing, while the
> source's module table gives a string. Do not rely on a return value.
<!-- source-error: N/keyControl.unlock | - | returns_void_on_member_page_vs_string_in_module_table -->

**Errors:**

- `KEY_NOT_LOCKED`: the key is not currently locked.
- `KEY_NOT_FOUND`: no key with that ID exists in the account.
- `ACCESS_TO_KEY_RESTRICTED`: access is denied because the key is restricted to other scripts or
  employees.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2021.1

## Objects

### Key (object)
<!-- id: N/keyControl.Key -->

A key record: the key file plus its name, description, password reference and access list.
Returned by `createKey`. Server scripts; since 2019.2.

#### Key.save()
<!-- id: N/keyControl.Key#save -->

Stores the key record and gives back the key's script ID.

> [!SOURCE-ERROR] In the source's module member table, the row for this method describes the `Key`
> object's members instead of the save operation. The method itself saves the key.
<!-- source-error: N/keyControl.Key#save | - | module_row_says_key_object_members -->

**Returns:** `Object`, described as the key's script ID.

> [!SOURCE-ERROR] The return type is given as `Object`, while the description says the key's script
> ID is returned. The source does not settle which is right; check the actual value, or read
> [`Key.scriptId`](#keyscriptid) instead.
<!-- source-error: N/keyControl.Key#save | - | key_save_returns_object_vs_script_id -->

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Key.file
<!-- id: N/keyControl.Key#file -->

`file.File`. The key's file object (see [N/file](N_file.md)). Since 2019.2.

#### Key.password
<!-- id: N/keyControl.Key#password -->

`String`, write-only. Accepts either the GUID of a credential, as produced by a field added with
[`Form.addCredentialField`](N_ui_serverWidget.md), or the script ID of an API secret. It cannot be
read back. Since 2019.2.

#### Key.scriptId
<!-- id: N/keyControl.Key#scriptId -->

`string`. The key's script ID. It is returned by `Key.save` and by `findKeys`. Since 2019.2.

#### Key.name
<!-- id: N/keyControl.Key#name -->

`string`. The key's name. Since 2019.2.

#### Key.description
<!-- id: N/keyControl.Key#description -->

`string`. The key's description. Since 2019.2.

#### Key.restrictions
<!-- id: N/keyControl.Key#restrictions -->

`number[] | string[]`. A list of employee IDs; only those employees can access the key. Since
2019.2.

> [!SOURCE-ERROR] The source's sample assigns a single string to a member named in the singular
> (`restriction`) rather than an array to `restrictions`. The declared property is the plural
> `restrictions`, holding a list; assign an array of employee IDs and verify the result.
<!-- source-error: N/keyControl.Key#restrictions | - | sample_assigns_string_to_singular_restriction_member -->

## Enums

### keyControl.Operator
<!-- id: N/keyControl.Operator -->

Matching operators for the object form of the `name` and `description` filters of `findKeys`.
Since 2019.2.

| Value |
|---|
| `STARTS_WITH` |
| `CONTAINS` |
| `ENDS_WITH` |
| `EQUALS` |

## Examples

A scheduled script that registers a new SFTP key uploaded by the IT team, limits it to the
employees named in a script parameter, and locks it. The file ID, the API secret script ID for the
password and the employee IDs all come from script parameters.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/keyControl', 'N/runtime'], (keyControl, runtime) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const key = keyControl.createKey({
      file: Number(script.getParameter({ name: 'custscript_new_key_file' })),
      name: '3PL warehouse SFTP key',
      description: 'Inbound shipment confirmations',
      scriptId: '_threepl_sftp',
      password: script.getParameter({ name: 'custscript_key_secret_id' }),
      restrictions: String(script.getParameter({ name: 'custscript_key_users' })).split(',')
    });
    key.save();
    keyControl.lock({ id: key.scriptId });
  };

  return { execute };
});
```

Listing keys whose names start with a vendor prefix, using the filter-object form:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/keyControl'], (keyControl) => {
  const vendorKeys = (prefix) => keyControl.findKeys({
    name: { value: prefix, operator: keyControl.Operator.STARTS_WITH, ignoreCase: true }
  });

  return { vendorKeys };
});
```
