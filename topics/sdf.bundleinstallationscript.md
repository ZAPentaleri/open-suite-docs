# SDF bundleinstallationscript object
<!-- id: topic:sdf.bundleinstallationscript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [BundleInstallationScript](../script-types/BundleInstallationScript.md). Shared rules
(ID prefixes, defaults, status meanings): [SDF script objects](sdf.script_objects.md). Value lists:
[SDF value lists for script objects](sdf.value_lists.md).

Generated catalogs: [catalog:sdf_bundleinstallationscript_object](../gen/catalogs/sdf_bundleinstallationscript_object.md),
[catalog:sdf_bundleinstallationscript_deployments](../gen/catalogs/sdf_bundleinstallationscript_deployments.md),
[catalog:sdf_bundleinstallationscript_script_fields](../gen/catalogs/sdf_bundleinstallationscript_script_fields.md).

The `bundleinstallationscript` object defines a bundle installation script record and its
deployments. For SuiteApp-project installation logic that runs during an SDF deploy, see the
separate [SDF installation script objects](sdf.sdfinstallationscript.md).

## Entry functions

Five fields map the bundle lifecycle events to functions in the script file:

| Field | Script entry point |
|---|---|
| `beforeinstallfunction` | `beforeInstall` |
| `afterinstallfunction` | `afterInstall` |
| `beforeupdatefunction` | `beforeUpdate` |
| `afterupdatefunction` | `afterUpdate` |
| `beforeuninstallfunction` | `beforeUninstall` |

## Deployments

Each deployment must give a `title` and a `status`. Bundle installation deployments have no
audience fields in the catalog; besides the required fields they only take `isdeployed`,
`loglevel` and `runasrole`.

## Object fields

From [catalog:sdf_bundleinstallationscript_object](../gen/catalogs/sdf_bundleinstallationscript_object.md).
Paths are relative to the `bundleinstallationscript` root.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customscript_`; max 40 characters |
| `name` | single-select list | | Required | Max 40 characters |
| `scriptfile` | filereference | | Required | JavaScript file reference |
| `description` | single-select list | | Optional | Max 999 characters |
| `isinactive` | boolean | | Optional | Default `F` |
| `notifyadmins` | boolean | | Optional | Default `F` |
| `notifyemails` | string | | Optional | |
| `notifyowner` | boolean | | Optional | Default `T` |
| `beforeinstallfunction` | string | | Optional | |
| `afterinstallfunction` | string | | Optional | |
| `beforeupdatefunction` | string | | Optional | |
| `afterupdatefunction` | string | | Optional | |
| `beforeuninstallfunction` | string | | Optional | |
| `customplugintypes.plugintype.plugintype` | single-select list | | Required in a `plugintype` | |
| `libraries.library.scriptfile` | filereference | | Required in a `library` | |

The catalog has no `notifygroup` or `notifyuser` for this object. It also types `name` and
`description` as single-select lists, which looks inconsistent for text fields; they are shown as
given.

## Deployment fields

From [catalog:sdf_bundleinstallationscript_deployments](../gen/catalogs/sdf_bundleinstallationscript_deployments.md).
Paths are relative to `bundleinstallationscript.scriptdeployments.scriptdeployment`.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | Default `TESTING` |
| `title` | string | | Required | |
| `isdeployed` | boolean | | Optional | Default `T` |
| `loglevel` | single-select list | [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) | Optional | Default `DEBUG` |
| `runasrole` | single-select list | [generic_role](../gen/catalogs/sdf_list_generic_role.md) | Optional | |

## Script parameter fields

From [catalog:sdf_bundleinstallationscript_script_fields](../gen/catalogs/sdf_bundleinstallationscript_script_fields.md).
Paths are relative to `bundleinstallationscript.scriptcustomfields.scriptcustomfield`. Values from
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

A bundle installation script that only needs install and update hooks, running as an
administrator. IDs are invented.

```xml
<bundleinstallationscript scriptid="customscript_acme_bundle_setup">
  <name>Acme bundle setup</name>
  <scriptfile>[/SuiteScripts/acme_bundle/bundle_setup.js]</scriptfile>
  <afterinstallfunction>afterInstall</afterinstallfunction>
  <afterupdatefunction>afterUpdate</afterupdatefunction>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_acme_bundle_setup">
      <title>Bundle setup</title>
      <status>RELEASED</status>
      <runasrole>ADMINISTRATOR</runasrole>
      <loglevel>AUDIT</loglevel>
    </scriptdeployment>
  </scriptdeployments>
</bundleinstallationscript>
```
