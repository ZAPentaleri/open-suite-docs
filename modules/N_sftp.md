# N/sftp
<!-- id: N/sftp -->

`N/sftp` lets a server script open an SFTP session to a remote server and transfer files over it with
SSH: upload, download, list, move and delete files, and create or remove directories. Authentication
can use a password (held as a credential GUID), an SSH key, an API secret, or a key combined with a
password or secret.

- **Availability:** server scripts only.
- **Permission:** when you authenticate with a public key, the "Key access" permission is needed
  (`LIST_KEYS` in the [permissions catalog](../gen/catalogs/permissions.md)).
- **Load it as:** `define(['N/sftp'], (sftp) => { ... })`
- Related: [N/keyControl](N_keyControl.md) (SSH keys), [N/crypto](N_crypto.md) (encrypting file
  content), [N/ui/serverWidget](N_ui_serverWidget.md) (credential fields), [N/https](N_https.md),
  [N/file](N_file.md), [RESTlet](../script-types/Restlet.md),
  [SFTP algorithm catalog](../gen/catalogs/sftp_algorithms.md).

## Scope and limits

- **SFTP only.** FTP, FTPS and SCP are not supported.
- **NetSuite is always the client.** NetSuite does not host SFTP servers, and external clients cannot
  start an SFTP transfer into NetSuite. When an outside system needs to push files in, use a RESTlet
  (binary file content then has to be base64-encoded) or the file record of SOAP web services instead.
- **Encryption.** SSH encrypts the session only; files are not encrypted at rest. A provider may
  require files to be encrypted at rest, may expect encrypted uploads, or may give you encrypted files
  to download; handle that yourself (see [N/crypto](N_crypto.md)).
- **Ciphers.** The remote server must support at least one of the encryption algorithms listed in the
  [SFTP algorithm catalog](../gen/catalogs/sftp_algorithms.md). If it supports none,
  `createConnection` fails with `FTP_UNSUPPORTED_ENCRYPTION_ALGORITHM`.

> [!SOURCE-ERROR] The source's introduction mentions only the AES CTR ciphers, although the algorithm
> list it also gives is longer. Use the full catalog, and check what the server actually negotiates,
> rather than assuming the CTR modes are the only option.
<!-- source-error: N/sftp | - | intro_lists_only_aes_ctr_ciphers -->

- **Firewall.** Any firewall policy on the remote side is configured by the SFTP provider.
- **Wildcards.** `?` and `*` act as wildcards; escape them in paths unless you mean them as patterns.
- **Files handled.** Both File Cabinet files and on-demand (in-memory) files can be uploaded. A
  downloaded file is on-demand; to keep it, give it a folder and save it.

## Host keys

- Host key checking is strict. The API offers no way to fetch the remote host key or to turn strict
  checking off, and you supply the host key with every connection.
- The host key verifies the server to NetSuite; it does not identify NetSuite to the server.
- When the server's key changes, you update it by hand. Ask your provider how they handle host key
  changes and failover.
- One way to obtain the key is to run `ssh-keyscan` against the server, giving the key type and port.

## Credentials

- **Methods:** user name and password; SSH key, with or without a passphrase; or password and key
  together.
- **Passwords** are collected through a credential field on a Suitelet form
  ([`Form.addCredentialField`](N_ui_serverWidget.md)), which produces a GUID; that GUID is what you
  pass to `createConnection` as `passwordGuid`. Encrypted custom fields are not supported for this.
- The credential field's domain restriction must include the SFTP server's domain, and its script ID
  restriction must include the script that connects.
- A credential field holds at most 32 characters. A credential can be restricted to the user who saved
  it.
- The GUID token has no relation to the password value itself. Passwords stored this way cannot be
  expired automatically, and saved credentials cannot be listed in the UI.
- **SSH keys** are uploaded in PEM format on the company keys setup page (see
  [N/keyControl](N_keyControl.md)); you reference one by its script ID (`keyId`).
- **API secrets** are stored on the company API secrets setup page; you reference one by its script
  ID (`secret`).

## Suggested setup sequence

1. Get the connection properties from the provider: user name, password or key, URL, port,
   directories, host key and host key type.
2. Check that the server supports one of the accepted ciphers.
3. Build a credential Suitelet: custom fields for the user name and the token; the GET request draws
   the form and the POST request saves the token.
4. Test the connection.
5. Write the server script that does the transfers.

Typical uses are uploading a File Cabinet file to the remote server and downloading a remote file into
the File Cabinet.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`sftp.createConnection(options)`](#sftpcreateconnectionoptions) | function | Open a connection to an SFTP server |
| [`Connection`](#connection-object) | object | An open SFTP session |
| [`sftp.Sort`](#sftpsort) | enum | Sort orders for `Connection.list` |
| [`sftp.DEFAULT_PORT_NUMBER`](#sftpdefault_port_number) | constant | 22 |
| [`sftp.MIN_PORT_NUMBER`](#sftpmin_port_number) | constant | 0 |
| [`sftp.MAX_PORT_NUMBER`](#sftpmax_port_number) | constant | 65535 |
| [`sftp.MIN_CONNECT_TIMEOUT`](#sftpmin_connect_timeout) | constant | 1 second |
| [`sftp.MAX_CONNECT_TIMEOUT`](#sftpmax_connect_timeout) | constant | 20 seconds |
| [`Connection.MAX_FILE_SIZE`](#connectionmax_file_size) | constant | 100,000,000 bytes |
| [`Connection.MAX_TRANSFER_TIMEOUT`](#connectionmax_transfer_timeout) | constant | 300 seconds |

## Functions

### sftp.createConnection(options)
<!-- id: N/sftp.createConnection -->

Opens a connection to a remote SFTP server and returns a [`Connection`](#connection-object).

Authentication rules:

- Supply at least one of `passwordGuid`, `keyId` or `secret` (password GUID, SSH key or API secret).
- For a two-factor login, combine `keyId` with either `passwordGuid` or `secret`.
- `passwordGuid` and `secret` cannot be used together.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.passwordGuid` | string | No | |
| `options.keyId` | string | No | |
| `options.secret` | string | No | |
| `options.hostKey` | string | Yes | |
| `options.username` | string | Yes | |
| `options.directory` | string | No | |
| `options.timeout` | number | No | `20` |
| `options.hostKeyType` | string | No | |
| `options.port` | number | No | `22` |

- `options.url`: the remote host name. Since 2016.2.
- `options.passwordGuid`: conditionally required (see the rules above). The GUID of a credential
  field added with `Form.addCredentialField` in a Suitelet, whose value the script obtains over
  HTTPS ([N/https](N_https.md)). Since 2016.2.
- `options.keyId`: conditionally required. The script ID of an SSH key uploaded in PEM format (see
  [N/keyControl](N_keyControl.md)). Since 2019.2.
- `options.secret`: conditionally required. The script ID of an API secret. Since 2021.1.
- `options.hostKey`: the fingerprint of the server you trust, base64-encoded; a typical value is a
  base64 string of 96 bytes or more. Since 2016.2.
- `options.username`: the remote user name; the script ID of an API secret is also accepted. When no
  user name is given, the login is anonymous. Since 2016.2.

> [!SOURCE-ERROR] The source marks `username` as required and at the same time says an omitted user
> name means an anonymous login. It does not settle which holds; supply a user name unless your
> provider confirms anonymous access works.
<!-- source-error: N/sftp.createConnection | options.username | username_required_but_default_login_anonymous -->

- `options.directory`: the remote directory to start in. Required when the server cannot resolve
  relative paths. Since 2016.2.
- `options.timeout`: connection timeout in seconds, 1 to 20 (see
  [`MIN_CONNECT_TIMEOUT`](#sftpmin_connect_timeout) and [`MAX_CONNECT_TIMEOUT`](#sftpmax_connect_timeout)).
  Since 2016.2.
- `options.hostKeyType`: `dsa`, `ecdsa` or `rsa`. Since 2016.2.
- `options.port`: the remote port (see [`DEFAULT_PORT_NUMBER`](#sftpdefault_port_number) and the
  port range constants). Since 2016.2.
- Approved encryption algorithms include `aes256_ctr`, `aes192_ctr` and `aes128_ctr`; if the server
  supports none of the approved algorithms, the call fails.

> [!SOURCE-ERROR] Where the source lists these approved algorithms, two of them appear as `es192_ctr`
> and `es128_ctr`, without the leading "a". The intended names appear to be `aes192_ctr` and
> `aes128_ctr`; check the algorithm catalog.
<!-- source-error: N/sftp.createConnection | - | algorithm_names_given_as_es192_ctr_es128_ctr -->

**Returns:** `sftp.Connection`, an open connection.

**Errors:**

- `FTP_UNKNOWN_HOST`: the host cannot be found.
- `FTP_CONNECT_TIMEOUT_EXCEEDED`: the server did not answer within `options.timeout`.
- `FTP_CANNOT_ESTABLISH_CONNECTION`: the connection was refused.
- `FTP_INVALID_PORT_NUMBER`: the port value is not valid.
- `FTP_INVALID_CONNECTION_TIMEOUT`: `options.timeout` is outside the allowed range.
- `FTP_INVALID_DIRECTORY`: the directory cannot be found.
- `FTP_INCORRECT_HOST_KEY`: `options.hostKey` is not a valid value for this server.
- `FTP_INCORRECT_HOST_KEY_TYPE`: the server does not accept the `options.hostKeyType` given.
- `FTP_MALFORMED_HOST_KEY`: `options.hostKey` is not in a supported form.
- `FTP_PERMISSION_DENIED`: access was denied.
- `FTP_UNSUPPORTED_ENCRYPTION_ALGORITHM`: the server supports none of the approved algorithms.
- `AUTHENTICATION_FAIL_TOO_MANY_INCORRECT_AUTHENTICATION_ATTEMPTS`: authentication failed after too
  many incorrect attempts.
- `NO_ROUTE_TO_HOST_FOUND`: the host cannot be reached.
- `CONNECTION_RESET`: the remote side reset the connection.
- `CONNECTION_CLOSED_BY_HOST`: the remote host closed the connection.
- `THE_REMOTE_PATH_FOR_FILE_IS_NOT_VALID`: a remote path argument is not valid.
- `SFTPCREDENTIAL_ENCODING_ERROR`: authentication failed because of a credential encoding problem.
- `UNABLE_TO_GET_SFTP_SERVER_ADDRESS`: the server address could not be obtained.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `options.secret` and `options.passwordGuid` were given.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.2

## Objects

### Connection (object)
<!-- id: N/sftp.Connection -->

A connection to a remote SFTP account, returned by
[`sftp.createConnection(options)`](#sftpcreateconnectionoptions). The paths you pass to its methods
are relative paths. Server scripts; since 2016.2.

#### Connection.download(options)
<!-- id: N/sftp.Connection#download -->

Fetches a remote file and returns it as an on-demand [`file.File`](N_file.md). The file exists only in
memory; to keep it, set its folder and save it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.directory` | string | No | |
| `options.timeout` | number | No | `300` |
| `options.filename` | string | Yes | |

- `options.directory`: relative path of the remote directory; the current directory when omitted.
  Since 2016.2.
- `options.timeout`: transfer timeout in seconds, 1 to 300 (see
  [`MAX_TRANSFER_TIMEOUT`](#connectionmax_transfer_timeout)). Since 2016.2.
- `options.filename`: the name of the remote file. Since 2016.2.

**Returns:** `file.File`, an on-demand file.

**Errors:**

- `FTP_MAXIMUM_FILE_SIZE_EXCEEDED`: the file is larger than the maximum size (see
  [`MAX_FILE_SIZE`](#connectionmax_file_size)).
- `FTP_NO_SUCH_FILE_OR_DIRECTORY`: the path does not exist.
- `FTP_TRANSFER_TIMEOUT_EXCEEDED`: the transfer ran past `options.timeout`.
- `FTP_INVALID_TRANSFER_TIMEOUT`: `options.timeout` is not between 1 and 300.
- `FTP_PERMISSION_DENIED`: the remote server refused access.
- `CONNECTION_RESET`: the connection was reset.
- `THE_REMOTE_PATH_FOR_FILE_IS_NOT_VALID`: the remote path is not valid.
- `CONNECTION_CLOSED_BY_HOST`: the host closed the connection.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2016.2

#### Connection.list(options)
<!-- id: N/sftp.Connection#list -->

Lists the contents of a remote directory, at most 5000 entries. `?` and `*` in the path work as
wildcards.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.path` | String | Yes | |
| `options.sort` | String | Yes | |

- `options.path`: relative path of the directory to list. Since 2019.2.
- `options.sort`: a value from [`sftp.Sort`](#sftpsort). Since 2019.2.

> [!SOURCE-ERROR] The source marks `options.sort` as required, yet its own syntax sample calls
> `list` without it. The spec does not say whether the call works without a sort order; pass one to
> be safe.
<!-- source-error: N/sftp.Connection#list | - | sort_marked_required_but_omitted_in_syntax_sample -->

**Returns:** `Array<Object>`, one entry per item with the fields `directory`, `name`, `size` and
`lastModified`. `directory` is a boolean that tells folders from files; the source does not state
the types of the other fields.

**Errors:**

- `FTP_INVALID_DIRECTORY`: the directory does not exist.
- `FTP_PERMISSION_DENIED`: the remote server refused access.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Connection.makeDirectory(options)
<!-- id: N/sftp.Connection#makeDirectory -->

Creates an empty directory on the remote server. Its parent directory must already exist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.path` | string | Yes | |

- `options.path`: relative path of the new directory. Since 2019.2.

**Returns:** `void`

**Errors:**

- `FTP_PERMISSION_DENIED`: the remote server refused access.
- `FTP_DIRECTORY_NOT_FOUND`: the parent directory does not exist.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Connection.move(options)
<!-- id: N/sftp.Connection#move -->

Moves a remote file or directory from one relative path to another.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.from` | String | Yes | |
| `options.to` | String | Yes | |

- `options.from`: relative path of the item to move. Since 2019.2.
- `options.to`: relative destination path. Since 2019.2.

**Returns:** `void`

**Errors:**

- `FTP_INVALID_MOVE`: the source or the destination cannot be found or accessed.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Connection.removeDirectory(options)
<!-- id: N/sftp.Connection#removeDirectory -->

Deletes a remote directory. It works only on an empty directory.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.path` | String | Yes | |

- `options.path`: relative path of the directory. Since 2019.2.

**Returns:** `void`

**Errors:**

- `FTP_DIRECTORY_NOT_EMPTY`: the directory still has contents.
- `FTP_PERMISSION_DENIED`: the remote server refused access.
- `FTP_DIRECTORY_NOT_FOUND`: the directory does not exist.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Connection.removeFile(options)
<!-- id: N/sftp.Connection#removeFile -->

Deletes a remote file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.path` | String | Yes | |

- `options.path`: relative path of the file. Since 2019.2.

**Returns:** `void`

**Errors:**

- `FTP_PERMISSION_DENIED`: the remote server refused access.
- `FTP_NO_SUCH_FILE_OR_DIRECTORY`: the path does not exist.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

#### Connection.upload(options)
<!-- id: N/sftp.Connection#upload -->

Sends a file to the remote server. The file can be a File Cabinet file or an on-demand file, up to
100 MB.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.filename` | string | No | |
| `options.directory` | string | No | |
| `options.timeout` | number | No | `300` |
| `options.replaceExisting` | boolean | No | `false` |
| `options.file` | file.File | Yes | |

- `options.filename`: the remote file name; defaults to the name of `options.file`. Characters that
  are not allowed are escaped. Since 2016.2.
- `options.directory`: relative path of the target directory; the current directory when omitted.
  Since 2016.2.
- `options.timeout`: seconds, 1 to 300. It applies when no data is received for that long; it is not
  a limit on the whole transfer. Since 2016.2.
- `options.replaceExisting`: when `false`, uploading to a name that already exists raises
  `FTP_FILE_ALREADY_EXISTS`. Since 2016.2.
- `options.file`: the file to send. Since 2016.2.
- Whether the existence check uses the list command is governed by an account preference for using
  list to test whether a file exists, found among the general account preferences. The source gives
  that preference only in descriptive form, not its exact label. A server without the stat command
  can also cause `FTP_FILE_ALREADY_EXISTS`.

**Returns:** `void`

**Errors:**

- `FTP_NO_SUCH_FILE_OR_DIRECTORY`: the path does not exist.
- `FTP_TRANSFER_TIMEOUT_EXCEEDED`: the transfer ran past `options.timeout`.
- `FTP_INVALID_TRANSFER_TIMEOUT`: `options.timeout` is not between 1 and 300.
- `FTP_FILE_ALREADY_EXISTS`: the name exists and `options.replaceExisting` is `false`, or the server
  lacks the stat command.
- `CONNECTION_RESET`: the connection was reset.
- `THE_REMOTE_PATH_FOR_FILE_IS_NOT_VALID`: the remote path is not valid.
- `CONNECTION_CLOSED_BY_HOST`: the host closed the connection.
- `FTP_PERMISSION_DENIED`: the remote server refused access.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2016.2

## Enums

### sftp.Sort
<!-- id: N/sftp.Sort -->

Sort orders for `options.sort` of [`Connection.list(options)`](#connectionlistoptions). The spec gives
the names only; the descriptions below are read from the names. Since 2019.2.

| Value | Order |
|---|---|
| `DATE` | By date |
| `DATE_DESC` | By date, descending |
| `SIZE` | By size |
| `SIZE_DESC` | By size, descending |
| `NAME` | By name |
| `NAME_DESC` | By name, descending |

## Constants

### sftp.DEFAULT_PORT_NUMBER
<!-- id: N/sftp.DEFAULT_PORT_NUMBER -->

`number`, value `22`. The default port number. Since 2019.2.

### sftp.MIN_PORT_NUMBER
<!-- id: N/sftp.MIN_PORT_NUMBER -->

`number`, value `0`. The lowest port number. Since 2019.2.

### sftp.MAX_PORT_NUMBER
<!-- id: N/sftp.MAX_PORT_NUMBER -->

`number`, value `65535`. The highest port number. Since 2019.2.

### sftp.MIN_CONNECT_TIMEOUT
<!-- id: N/sftp.MIN_CONNECT_TIMEOUT -->

`number`, value `1` (seconds). The shortest connection timeout. Since 2016.2.

### sftp.MAX_CONNECT_TIMEOUT
<!-- id: N/sftp.MAX_CONNECT_TIMEOUT -->

`number`, value `20` (seconds). The longest connection timeout. Since 2016.2.

### Connection.MAX_FILE_SIZE
<!-- id: N/sftp.Connection#MAX_FILE_SIZE -->

`number`, value `100000000` (bytes). The largest file size. Since 2019.2.

### Connection.MAX_TRANSFER_TIMEOUT
<!-- id: N/sftp.Connection#MAX_TRANSFER_TIMEOUT -->

`number`, value `300` (seconds). The longest transfer timeout. Since 2019.2.

## Examples

Every example reads connection details from script parameters. The password GUID comes from a
credential field (see the last example), keys and secrets are referenced by script ID, and an
administrator enters the host key in a parameter; no secret value appears in code.

A scheduled script that sends the day's payment file from the File Cabinet to a bank's SFTP drop
folder, refusing to overwrite an existing file of the same name:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/sftp', 'N/file', 'N/runtime', 'N/log'], (sftp, file, runtime, log) => {
  const param = (name) => runtime.getCurrentScript().getParameter({ name });

  const execute = () => {
    const connection = sftp.createConnection({
      url: param('custscript_bank_sftp_host'),
      username: param('custscript_bank_sftp_user'),
      keyId: param('custscript_bank_sftp_key_id'),
      hostKey: param('custscript_bank_sftp_host_key'),
      hostKeyType: 'rsa',
      port: sftp.DEFAULT_PORT_NUMBER,
      timeout: 15
    });

    const payments = file.load({ id: param('custscript_bank_payment_file') });
    connection.upload({
      file: payments,
      directory: 'inbound/payments',
      filename: `pay_${Date.now()}.csv`,
      replaceExisting: false
    });
    log.audit({ title: 'Payment file sent', details: payments.name });
  };

  return { execute };
});
```

A Map/Reduce script whose input stage lists a supplier's outbox, newest first; each map call
downloads one price list, saves it to a File Cabinet folder and moves the remote copy to an archive
directory:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/sftp', 'N/runtime'], (sftp, runtime) => {
  const param = (name) => runtime.getCurrentScript().getParameter({ name });

  const connect = () => sftp.createConnection({
    url: param('custscript_supplier_host'),
    username: param('custscript_supplier_user'),
    passwordGuid: param('custscript_supplier_pwd_guid'),
    hostKey: param('custscript_supplier_host_key'),
    directory: 'outbox'
  });

  const getInputData = () => {
    const entries = connect().list({ path: '.', sort: sftp.Sort.DATE_DESC });
    return entries
      .filter((entry) => !entry.directory && entry.name.endsWith('.xml'))
      .map((entry) => entry.name);
  };

  const map = (context) => {
    const name = context.value;
    const connection = connect();
    const priceList = connection.download({ filename: name, timeout: 120 });
    priceList.folder = Number(param('custscript_pricelist_folder'));
    priceList.save();
    connection.move({ from: name, to: `archive/${name}` });
  };

  return { getInputData, map };
});
```

A credential Suitelet: the GET request draws a form with a credential field limited to the SFTP host
and to the transfer script, and the POST request stores the resulting GUID (never the password) on a
custom record that the transfer script reads. The record type, field IDs and script IDs are invented
for this example.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/record'], (serverWidget, record) => {
  const onRequest = (context) => {
    if (context.request.method === 'GET') {
      const form = serverWidget.createForm({ title: 'Supplier SFTP login' });
      form.addCredentialField({
        id: 'custpage_sftp_pwd',
        label: 'SFTP password',
        restrictToDomains: ['sftp.supplier.example'],
        restrictToScriptIds: ['customscript_supplier_pricelist_mr'],
        restrictToCurrentUser: false
      });
      form.addSubmitButton({ label: 'Save' });
      context.response.writePage({ pageObject: form });
      return;
    }

    const guid = context.request.parameters.custpage_sftp_pwd;
    record.submitFields({
      type: 'customrecord_sftp_profile',
      id: 1,
      values: { custrecord_sftp_pwd_guid: guid }
    });
    context.response.write({ output: 'Credential saved.' });
  };

  return { onRequest };
});
```
