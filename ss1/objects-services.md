# Service and request objects (SuiteScript 1.0)

> **Legacy API.** This page describes the SuiteScript 1.0 service objects (context, files, errors,
> email merge, CSV import, duplicate merging, configuration, credentials, pivot reports) and the
> Suitelet request and response objects, so that tools can read, convert and minimally repair
> existing 1.0 scripts. It is not a guide for new development; the
> [1.0 to 2.1 conversion guide](../conversion/ss1-to-ss21.md) maps each member to 2.x.

Related pages: [1.0 Suitelets](../topics/ss1_api.suitelets.md),
[1.0 scheduled scripts](../topics/ss1_api.scheduled_scripts.md),
[record and search objects](objects-record-search.md),
[forms and fields](objects-forms.md),
[assistants, lists and portlets](objects-ui-other.md).

How to read this page:

- 1.0 methods take **positional** arguments. Parameter tables list them in call order; keep that
  order when repairing or converting a call.
- Usage units: every member costs 0 units except `nlobjEmailMerger.merge` (20),
  `nlobjJobManager.submit` (100), `nlobjJobManager.getFuture` (5) and `nlobjResponse.renderPDF` (10).
- Releases: the source gives no release ("since") for most of these objects as a whole (only
  `nlobjReportForm` has one); each method shows the **Since:** release the source gives for it, and
  says so where it gives none.
- The pivot report objects (`nlobjPivot*`, `nlobjReport*`) have no 1.0 to 2.x mapping entries in the
  source, so the conversion guide cannot name a 2.x target for them.

## Objects

### nlobjCredentialBuilder (object)
<!-- id: ss1:nlobjCredentialBuilder -->

Builds the request string for `nlapiRequestURLWithCredentials`, so a script can assemble values that
include stored credentials without ever seeing them in clear text. Created with `new`. Available in
user event, scheduled, portlet and Suitelet scripts only. When the built value is used as a URL,
pass it without encoding or hashing it; otherwise URL validation fails.

#### new nlobjCredentialBuilder(string, domainString)
<!-- id: ss1:nlobjCredentialBuilder#constructor -->

Creates a builder.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `string` | string | Yes | |
| `domainString` | string | Yes | |

- `string`: the request string; it may embed the GUID of a stored credential.
- `domainString`: must match the host name of the URL exactly.

**Returns:** the source states no return value; the object is created with `new`.

**Governance:** 0 units.

The source states no release for the constructor.

#### nlobjCredentialBuilder.replace(string1, string2)
<!-- id: ss1:nlobjCredentialBuilder#replace -->

Replaces every occurrence of `string1` with `string2` in the builder's content.

> [!SOURCE-ERROR] The source maps this method to a 2.x target in one place and lists it as having no
> 2.x equivalent in another. Check the conversion guide and test before relying on a mapping.
<!-- source-error: ss1:nlobjCredentialBuilder#replace | - | credentialbuilder_replace_mapped_vs_listed_unmapped -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `string1` | string | Yes | |
| `string2` | string | Yes | |

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjCredentialBuilder.append(object)
<!-- id: ss1:nlobjCredentialBuilder#append -->

Appends another value to the builder.

> [!SOURCE-ERROR] The source's heading for this method names its argument differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjCredentialBuilder#append | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `object` | Object | Yes | |

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjCredentialBuilder.base64()
<!-- id: ss1:nlobjCredentialBuilder#base64 -->

Base64-encodes the builder's content.

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjCredentialBuilder.sha256()
<!-- id: ss1:nlobjCredentialBuilder#sha256 -->

Hashes the builder's content with SHA-256.

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjCredentialBuilder.utf8()
<!-- id: ss1:nlobjCredentialBuilder#utf8 -->

Encodes the builder's content as UTF-8.

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2013.2

#### nlobjCredentialBuilder.md5()
<!-- id: ss1:nlobjCredentialBuilder#md5 -->

Hashes the builder's content with MD5.

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2015.1

#### nlobjCredentialBuilder.sha1()
<!-- id: ss1:nlobjCredentialBuilder#sha1 -->

Hashes the builder's content with SHA-1.

**Returns:** `nlobjCredentialBuilder`

**Governance:** 0 units.

**Since:** 2013.2

### nlobjEmailMerger (object)
<!-- id: ss1:nlobjEmailMerger -->

Runs a mail merge with a scriptable email template; created by `nlapiCreateEmailMerger`. Templates
can merge contact, case, customer, employee, partner and vendor records, every transaction type and
every custom record. Server scripts only.

> [!SOURCE-ERROR] The source's API map gives this object the same 2.x target as `nlobjMergeResult`
> (`N/render.EmailMergeResult`), so the two objects share one target. Do not treat that target as a
> replacement for the merger itself; check the conversion guide.
<!-- source-error: ss1:nlobjEmailMerger | - | api_map_target_same_as_nlobjmergeresult -->

#### nlobjEmailMerger.setEntity(entityType, entityId)
<!-- id: ss1:nlobjEmailMerger#setEntity -->

Sets the entity whose fields fill the template.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `entityType` | string | Yes | |
| `entityId` | number | Yes | |

- `entityType`: `customer`, `contact`, `partner`, `vendor` or `employee`.

**Returns:** `void`

**Errors:**

- `SSS_MERGER_ERROR_OCCURRED`: the operation failed.
- `SSS_INVALID_TYPE_ARG`: an argument value is invalid.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.1

#### nlobjEmailMerger.setRecipient(recipientType, recipientId)
<!-- id: ss1:nlobjEmailMerger#setRecipient -->

Sets a second entity, the recipient, whose fields can also be merged.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `recipientType` | string | Yes | |
| `recipientId` | number | Yes | |

- `recipientType`: `customer`, `contact`, `partner`, `vendor` or `employee`.

**Returns:** `void`

**Errors:**

- `SSS_MERGER_ERROR_OCCURRED`: the operation failed.
- `SSS_INVALID_TYPE_ARG`: an argument value is invalid.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.1

#### nlobjEmailMerger.merge()
<!-- id: ss1:nlobjEmailMerger#merge -->

Performs the merge. When converting: in 2.x the merge happens automatically inside
`N/render.mergeEmail`, so there is no separate call to make.

**Returns:** `nlobjMergeResult`

**Errors:**

- `SSS_MERGER_ERROR_OCCURRED`: the operation failed.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.1

#### nlobjEmailMerger.setTransaction(transactionId)
<!-- id: ss1:nlobjEmailMerger#setTransaction -->

Sets the transaction whose fields fill the template.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `transactionId` | number | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_MERGER_ERROR_OCCURRED`: the operation failed.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.1

#### nlobjEmailMerger.setCustomRecord(recordType, recordId)
<!-- id: ss1:nlobjEmailMerger#setCustomRecord -->

Sets the custom record whose fields fill the template.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `recordType` | string | Yes | |
| `recordId` | number | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_INVALID_TYPE_ARG`: an argument value is invalid.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.1

#### nlobjEmailMerger.setSupportCase(caseId)
<!-- id: ss1:nlobjEmailMerger#setSupportCase -->

Sets the support case whose fields fill the template.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `caseId` | number | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.1

### nlobjMergeResult (object)
<!-- id: ss1:nlobjMergeResult -->

The outcome of `nlobjEmailMerger.merge`: the merged subject and body. Server scripts only.

#### nlobjMergeResult.getBody()
<!-- id: ss1:nlobjMergeResult#getBody -->

Returns the merged email body.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2015.1

#### nlobjMergeResult.getSubject()
<!-- id: ss1:nlobjMergeResult#getSubject -->

Returns the merged email subject.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2015.1

### nlobjConfiguration (object)
<!-- id: ss1:nlobjConfiguration -->

An account configuration page, returned by `nlapiLoadConfiguration` and saved back with
`nlapiSubmitConfiguration`.

#### nlobjConfiguration.getAllFields()
<!-- id: ss1:nlobjConfiguration#getAllFields -->

Returns the names of all fields on the configuration page.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.getFieldText(name)
<!-- id: ss1:nlobjConfiguration#getFieldText -->

Returns the display text of a select field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`, or null when the field does not exist or access to it is restricted.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.getFieldTexts(name)
<!-- id: ss1:nlobjConfiguration#getFieldTexts -->

Returns the display texts of a multiselect field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `Array`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.getFieldValue(name)
<!-- id: ss1:nlobjConfiguration#getFieldValue -->

Returns a field's value; for a select field, the internal ID as a string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.getFieldValues(name)
<!-- id: ss1:nlobjConfiguration#getFieldValues -->

Returns the values of a multiselect field as a read-only array.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string[]`, or null when the field is not on the page.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.getType()
<!-- id: ss1:nlobjConfiguration#getType -->

Returns the configuration type, such as `accountingpreferences` or `taxperiods`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.setFieldText(name, text)
<!-- id: ss1:nlobjConfiguration#setFieldText -->

Sets a select field by its display text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `text` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.setFieldTexts(name, texts)
<!-- id: ss1:nlobjConfiguration#setFieldTexts -->

Sets a multiselect field by display texts.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjConfiguration#setFieldTexts | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `texts` | string[] | Yes | |

- `texts`: an array of display values.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.setFieldValues(name, value)
<!-- id: ss1:nlobjConfiguration#setFieldValues -->

Sets a multiselect field from an array of values.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.getField(fldnam)
<!-- id: ss1:nlobjConfiguration#getField -->

Returns the field object of a configuration field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fldnam` | string | Yes | |

**Returns:** `nlobjField` (see [objects-forms.md](objects-forms.md)).

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjConfiguration.setFieldValue(name, value)
<!-- id: ss1:nlobjConfiguration#setFieldValue -->

Sets a field's value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

### nlobjContext (object)
<!-- id: ss1:nlobjContext -->

Information about the current user and about the script's execution context, returned by
`nlapiGetContext`. Script parameters are read through it with `getSetting('SCRIPT', name)`.

#### nlobjContext.getBundleId()
<!-- id: ss1:nlobjContext#getBundleId -->

Returns the ID of the bundle the script belongs to.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2016.1

#### nlobjContext.getColorPreferences()
<!-- id: ss1:nlobjContext#getColorPreferences -->

Deprecated since 2014.2. The source states neither a return type nor what the method returns.

**Returns:** not stated in the source.

**Governance:** 0 units.

**Deprecated:** since 2014.2. The source states no release for its introduction.

#### nlobjContext.getCompany()
<!-- id: ss1:nlobjContext#getCompany -->

Returns the account ID as a string, for example `NL555ABC`.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getDepartment()
<!-- id: ss1:nlobjContext#getDepartment -->

Returns the current user's department ID as an integer.

**Returns:** `number`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getEmail()
<!-- id: ss1:nlobjContext#getEmail -->

Returns the current user's email address. The user's employee record must have an email address.
It also works for a shopper who is recognized but not logged in.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getEnvironment()
<!-- id: ss1:nlobjContext#getEnvironment -->

Returns the environment type: `SANDBOX`, `PRODUCTION`, `BETA` or `INTERNAL`.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getExecutionContext()
<!-- id: ss1:nlobjContext#getExecutionContext -->

Returns what triggered the script: `userinterface`, `webservices`, `csvimport`, `portlet`,
`scheduled`, `suitelet`, `custommassupdate`, `workflow`, `webstore` or `userevent`. Several of these
need explaining: `userinterface` covers client scripts and user events fired from the UI;
`userevent` covers records generated in the back end; `webstore` covers user events fired from the
web store; `custommassupdate` means a mass update script.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getFeature(name)
<!-- id: ss1:nlobjContext#getFeature -->

Returns `true` when the named feature is enabled. The valid names are those listed on the account's
Enable Features page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjContext.getLocation()
<!-- id: ss1:nlobjContext#getLocation -->

Returns the current user's location ID as an integer.

**Returns:** `number`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getLogLevel()
<!-- id: ss1:nlobjContext#getLogLevel -->

Returns the script's log level: `DEBUG`, `AUDIT`, `ERROR` or `EMERGENCY`. Not supported in client
scripts.

**Returns:** `string`

**Governance:** 0 units.

**Availability:** not client scripts.

**Since:** 2008.2

#### nlobjContext.getName()
<!-- id: ss1:nlobjContext#getName -->

Returns the current user's name. It also works for a shopper who is recognized but not logged in.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getPercentComplete()
<!-- id: ss1:nlobjContext#getPercentComplete -->

Returns the percent complete of a scheduled script, the same figure the scheduled script status
page shows.

**Returns:** `number`

**Governance:** 0 units.

**Availability:** scheduled scripts only.

**Since:** 2009.1

#### nlobjContext.getPermission(name)
<!-- id: ss1:nlobjContext#getPermission -->

Returns the current user's level for a permission: 4 (FULL), 3 (EDIT), 2 (CREATE), 1 (VIEW) or
0 (NONE). A script that runs as administrator gets 4; the source states that under `getPreference`
(see the note there).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjContext.getPreference(name)
<!-- id: ss1:nlobjContext#getPreference -->

Reads a general or accounting preference. A checkbox preference comes back as `T` or `F`, a dropdown
as a string. To change company or accounting preferences, load them with `nlapiLoadConfiguration`
(`companypreferences`, `accountingpreferences`).

> [!SOURCE-ERROR] The source says here that `nlapiLoadConfiguration` returns an `nlobjRecord`, while
> elsewhere it returns an `nlobjConfiguration`. Use the `nlobjConfiguration` methods on its result.
<!-- source-error: ss1:nlobjContext#getPreference | - | says_nlapiloadconfiguration_returns_nlobjrecord -->

> [!SOURCE-ERROR] A note about permission levels (a script running as administrator gets level 4)
> sits under this method in the source, although it belongs to `getPermission`.
<!-- source-error: ss1:nlobjContext#getPreference | - | permission_level_note_placed_under_getpreference -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjContext.getQueueCount()
<!-- id: ss1:nlobjContext#getQueueCount -->

Returns the number of scheduling queues: 1 without a SuiteCloud Plus license, 5 with one license
(multiples of 5 per license). Useful for spreading load over several deployments.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjContext.getRemainingUsage()
<!-- id: ss1:nlobjContext#getRemainingUsage -->

Returns the usage units the script still has, as an integer.

**Returns:** `number`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getRole()
<!-- id: ss1:nlobjContext#getRole -->

Returns the current user's role ID as a string.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2007.1

#### nlobjContext.getRoleCenter()
<!-- id: ss1:nlobjContext#getRoleCenter -->

Returns the center type of the current role, such as `SALES`, `ACCOUNTING` or `CLASSIC`; it can also
return the ID of a custom center.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjContext.getRoleId()
<!-- id: ss1:nlobjContext#getRoleId -->

Returns the script ID of the current role. This helps with bundled roles, whose internal IDs change
from one account to another.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjContext.getSetting(type, name)
<!-- id: ss1:nlobjContext#getSetting -->

Reads a setting of the given type. This is how 1.0 scripts read their script parameters
(`type` = `SCRIPT`).

> [!SOURCE-ERROR] The source misspells the object name as `nlobContext` [sic] in this method's
> entry. The object is `nlobjContext`.
<!-- source-error: ss1:nlobjContext#getSetting | - | object_name_written_nlobcontext -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `name` | string | Yes | |

- `type`: `SESSION` (a per-session variable, server only; not supported in client scripts),
  `FEATURE` (returns `T` or `F`), `PERMISSION` (a level from 0 to 4) or `SCRIPT` (a script
  parameter).

**Returns:** `string | number`: an integer for `PERMISSION`, a string for `SCRIPT`, `SESSION` and
`FEATURE`.

**Governance:** 0 units.

The source states no release for this method.

#### nlobjContext.getSubsidiary()
<!-- id: ss1:nlobjContext#getSubsidiary -->

Returns the current user's subsidiary ID as an integer.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2007.1

#### nlobjContext.getUser()
<!-- id: ss1:nlobjContext#getUser -->

Returns the current user's ID as a string.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2007.1

#### nlobjContext.getVersion()
<!-- id: ss1:nlobjContext#getVersion -->

Returns the account's version number, for example `2010.2`.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2010.2

#### nlobjContext.setPercentComplete(pct)
<!-- id: ss1:nlobjContext#setPercentComplete -->

Sets the percent complete of a scheduled script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `pct` | number | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** scheduled scripts only.

**Since:** 2009.1

#### nlobjContext.setSessionObject(name, value)
<!-- id: ss1:nlobjContext#setSessionObject -->

Stores a value for the rest of the current user's login session; meant as temporary storage before
the data is written somewhere permanent, such as a custom record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjContext.setSetting(type, name, value)
<!-- id: ss1:nlobjContext#setSetting -->

Sets a session variable. Supported in server scripts.

> [!SOURCE-ERROR] The source's text for this method says `getSessionObject` sets session variables;
> `setSessionObject` appears to be meant.
<!-- source-error: ss1:nlobjContext#setSetting | - | says_getsessionobject_sets_session_variables -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `name` | string | Yes | |
| `value` | string | Yes | |

- `type`: the only accepted value is `SESSION`.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

The source states no release for this method.

#### nlobjContext.getDeploymentId()
<!-- id: ss1:nlobjContext#getDeploymentId -->

Returns the script deployment ID.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjContext.getScriptId()
<!-- id: ss1:nlobjContext#getScriptId -->

Returns the script ID.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjContext.getSessionObject(name)
<!-- id: ss1:nlobjContext#getSessionObject -->

Reads a value stored with `setSessionObject`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

### nlobjCSVImport (object)
<!-- id: ss1:nlobjCSVImport -->

A CSV import job, returned by `nlapiCreateCSVImport` and run with `nlapiSubmitCSVImport`. One import
handles at most 25,000 records. Call `setMapping` first; calling `setPrimaryFile` before it causes an
error. See also [CSV import](../topics/csv_import.md).

#### nlobjCSVImport.setLinkedFile(sublist, file)
<!-- id: ss1:nlobjCSVImport#setLinkedFile -->

Adds a linked file for a multi-file import; call it once for each linked file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | string | Yes | |
| `file` | string | Yes | |

- `sublist`: the sublist's internal ID.
- `file`: an `nlobjFile` from `nlapiLoadFile`, or the raw CSV text.

> [!SOURCE-ERROR] The source types `file` as a string, but it also accepts an `nlobjFile`; its type
> list is incomplete.
<!-- source-error: ss1:nlobjCSVImport#setLinkedFile | file | type_cell_says_string_but_nlobjfile_accepted -->

**Returns:** `void`

**Errors:**

- `SSS_INVALID_CSV_CONTENT`: an argument value is invalid (the file content cannot be used).

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjCSVImport.setMapping(savedImport)
<!-- id: ss1:nlobjCSVImport#setMapping -->

Chooses the saved CSV import (the field mapping) to use. Call this first.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `savedImport` | string | Yes | |

- `savedImport`: the internal ID or the script ID of a saved import, as listed on the saved CSV
  imports page.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjCSVImport.setOption(option, value)
<!-- id: ss1:nlobjCSVImport#setOption -->

Sets an import option.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `option` | string | Yes | |
| `value` | string | Yes | |

- `option`: the only accepted value is `jobName`.
- `value`: the job name shown on the CSV import status page; without it, the name is built from the
  import type, the file name and the user's email.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjCSVImport.setPrimaryFile(file)
<!-- id: ss1:nlobjCSVImport#setPrimaryFile -->

Sets the main CSV file. Call `setMapping` before this.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `file` | string | Yes | |

- `file`: an `nlobjFile` from `nlapiLoadFile`, or the raw CSV text.

> [!SOURCE-ERROR] The source types `file` as a string, but it also accepts an `nlobjFile`; its type
> list is incomplete.
<!-- source-error: ss1:nlobjCSVImport#setPrimaryFile | file | type_cell_says_string_but_nlobjfile_accepted -->

**Returns:** `void`

**Errors:**

- `SSS_INVALID_CSV_CONTENT`: an argument value is invalid (the file content cannot be used).

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjCSVImport.setQueue(string)
<!-- id: ss1:nlobjCSVImport#setQueue -->

Overrides the queue preference for this one import; call it every time an override is needed.
Requires a SuiteCloud Plus license.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `string` | string | Yes | |

- `string`: a queue number from `'1'` to `'5'`; which numbers are valid depends on the license.

**Returns:** `void`

**Errors:**

- `SSS_INVALID_CSV_QUEUE`: an argument value is invalid (the queue number).

**Governance:** 0 units.

**Since:** 2014.1

### nlobjDuplicateJobRequest (object)
<!-- id: ss1:nlobjDuplicateJobRequest -->

A request to merge or otherwise handle duplicate records, returned by
`nlobjJobManager.createJobRequest`. A request covers at most 200 records. The source points to a
worked example of merging duplicates with the job manager; it is not reproduced here.

#### nlobjDuplicateJobRequest.setEntityType(entityType)
<!-- id: ss1:nlobjDuplicateJobRequest#setEntityType -->

Sets the entity type of the duplicates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `entityType` | constant | Yes | |

- `entityType`: `ENTITY_CUSTOMER`, `ENTITY_CONTACT`, `ENTITY_LEAD`, `ENTITY_PROSPECT`,
  `ENTITY_PARTNER` or `ENTITY_VENDOR`, passed as a constant of the request instance (for example
  `request.ENTITY_CUSTOMER`). `ENTITY_CUSTOMER` also takes in prospects and leads.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjDuplicateJobRequest.setMasterId(masterID)
<!-- id: ss1:nlobjDuplicateJobRequest#setMasterId -->

Sets the record that the duplicates are merged into.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `masterID` | string | Yes | |

- `masterID`: needed when the selection mode is `MASTERSELECTIONMODE_SELECT_BY_ID` and ignored in
  the other modes, although the source marks it required.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjDuplicateJobRequest.setMasterSelectionMode(mode)
<!-- id: ss1:nlobjDuplicateJobRequest#setMasterSelectionMode -->

Sets how the master record is chosen.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `mode` | string | Yes | |

- `mode`: `MASTERSELECTIONMODE_CREATED_EARLIEST`, `MASTERSELECTIONMODE_MOST_RECENT_ACTIVITY`,
  `MASTERSELECTIONMODE_MOST_POPULATED_FIELDS` or `MASTERSELECTIONMODE_SELECT_BY_ID`, passed as a
  constant of the request instance.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjDuplicateJobRequest.setOperation(operation)
<!-- id: ss1:nlobjDuplicateJobRequest#setOperation -->

Sets what to do with the duplicates.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `operation` | string | Yes | |

- `operation`: `OPERATION_MERGE`, `OPERATION_DELETE`, `OPERATION_MAKE_MASTER_PARENT` or
  `OPERATION_MARK_AS_NOT_DUPES`, passed as a constant of the request instance.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjDuplicateJobRequest.setRecords(dupeRecords)
<!-- id: ss1:nlobjDuplicateJobRequest#setRecords -->

Sets the duplicate records the job works on.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `dupeRecords` | Array | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

### nlobjError (object)
<!-- id: ss1:nlobjError -->

An error object, returned by `nlapiCreateError`.

#### nlobjError.getId()
<!-- id: ss1:nlobjError#getId -->

Returns the error's reference ID, the one shown in the deployment's execution log, the debugger's
execution log and the error email.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjError.getInternalId()
<!-- id: ss1:nlobjError#getInternalId -->

When the error was thrown in an `afterSubmit` user event, returns the internal ID of the record that
was submitted.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjError.getStackTrace()
<!-- id: ss1:nlobjError#getStackTrace -->

Returns the lines that locate where the error happened.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjError.getUserEvent()
<!-- id: ss1:nlobjError#getUserEvent -->

Returns the user event in which the error was thrown: `beforeLoad`, `beforeSubmit` or
`afterSubmit`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjError.getCode()
<!-- id: ss1:nlobjError#getCode -->

Returns the error code.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjError.getDetails()
<!-- id: ss1:nlobjError#getDetails -->

Returns the error's detail text.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjFile (object)
<!-- id: ss1:nlobjFile -->

A file, returned by `nlapiCreateFile`, `nlapiLoadFile`, `nlapiMergeRecord` and `nlapiPrintRecord`.

> [!SOURCE-ERROR] The source's list of this object's methods leaves out `getEncoding`, which has its
> own entry. It is documented below.
<!-- source-error: ss1:nlobjFile | - | method_list_omits_getencoding -->

#### nlobjFile.getEncoding()
<!-- id: ss1:nlobjFile#getEncoding -->

Returns the file's character encoding: `UTF-8`, `windows-1252`, `ISO-8859-1`, `GB18030`,
`SHIFT_JIS`, `MacRoman`, `GB2312` or `Big5`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2010.1

#### nlobjFile.getFolder()
<!-- id: ss1:nlobjFile#getFolder -->

Returns the internal ID of the file's folder as an integer.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.getId()
<!-- id: ss1:nlobjFile#getId -->

Returns the file's internal ID; only for files stored in the File Cabinet.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.getType()
<!-- id: ss1:nlobjFile#getType -->

Returns the file type, such as `PDF`, `CSV` or `PLAINTEXT`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.getURL()
<!-- id: ss1:nlobjFile#getURL -->

Returns the file's URL; only for files stored in the File Cabinet.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.getValue()
<!-- id: ss1:nlobjFile#getValue -->

Returns the file's contents; binary files come back base64-encoded. Works for files of up to 10 MB.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.isInactive()
<!-- id: ss1:nlobjFile#isInactive -->

Returns `true` when the file is inactive.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.isOnline()
<!-- id: ss1:nlobjFile#isOnline -->

Returns `true` when the file is available without login.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.setEncoding(encodingType)
<!-- id: ss1:nlobjFile#setEncoding -->

Sets the file's character encoding.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `encodingType` | string | Yes | |

- `encodingType`: `UTF-8`, `windows-1252`, `ISO-8859-1`, `GB18030`, `SHIFT_JIS`, `MacRoman`,
  `GB2312` or `Big5`, matched case-sensitively. `GB2312` and `Big5` cannot be used for new files.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2010.1

#### nlobjFile.setFolder(id)
<!-- id: ss1:nlobjFile#setFolder -->

Sets the folder the file is saved in.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | number | Yes | |

- `id`: the folder's internal ID, for example `10`, `-4` or `20`.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.setIsInactive(inactive)
<!-- id: ss1:nlobjFile#setIsInactive -->

Marks the file active or inactive.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `inactive` | boolean | Yes | |

- `inactive`: `true` hides the file from lists unless inactive items are being shown.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.setIsOnline(online)
<!-- id: ss1:nlobjFile#setIsOnline -->

Controls whether the file can be downloaded without login.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `online` | boolean | Yes | |

- `online`: `true` makes it downloadable without login.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.getDescription()
<!-- id: ss1:nlobjFile#getDescription -->

Returns the file's description.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.getName()
<!-- id: ss1:nlobjFile#getName -->

Returns the file's name.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjFile.getSize()
<!-- id: ss1:nlobjFile#getSize -->

Returns the file's size.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.setDescription(description)
<!-- id: ss1:nlobjFile#setDescription -->

Sets the file's description.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `description` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjFile.setName(name)
<!-- id: ss1:nlobjFile#setName -->

Sets the file's name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.1

### nlobjFuture (object)
<!-- id: ss1:nlobjFuture -->

The status of a submitted job, returned by `nlobjJobManager.getFuture`. The source points to a
worked example of merging duplicates with the job manager; it is not reproduced here.

#### nlobjFuture.isDone()
<!-- id: ss1:nlobjFuture#isDone -->

Returns `true` when the job has finished.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjFuture.isCancelled()
<!-- id: ss1:nlobjFuture#isCancelled -->

Tells whether the job was cancelled. For a merge-duplicate-records job it always returns `false`.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2013.1

### nlobjJobManager (object)
<!-- id: ss1:nlobjJobManager -->

Submits and tracks jobs such as duplicate-record merges; returned by `nlapiGetJobManager`. The
source points to a worked example of merging duplicates with the job manager; it is not reproduced
here.

> [!SOURCE-ERROR] The source's API map gives this object the same 2.x target as `nlobjFuture`, the
> status object (`N/task.EntityDeduplicationTaskStatus`). The manager submits jobs, so check the
> conversion guide for the 2.x way to submit.
<!-- source-error: ss1:nlobjJobManager | - | api_map_target_same_as_nlobjfuture_status_object -->

#### nlobjJobManager.submit(nlobjDuplicateJobRequest)
<!-- id: ss1:nlobjJobManager#submit -->

Submits a job request. The job is queued rather than run at once. At most 200 records per request.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `nlobjDuplicateJobRequest` | Object | Yes | |

**Returns:** the job ID of the submitted job. The source does not state its type.

**Governance:** 100 units.

**Since:** 2013.1

#### nlobjJobManager.getFuture()
<!-- id: ss1:nlobjJobManager#getFuture -->

Returns a status object for checking on a submitted job.

> [!SOURCE-ERROR] The source's reference lists no parameters for this method, yet its example passes
> the job ID returned by `submit`. When repairing a call, keep the job ID argument an existing script
> passes.
<!-- source-error: ss1:nlobjJobManager#getFuture | - | getfuture_ref_has_no_params_but_example_passes_jobid -->

**Returns:** `nlobjFuture`

**Governance:** 5 units.

**Since:** 2013.1

#### nlobjJobManager.createJobRequest()
<!-- id: ss1:nlobjJobManager#createJobRequest -->

Creates an empty duplicate-record job request.

**Returns:** `nlobjDuplicateJobRequest`

**Governance:** 0 units.

**Since:** 2013.1

### nlobjLogin (object)
<!-- id: ss1:nlobjLogin -->

The current user's login credentials, returned by `nlapiGetLogin`; used to change the user's login
email or password.

#### nlobjLogin.changeEmail(currentPassword, newEmail, justThisAccount)
<!-- id: ss1:nlobjLogin#changeEmail -->

Changes the user's login email.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `currentPassword` | string | Yes | |
| `newEmail` | string | Yes | |
| `justThisAccount` | boolean | No | `true` |

- `justThisAccount`: `true` changes the email for this account's roles only; `false` changes it for
  all accounts and roles.

**Returns:** not stated in the source.

**Errors:**

- An error is raised when an argument is invalid. The source does not give the error code.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjLogin.changePassword(currentPassword, newPassword)
<!-- id: ss1:nlobjLogin#changePassword -->

Changes the user's password.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `currentPassword` | string | Yes | |
| `newPassword` | string | Yes | |

**Returns:** not stated in the source.

**Errors:**

- An error is raised when an argument is invalid. The source does not give the error code.

**Governance:** 0 units.

**Since:** 2012.2

### nlobjPivotColumn (object)
<!-- id: ss1:nlobjPivotColumn -->

A column of a pivot table, reached from `nlobjPivotTable.getColumnHierarchy`. No 2.x mapping is
given in the source.

#### nlobjPivotColumn.getParent()
<!-- id: ss1:nlobjPivotColumn#getParent -->

Returns the parent column.

**Returns:** `nlobjPivotColumn`, or null when there is no parent.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotColumn.getSummaryLine()
<!-- id: ss1:nlobjPivotColumn#getSummaryLine -->

Returns the summary column.

**Returns:** `nlobjPivotColumn`, or null when there is no summary line.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotColumn.getVisibleChildren()
<!-- id: ss1:nlobjPivotColumn#getVisibleChildren -->

Returns the visible child columns.

**Returns:** `nlobjPivotColumn[]`, or null when there are no children.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotColumn.isHidden()
<!-- id: ss1:nlobjPivotColumn#isHidden -->

Returns `true` when the column is hidden.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotColumn.getDependency(alias)
<!-- id: ss1:nlobjPivotColumn#getDependency -->

Only the name is documented.

> [!SOURCE-ERROR] The source has only a heading for this method, with no description, and leaves it
> out of the object's method list. Its parameter's type, whether it is required, its return value
> and its release are all unstated.
<!-- source-error: ss1:nlobjPivotColumn#getDependency | - | undocumented_stub_heading_only -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `alias` | any | No | |

- `alias`: typed `any` because the source gives no type; requiredness is not stated either.

**Returns:** not stated in the source.

**Governance:** 0 units.

#### nlobjPivotColumn.getAlias()
<!-- id: ss1:nlobjPivotColumn#getAlias -->

Returns the column's alias.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotColumn.getLabel()
<!-- id: ss1:nlobjPivotColumn#getLabel -->

Returns the column's label.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotColumn.getValue()
<!-- id: ss1:nlobjPivotColumn#getValue -->

Returns the column's value.

**Returns:** `Object`

**Governance:** 0 units.

**Since:** 2012.2

### nlobjPivotRow (object)
<!-- id: ss1:nlobjPivotRow -->

A row of a pivot table, reached from `nlobjPivotTable.getRowHierarchy`. No 2.x mapping is given in
the source.

#### nlobjPivotRow.getChildren()
<!-- id: ss1:nlobjPivotRow#getChildren -->

Returns the child rows.

**Returns:** `nlobjPivotRow[]`, or null for a detail line or a row without children.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotRow.getOpeningLine()
<!-- id: ss1:nlobjPivotRow#getOpeningLine -->

Only the name is documented.

> [!SOURCE-ERROR] The source has only a heading for this method, with no description. Its return
> value and its release are unstated.
<!-- source-error: ss1:nlobjPivotRow#getOpeningLine | - | undocumented_stub_heading_only -->

**Returns:** not stated in the source.

**Governance:** 0 units.

#### nlobjPivotRow.getParent()
<!-- id: ss1:nlobjPivotRow#getParent -->

Returns the parent row.

**Returns:** `nlobjPivotRow`, or null when there is no parent.

> [!SOURCE-ERROR] The source's descriptions of `getParent` and `getSummaryLine` on this object appear
> to be swapped. The return facts shown here follow the spec; verify against a real report.
<!-- source-error: ss1:nlobjPivotRow#getParent | - | descriptions_of_getparent_and_getsummaryline_swapped -->

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotRow.getSummaryLine()
<!-- id: ss1:nlobjPivotRow#getSummaryLine -->

Returns the summary row.

**Returns:** `nlobjPivotRow`, or null for a detail line.

> [!SOURCE-ERROR] The source's descriptions of `getSummaryLine` and `getParent` on this object appear
> to be swapped. The return facts shown here follow the spec; verify against a real report.
<!-- source-error: ss1:nlobjPivotRow#getSummaryLine | - | descriptions_of_getparent_and_getsummaryline_swapped -->

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotRow.getValue(pivotColumn)
<!-- id: ss1:nlobjPivotRow#getValue -->

Returns a value of the row. It has two forms: `getValue()` with no argument returns the row's own
value, and `getValue(pivotColumn)` returns the row's value in that column. In the second form,
`pivotColumn` is a required `nlobjPivotColumn`. The source gives no parameter list of its own for
the method, so no table is shown.

**Returns:** `Object`, or null when the row is not a detail line.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotRow.isDetailLine()
<!-- id: ss1:nlobjPivotRow#isDetailLine -->

Returns `true` when the row is a detail line.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotRow.getAlias()
<!-- id: ss1:nlobjPivotRow#getAlias -->

Returns the row's alias.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotRow.getLabel()
<!-- id: ss1:nlobjPivotRow#getLabel -->

Returns the row's label.

> [!SOURCE-ERROR] The source lists and describes this method but has no heading of its own for it.
> Its release is not stated.
<!-- source-error: ss1:nlobjPivotRow#getLabel | - | member_heading_missing_listed_and_described -->

**Returns:** `string`

**Governance:** 0 units.

### nlobjPivotTable (object)
<!-- id: ss1:nlobjPivotTable -->

The result of a pivot report, from `nlobjPivotTableHandle.getPivotTable`. No 2.x mapping is given in
the source.

#### nlobjPivotTable.getColumnHierarchy()
<!-- id: ss1:nlobjPivotTable#getColumnHierarchy -->

Returns the root column of the pivot table.

**Returns:** `nlobjPivotColumn`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotTable.getRowHierarchy()
<!-- id: ss1:nlobjPivotTable#getRowHierarchy -->

Returns the root row of the pivot table.

**Returns:** `nlobjPivotRow`

**Governance:** 0 units.

**Since:** 2012.2

### nlobjPivotTableHandle (object)
<!-- id: ss1:nlobjPivotTableHandle -->

A handle on a pivot report execution, returned by `nlobjReportDefinition.executeReport` when no form
is passed. No 2.x mapping is given in the source.

#### nlobjPivotTableHandle.getPivotTable()
<!-- id: ss1:nlobjPivotTableHandle#getPivotTable -->

Returns the pivot table. The call blocks until the report has finished executing; if blocking is
not acceptable, poll `isReady` first.

**Returns:** `nlobjPivotTable`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjPivotTableHandle.isReady()
<!-- id: ss1:nlobjPivotTableHandle#isReady -->

Returns `true` when the report has finished executing.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2012.2

### nlobjReportColumn (object)
<!-- id: ss1:nlobjReportColumn -->

A column of a report definition, returned by `nlobjReportDefinition.addColumn`. No 2.x mapping is
given in the source.

#### nlobjReportColumn.getFormula()
<!-- id: ss1:nlobjReportColumn#getFormula -->

Returns the column's formula.

**Returns:** `string | null`; null when the column has no formula.

**Governance:** 0 units.

The source states no release for this method.

#### nlobjReportColumn.isMeasure()
<!-- id: ss1:nlobjReportColumn#isMeasure -->

Returns `true` when the column is a measure.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportColumn.getParent()
<!-- id: ss1:nlobjReportColumn#getParent -->

Returns the column hierarchy the column belongs to.

**Returns:** `nlobjReportColumnHierarchy`

**Governance:** 0 units.

The source states no release for this method.

### nlobjReportColumnHierarchy (object)
<!-- id: ss1:nlobjReportColumnHierarchy -->

A column hierarchy of a report definition, returned by `nlobjReportDefinition.addColumnHierarchy`.
No 2.x mapping is given in the source.

#### nlobjReportColumnHierarchy.getParent()
<!-- id: ss1:nlobjReportColumnHierarchy#getParent -->

Returns the parent hierarchy.

**Returns:** `nlobjReportColumnHierarchy | null`; null for the root.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportColumnHierarchy.getChildren()
<!-- id: ss1:nlobjReportColumnHierarchy#getChildren -->

Returns the child hierarchy.

**Returns:** `nlobjReportColumnHierarchy`

> [!SOURCE-ERROR] The method's name is plural, but the source gives a single object as the return
> type, not an array. Check what an existing script does with the result.
<!-- source-error: ss1:nlobjReportColumnHierarchy#getChildren | - | getchildren_plural_name_single_object_return -->

**Governance:** 0 units.

**Since:** 2012.2

### nlobjReportDefinition (object)
<!-- id: ss1:nlobjReportDefinition -->

A pivot report definition, returned by `nlapiCreateReportDefinition`. It is either rendered through
an `nlobjReportForm` or executed to read pivot table values. The source points to a worked example
of building a pivot report; it is not reproduced here. No 2.x mapping is given in the source.

#### nlobjReportDefinition.addColumn(alias, isMeasure, label, parent, format, formula)
<!-- id: ss1:nlobjReportDefinition#addColumn -->

Adds a column to the report.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `alias` | string | Yes | |
| `isMeasure` | boolean | Yes | |
| `label` | string | Yes | |
| `parent` | nlobjReportColumnHierarchy | No | |
| `format` | string | Yes | |
| `formula` | string | No | |

- `isMeasure`: `true` makes the column a measure.
- `parent`: the column hierarchy to place it under; null means no parent column.
- `format`: the column's data type.
- `formula`: written as `F(x,y,z) = expression`, where the variables are aliases defined earlier.

**Returns:** `nlobjReportColumn`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportDefinition.addColumnHierarchy(alias, label, parent, format)
<!-- id: ss1:nlobjReportDefinition#addColumnHierarchy -->

Adds a column hierarchy.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `alias` | string | Yes | |
| `label` | string | Yes | |
| `parent` | nlobjReportColumnHierarchy | No | |
| `format` | string | Yes | |

- `parent`: null makes it the root column.
- `format`: the column's data type.

**Returns:** `nlobjReportColumnHierarchy`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportDefinition.addRowHierarchy(alias, label, format)
<!-- id: ss1:nlobjReportDefinition#addRowHierarchy -->

Adds a row hierarchy.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `alias` | string | Yes | |
| `label` | string | Yes | |
| `format` | string | Yes | |

- `format`: the data type.

**Returns:** `nlobjReportRowHierarchy`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportDefinition.addSearchDatasource(searchType, id, filters, columns, map)
<!-- id: ss1:nlobjReportDefinition#addSearchDatasource -->

Feeds the report from a search.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `searchType` | string | Yes | |
| `id` | string | No | |
| `filters` | nlobjSearchFilter[] | Yes | |
| `columns` | nlobjSearchColumn[] | Yes | |
| `map` | string | Yes | |

- `id`: the ID of a saved search to use.
- `filters`: filter objects only; filter expressions are not supported.
- `map`: the mapping from search columns to report rows and columns.

**Returns:** not stated in the source.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportDefinition.executeReport(form)
<!-- id: ss1:nlobjReportDefinition#executeReport -->

Runs the report. Without a form, it runs synchronously and returns a handle from which the pivot
table can be read. With a form, it returns at once with the report form and no pivot handle; pass a
form when the report is to be rendered in the browser. Only one synchronous execution can run at a
time: starting a second one invalidates the first.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `form` | nlobjReportForm | No | |

**Returns:** `nlobjPivotTableHandle | nlobjReportForm`: the handle without a form, the report form
with one.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportDefinition.setTitle(title)
<!-- id: ss1:nlobjReportDefinition#setTitle -->

Sets the report title.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | No | |

**Returns:** not stated in the source.

**Governance:** 0 units.

**Since:** 2012.2

### nlobjReportForm (object)
<!-- id: ss1:nlobjReportForm -->

Wraps a report form and renders a report as HTML. It is what `nlapiCreateReportForm(title)` returns,
and what you pass to `nlobjReportDefinition.executeReport` to have the report rendered in the
browser. The source lists no methods for it and gives no 2.x mapping.

**Since:** 2012.2

### nlobjReportRowHierarchy (object)
<!-- id: ss1:nlobjReportRowHierarchy -->

A row hierarchy of a report definition, returned by `nlobjReportDefinition.addRowHierarchy`. No 2.x
mapping is given in the source.

#### nlobjReportRowHierarchy.getParent()
<!-- id: ss1:nlobjReportRowHierarchy#getParent -->

Returns the parent hierarchy.

**Returns:** `nlobjReportRowHierarchy | null`; null for the root.

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjReportRowHierarchy.getChild()
<!-- id: ss1:nlobjReportRowHierarchy#getChild -->

Returns the child hierarchy.

**Returns:** `nlobjReportRowHierarchy`

**Governance:** 0 units.

**Since:** 2012.2

### nlobjRequest (object)
<!-- id: ss1:nlobjRequest -->

The incoming GET or POST request of a Suitelet, passed as the first argument of the Suitelet
function.

> [!SOURCE-ERROR] The source's API map notes describe converting some of these getters to 2.x options
> calls, although the 2.x targets are read-only properties. Read them as properties when converting.
<!-- source-error: ss1:nlobjRequest | - | map_notes_write_readonly_properties_as_options_calls -->

#### nlobjRequest.getAllHeaders()
<!-- id: ss1:nlobjRequest#getAllHeaders -->

Returns an object that maps header names to values. Each header appears twice, once in lower case
and once in title case.

**Returns:** `Object`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getAllParameters()
<!-- id: ss1:nlobjRequest#getAllParameters -->

Returns an object that maps parameter names to values.

> [!SOURCE-ERROR] The source's return-type cell says a string array while its description says an
> object. The spec follows the description (`Object`); check how an existing script uses the result.
<!-- source-error: ss1:nlobjRequest#getAllParameters | - | returns_cell_says_string_array_description_says_object -->

**Returns:** `Object`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getBody()
<!-- id: ss1:nlobjRequest#getBody -->

Returns the body of a POST request.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjRequest.getFile(id)
<!-- id: ss1:nlobjRequest#getFile -->

Returns a file uploaded through a file field that was added with `nlobjForm.addField` (see
[objects-forms.md](objects-forms.md)).

> [!SOURCE-ERROR] The source has no parameters section for this method and a stray "10" in its text.
> The spec records one parameter, `id`, with no stated type or requiredness; pass the file field's
> name as an existing script does.
<!-- source-error: ss1:nlobjRequest#getFile | - | parameters_section_missing_stray_10_in_body -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | any | No | |

- `id`: typed `any` because the source gives no type; requiredness is not stated either.

**Returns:** `nlobjFile`

**Governance:** 0 units.

**Since:** 2010.1

#### nlobjRequest.getHeader(name)
<!-- id: ss1:nlobjRequest#getHeader -->

Returns one header's value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

- `name`: matched without regard to case.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getLineItemCount(group)
<!-- id: ss1:nlobjRequest#getLineItemCount -->

Returns the number of lines posted for a sublist. Lines are numbered from 1.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getLineItemValue(group, name, line)
<!-- id: ss1:nlobjRequest#getLineItemValue -->

Returns a posted sublist value. Custom column fields must be set to show on the form; hidden fields
are always available.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `name` | string | Yes | |
| `line` | number | Yes | |

- `line`: line number, counted from 1.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getMethod()
<!-- id: ss1:nlobjRequest#getMethod -->

Returns the HTTP method: `GET` or `POST`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjRequest.getParameter(name)
<!-- id: ss1:nlobjRequest#getParameter -->

Returns one parameter's value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getParameterValues(name)
<!-- id: ss1:nlobjRequest#getParameterValues -->

Returns all the values of a parameter.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjRequest.getURL()
<!-- id: ss1:nlobjRequest#getURL -->

Returns the request URL.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.1

### nlobjResponse (object)
<!-- id: ss1:nlobjResponse -->

Has two roles. In a Suitelet it is the outgoing response, passed as the second argument of the
Suitelet function. It is also what `nlapiRequestURL` returns, and several getters work only on that
returned object. gzip and deflate compression are supported.

#### nlobjResponse.addHeader(name, value)
<!-- id: ss1:nlobjResponse#addHeader -->

Adds a header. Headers you define yourself must start with `Custom-Header`. If a header of that name
is already set, a second one with the same name is added.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.getAllHeaders()
<!-- id: ss1:nlobjResponse#getAllHeaders -->

Returns the names of all headers. Only on the object returned by `nlapiRequestURL`.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.getBody()
<!-- id: ss1:nlobjResponse#getBody -->

Returns the response body. Only on the object returned by `nlapiRequestURL`.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjResponse.getCode()
<!-- id: ss1:nlobjResponse#getCode -->

Returns the HTTP status code as a string. Only on the object returned by `nlapiRequestURL`.

**Returns:** `string`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjResponse.getError()
<!-- id: ss1:nlobjResponse#getError -->

Returns the error of a failed call. Only on the object returned by `nlapiRequestURL` in a client
script.

**Returns:** `nlobjError`

**Governance:** 0 units.

**Availability:** client scripts (result of `nlapiRequestURL`).

The source states no release for this method.

#### nlobjResponse.getHeader(name)
<!-- id: ss1:nlobjResponse#getHeader -->

Returns one header's value. Only on the object returned by `nlapiRequestURL`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.getHeaders(name)
<!-- id: ss1:nlobjResponse#getHeaders -->

Returns all the values of a header. Only on the object returned by `nlapiRequestURL`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | No | |

> [!SOURCE-ERROR] The source does not say whether `name` is required; the spec marks it optional only
> because nothing is stated. Pass the header name.
<!-- source-error: ss1:nlobjResponse#getHeaders | name | param_requiredness_not_stated -->

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.renderPDF(xmlString)
<!-- id: ss1:nlobjResponse#renderPDF -->

Turns XML into a PDF with the BFO report generator and sends it as the response. It takes the same
input as `nlapiXMLToPDF` and exists for producing a PDF without File Cabinet permissions. Supported
in server scripts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `xmlString` | string | Yes | |

**Returns:** `void`

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2014.2

#### nlobjResponse.setCDNCacheable(type)
<!-- id: ss1:nlobjResponse#setCDNCacheable -->

Sets how long the CDN may cache the response. Only in shopping SSP pages; not supported in
Suitelets. Individual cached assets cannot be invalidated.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | constant | Yes | |

- `type`: `CACHE_DURATION_UNIQUE` (not cached), `CACHE_DURATION_SHORT` (5 minutes),
  `CACHE_DURATION_MEDIUM` (2 hours) or `CACHE_DURATION_LONG` (7 days), passed unquoted as a constant
  of the response object (for example `response.CACHE_DURATION_SHORT`).

**Returns:** `void`

**Governance:** 0 units.

**Availability:** shopping SSP pages only; not Suitelets.

**Since:** 2013.1

#### nlobjResponse.setContentType(type, name, disposition)
<!-- id: ss1:nlobjResponse#setContentType -->

Sets the content type of a Suitelet response. Suitelets only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `name` | string | No | |
| `disposition` | string | No | `attachment` |

- `type`: one of `AUTOCAD`, `BMPIMAGE`, `CSV`, `EXCEL`, `FLASH`, `GIFIMAGE`, `GZIP`, `HTMLDOC`,
  `ICON`, `JAVASCRIPT`, `JPGIMAGE`, `JSON`, `MESSAGERFC`, `MP3`, `MPEGMOVIE`, `MSPROJECT`, `PDF`,
  `PJPGIMAGE`, `PLAINTEXT`, `PNGIMAGE`, `POSTSCRIPT`, `POWERPOINT`, `QUICKTIME`, `RTF`, `SMS`,
  `STYLESHEET`, `TIFFIMAGE`, `VISIO`, `WORD`, `XMLDOC`, `ZIP`.
- `name`: a file name for the content, for example `name.pdf`.
- `disposition`: `inline` or `attachment`.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** Suitelets only.

**Since:** 2008.2

#### nlobjResponse.setEncoding(encodingType)
<!-- id: ss1:nlobjResponse#setEncoding -->

Sets the character encoding of the response. The browser's encoding must match it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `encodingType` | string | Yes | `UTF-8` |

- `encodingType`: `UTF-8`, `windows-1252`, `ISO-8859-1`, `GB18030`, `GB2312`, `SHIFT_JIS` or
  `MacRoman`, matched case-sensitively.

> [!SOURCE-ERROR] The source's note on `GB2312` (that it cannot be used for new files) appears to be
> copied from `nlobjFile.setEncoding`; it does not fit a response. The spec lists `GB2312` as allowed
> here, and `Big5` is not in this list.
<!-- source-error: ss1:nlobjResponse#setEncoding | encodingType | gb2312_new_file_note_copied_from_nlobjfile -->

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjResponse.setHeader(name, value)
<!-- id: ss1:nlobjResponse#setHeader -->

Sets a header of a Suitelet response. Headers you define yourself must start with `Custom-Header`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `value` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG` or `SSS_INVALID_HEADER`: the header name does not start with `Custom-Header`.

**Governance:** 0 units.

**Availability:** Suitelets only.

**Since:** 2008.2

#### nlobjResponse.sendRedirect(type, identifier, id, editmode, parameters)
<!-- id: ss1:nlobjResponse#sendRedirect -->

Redirects the user; the Suitelet counterpart of `nlapiSetRedirectURL`. Redirects go to internal
NetSuite URLs, except from Suitelets that are available without login.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `identifier` | string | Yes | |
| `id` | string | No | |
| `editmode` | boolean | No | |
| `parameters` | Object | No | |

- `type`: `RECORD`, `TASKLINK`, `SUITELET` or `EXTERNAL`; `EXTERNAL` only from Suitelets available
  without login.
- `id`: the record's internal ID.
- `editmode`: `true` opens the record in edit mode, `false` in view mode.
- `parameters`: extra URL parameters; their keys must start with `custparam`, otherwise
  `SSS_INVALID_ARG` is raised.

> [!SOURCE-ERROR] The source describes `id` as a record type ID, although the parameter carries the
> record's internal ID. Pass the internal ID, and verify against an existing working call.
<!-- source-error: ss1:nlobjResponse#sendRedirect | id | record_id_described_as_record_type_id -->

**Returns:** `void`

**Errors:**

- `SSS_INVALID_ARG`: a key in `parameters` does not start with `custparam`.

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.write(output)
<!-- id: ss1:nlobjResponse#write -->

Writes output to the response: a string or an `nlobjFile`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `output` | string \| nlobjFile | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.writePage(pageobject)
<!-- id: ss1:nlobjResponse#writePage -->

Writes a form or a list page to the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `pageobject` | nlobjForm \| nlobjList | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjResponse.writeLine(output)
<!-- id: ss1:nlobjResponse#writeLine -->

Writes a line of output to the response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `output` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

## Examples

A scheduled script that recalculates a custom field on a batch of records, reads its batch size from
a script parameter, reports progress, and stops cleanly when usage runs low. When converting, the
context calls map to `N/runtime`.

```js
function refreshVendorScores() {
  var context = nlapiGetContext();
  var batchSize = parseInt(context.getSetting('SCRIPT', 'custscript_score_batch'), 10) || 50;
  var ids = (context.getSetting('SCRIPT', 'custscript_vendor_ids') || '').split(',');
  var done = 0;

  for (var i = 0; i < ids.length && i < batchSize; i++) {
    if (context.getRemainingUsage() < 40) {
      nlapiLogExecution('AUDIT', 'Stopping early', 'Processed ' + done + ' vendors');
      break;
    }
    if (!ids[i]) {
      continue;
    }
    var vendor = nlapiLoadRecord('vendor', ids[i]);
    vendor.setFieldValue('custentity_score_checked', 'T');
    nlapiSubmitRecord(vendor);
    done += 1;
    context.setPercentComplete(Math.round((done / batchSize) * 100));
  }
  return done;
}
```

A Suitelet that answers GET with a PDF packing note built from XML, and rejects other methods.
`renderPDF` costs 10 units and needs no File Cabinet access.

```js
function packingNoteSuitelet(request, response) {
  if (request.getMethod() !== 'GET') {
    response.write('Only GET is supported.');
    return;
  }
  var orderRef = request.getParameter('custparam_order') || 'unknown';
  var xml = '<?xml version="1.0"?>\n' +
    '<pdf><head></head><body>' +
    '<p>Packing note for order ' + orderRef + '</p>' +
    '</body></pdf>';
  response.renderPDF(xml);
}
```

A scheduled helper that queues a CSV import from a File Cabinet file. `setMapping` comes first, as
the import object requires.

```js
function queuePriceListImport(savedImportId, fileId) {
  var job = nlapiCreateCSVImport();
  job.setMapping(savedImportId);
  job.setPrimaryFile(nlapiLoadFile(fileId).getValue());
  job.setOption('jobName', 'Nightly price list ' + new Date().toISOString().slice(0, 10));
  return nlapiSubmitCSVImport(job);
}
```

Inspecting a SuiteScript error in a catch block. Checking for `getCode` first keeps the handler safe
when a plain JavaScript error arrives instead (general JavaScript guidance).

```js
function describeFailure(e) {
  if (e && typeof e.getCode === 'function') {
    return e.getCode() + ': ' + e.getDetails() + ' (ref ' + e.getId() + ')';
  }
  return String(e);
}
```
