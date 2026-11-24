# SDF massupdatescript object
<!-- id: topic:sdf.massupdatescript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [MassUpdateScript](../script-types/MassUpdateScript.md). Shared rules (ID prefixes,
defaults, audience fields, status meanings): [SDF script objects](sdf.script_objects.md). Value
lists: [SDF value lists for script objects](sdf.value_lists.md).

Generated catalogs: [catalog:sdf_massupdatescript_object](../gen/catalogs/sdf_massupdatescript_object.md),
[catalog:sdf_massupdatescript_deployments](../gen/catalogs/sdf_massupdatescript_deployments.md),
[catalog:sdf_massupdatescript_script_fields](../gen/catalogs/sdf_massupdatescript_script_fields.md).

The `massupdatescript` object defines a mass update script record and the record types on which the
mass update is offered.

## Entry function and deployments

- The script's entry function is named in `defaultfunction`.
- Each deployment must give a `recordtype` and a `status`.
- For a mass update deployment, `status` is either `RELEASED` or `TESTING`.

## Object fields

From [catalog:sdf_massupdatescript_object](../gen/catalogs/sdf_massupdatescript_object.md). Paths
are relative to the `massupdatescript` root.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customscript_`; max 40 characters |
| `name` | single-select list | | Required | Max 40 characters |
| `scriptfile` | filereference | | Required | JavaScript file reference |
| `defaultfunction` | string | | Optional | Entry function name |
| `description` | single-select list | | Optional | Max 999 characters |
| `isinactive` | boolean | | Optional | Default `F` |
| `notifyadmins` | boolean | | Optional | Default `F` |
| `notifyemails` | string | | Optional | |
| `notifygroup` | string | | Optional | No account-specific values |
| `notifyowner` | boolean | | Optional | Default `T` |
| `notifyuser` | boolean | | Optional | Default `F` |
| `libraries.library.scriptfile` | filereference | | Required in a `library` | |

The catalog types `name` and `description` as single-select lists, which looks inconsistent for
text fields; they are shown as given.

## Deployment fields

From [catalog:sdf_massupdatescript_deployments](../gen/catalogs/sdf_massupdatescript_deployments.md).
Paths are relative to `massupdatescript.scriptdeployments.scriptdeployment`.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | `RELEASED` or `TESTING`; default `TESTING` |
| `recordtype` | single-select list | [scriptdeployment_recordtype](../gen/catalogs/sdf_list_scriptdeployment_recordtype.md) | Required | |
| `allemployees` | boolean | | Optional | Default `F` |
| `allpartners` | boolean | | Optional | Default `F`; needs feature `CRM` |
| `allroles` | boolean | | Optional | Default `F`; `T` = all internal roles |
| `auddepartment` | multi-select list | | Optional | Needs feature `DEPARTMENTS` |
| `audemployee` | multi-select list | | Optional | |
| `audgroup` | multi-select list | | Optional | |
| `audpartner` | multi-select list | | Optional | Needs feature `CRM` |
| `audslctrole` | multi-select list | [generic_role](../gen/catalogs/sdf_list_generic_role.md) | Optional | Use for external roles |
| `audsubsidiary` | multi-select list | | Optional | Needs feature `SUBSIDIARIES` |
| `isdeployed` | boolean | | Optional | Default `T` |
| `loglevel` | single-select list | [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) | Optional | Default `DEBUG` |
| `runasrole` | single-select list | [generic_role](../gen/catalogs/sdf_list_generic_role.md) | Optional | |

## Script parameter fields

From [catalog:sdf_massupdatescript_script_fields](../gen/catalogs/sdf_massupdatescript_script_fields.md).
Paths are relative to `massupdatescript.scriptcustomfields.scriptcustomfield`. Values from
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

A mass update that re-stamps a hold flag on purchase orders, with a checkbox parameter that lets
the person running it choose whether to clear or set the flag. IDs are invented.

```xml
<massupdatescript scriptid="customscript_acme_po_hold_mu">
  <name>Acme PO hold mass update</name>
  <scriptfile>[/SuiteScripts/acme_purch/po_hold_mu.js]</scriptfile>
  <defaultfunction>each</defaultfunction>
  <scriptcustomfields>
    <scriptcustomfield scriptid="custscript_acme_po_hold_clear">
      <fieldtype>CHECKBOX</fieldtype>
      <label>Clear hold instead of setting it</label>
      <defaultchecked>F</defaultchecked>
    </scriptcustomfield>
  </scriptcustomfields>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_acme_po_hold_mu">
      <recordtype>PURCHASEORDER</recordtype>
      <status>TESTING</status>
      <audslctrole>AP_CLERK</audslctrole>
    </scriptdeployment>
  </scriptdeployments>
</massupdatescript>
```
