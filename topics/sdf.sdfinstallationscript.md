# SDF installation script objects
<!-- id: topic:sdf.sdfinstallationscript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [SDFInstallationScript](../script-types/SDFInstallationScript.md). Shared rules (ID
prefixes, defaults): [SDF script objects](sdf.script_objects.md). Value lists:
[SDF value lists for script objects](sdf.value_lists.md). Where scripts are triggered:
[SDF deploy file](sdf.deploy_file.md).

Generated catalogs: [catalog:sdf_sdfinstallationscript_object](../gen/catalogs/sdf_sdfinstallationscript_object.md),
[catalog:sdf_sdfinstallationscript_deployments](../gen/catalogs/sdf_sdfinstallationscript_deployments.md),
[catalog:sdf_sdfinstallationscript_script_fields](../gen/catalogs/sdf_sdfinstallationscript_script_fields.md).

An SDF installation script is a script that runs while a SuiteApp project is being deployed. It is
described by the `sdfinstallationscript` object.

> [!SOURCE-ERROR] One source passage calls this object `sdfinstallationobject` [sic], which looks
> like a typo. The object-types catalog and the field catalogs use `sdfinstallationscript`; use that
> root tag.
<!-- source-error: topic:sdf.sdfinstallationscript | - | names_object_sdfinstallationobject -->

## Where it applies and what it is for

Installation scripts work only in **SuiteApp projects**; account customization projects cannot use
them. Typical jobs:

- validating the target account before or during installation,
- checking that required permissions and features are in place,
- stopping the installation when a check fails, and
- migrating data between versions.

## The three pieces

1. A SuiteScript file in the project's File Cabinet folder.
2. An `sdfinstallationscript` object that points at that file and defines one or more deployments.
3. A `run` element in the [deploy file](sdf.deploy_file.md) that names the script and deployment
   to run.

The position of the `run` element among the deploy file's entries decides when the script executes
in the deploy sequence.

## Runtime behaviour

- The script receives `fromVersion` and `toVersion`: the SuiteApp version currently installed in the
  account and the version being deployed. Compare them to decide which migration steps to run.
- If the script throws an error, the SuiteApp deployment fails. The error is written both to the
  script's execution log and to the deploy console.
- A deployment can set values for the script's parameters, so different deployments of the same
  script can behave differently.

## Deployments

Each deployment must give a `title` and a `status`. The status decides who can run it:

| Status | Who the deployment runs for |
|---|---|
| `TESTING` | Only the owner of the deployment |
| `RELEASED` | Other users as well |

This page's description of `TESTING` (owner only) is narrower than the general description on
[SDF script objects](sdf.script_objects.md) (owner and audience); the spec does not reconcile the
two, and installation script deployments have no audience fields.

## Object fields

From [catalog:sdf_sdfinstallationscript_object](../gen/catalogs/sdf_sdfinstallationscript_object.md).
Paths are relative to the `sdfinstallationscript` root.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customscript_`; max 40 characters |
| `name` | single-select list | | Required | Max 40 characters |
| `scriptfile` | filereference | | Required | JavaScript file reference |
| `description` | single-select list | | Optional | Max 999 characters |
| `isinactive` | boolean | | Optional | Default `F` |
| `notifyadmins` | boolean | | Optional | Default `F` |
| `notifyemails` | string | | Optional | |
| `notifygroup` | string | | Optional | No account-specific values |
| `notifyowner` | boolean | | Optional | Default `T` |
| `notifyuser` | boolean | | Optional | Default `F` |
| `customplugintypes.plugintype.plugintype` | single-select list | | Required in a `plugintype` | |

The catalog has no entry-function field for this object. It types `name` and `description` as
single-select lists, which looks inconsistent for text fields; they are shown as given.

## Deployment fields

From [catalog:sdf_sdfinstallationscript_deployments](../gen/catalogs/sdf_sdfinstallationscript_deployments.md).
Paths are relative to `sdfinstallationscript.scriptdeployments.scriptdeployment`.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | Default `TESTING` |
| `title` | string | | Required | |
| `isdeployed` | boolean | | Optional | Default `T` |
| `loglevel` | single-select list | [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) | Optional | Default `DEBUG` |

## Script parameter fields

From [catalog:sdf_sdfinstallationscript_script_fields](../gen/catalogs/sdf_sdfinstallationscript_script_fields.md).
Paths are relative to `sdfinstallationscript.scriptcustomfields.scriptcustomfield`. Values from
`generic_standard_field` and `generic_customfield_selectrecordtype` may require a manifest feature
(see [the feature rule](sdf.value_lists.md#the-feature-rule)).

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `custscript` |
| `fieldtype` | single-select list | [generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md) | Required | Default `TEXT` |
| `label` | single-select list | | Required | Max 200 characters |
| `selectrecordtype` | single-select list | [generic_customfield_selectrecordtype](../gen/catalogs/sdf_list_generic_customfield_selectrecordtype.md) | Conditional | Required for `SELECT` / `MULTISELECT` |
| `applyformatting` | boolean | | Optional | Default `T` |
| `defaultchecked` | boolean | | Optional | |
| `defaultselection` | single-select list | | Optional | |
| `defaultvalue` | string | | Optional | |
| `description` | string | | Optional | |
| `displaytype` | single-select list | [generic_customfield_displaytype](../gen/catalogs/sdf_list_generic_customfield_displaytype.md) | Optional | Default `NORMAL` |
| `dynamicdefault` | single-select list | [generic_customfield_dynamicdefault](../gen/catalogs/sdf_list_generic_customfield_dynamicdefault.md) | Optional | |
| `help` | single-select list | | Optional | |
| `linktext` | string | | Optional | |
| `minvalue` | string | | Optional | |
| `maxvalue` | string | | Optional | |
| `storevalue` | boolean | | Optional | Default `T` |
| `accesslevel` | single-select list | [generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) | Optional | Default `2` |
| `checkspelling` | boolean | | Optional | |
| `displayheight` | integer | | Optional | |
| `displaywidth` | integer | | Optional | |
| `isformula` | boolean | | Optional | |
| `ismandatory` | boolean | | Optional | |
| `maxlength` | string | | Optional | |
| `onparentdelete` | single-select list | [generic_customfield_onparentdelete](../gen/catalogs/sdf_list_generic_customfield_onparentdelete.md) | Optional | |
| `searchcomparefield` | single-select list | [generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) | Optional | |
| `searchdefault` | single-select list | | Optional | |
| `searchlevel` | single-select list | [generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) | Optional | Default `2` |
| `setting` | single-select list | [script_setting](../gen/catalogs/sdf_list_script_setting.md) | Optional | |
| `roleaccesses.roleaccess.role` | single-select list | [customrecordtype_permittedrole](../gen/catalogs/sdf_list_customrecordtype_permittedrole.md) | Required in a `roleaccess` | |
| `roleaccesses.roleaccess.accesslevel` | single-select list | [generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) | Required in a `roleaccess` | Default `0` |
| `roleaccesses.roleaccess.searchlevel` | single-select list | [generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) | Required in a `roleaccess` | Default `0` |
| `customfieldfilters.customfieldfilter.fldfilter` | single-select list | [generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) | Required in a `customfieldfilter` | |
| `customfieldfilters.customfieldfilter.fldfilterchecked` | boolean | | Optional | Default `F` |
| `customfieldfilters.customfieldfilter.fldfiltercomparetype` | single-select list | [generic_customfield_fldfiltercomparetype](../gen/catalogs/sdf_list_generic_customfield_fldfiltercomparetype.md) | Optional | Default `EQ` |
| `customfieldfilters.customfieldfilter.fldfiltersel` | multi-select list | | Optional | |
| `customfieldfilters.customfieldfilter.fldfilterval` | string | | Optional | |
| `customfieldfilters.customfieldfilter.fldfilternotnull` | boolean | | Optional | Default `F` |
| `customfieldfilters.customfieldfilter.fldfilternull` | boolean | | Optional | Default `F` |
| `customfieldfilters.customfieldfilter.fldcomparefield` | single-select list | [generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) | Optional | |

## Example

An installation script object with one parameter (a checkbox that turns on a data backfill) and a
released deployment. The matching `run` element goes in the deploy file; its inner element names
are not given in the spec, so it is not shown. IDs are invented.

```xml
<sdfinstallationscript scriptid="customscript_acme_app_migrate">
  <name>Acme app migration</name>
  <scriptfile>[/SuiteApps/com.acme.fieldops/migrate.js]</scriptfile>
  <notifyowner>T</notifyowner>
  <scriptcustomfields>
    <scriptcustomfield scriptid="custscript_acme_mig_backfill">
      <fieldtype>CHECKBOX</fieldtype>
      <label>Backfill visit history</label>
      <defaultchecked>T</defaultchecked>
    </scriptcustomfield>
  </scriptcustomfields>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_acme_app_migrate">
      <title>Migrate on deploy</title>
      <status>RELEASED</status>
      <loglevel>AUDIT</loglevel>
    </scriptdeployment>
  </scriptdeployments>
</sdfinstallationscript>
```
