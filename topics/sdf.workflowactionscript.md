# SDF workflowactionscript object
<!-- id: topic:sdf.workflowactionscript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [WorkflowActionScript](../script-types/WorkflowActionScript.md). Shared rules (ID
prefixes, defaults, audience fields, status meanings): [SDF script objects](sdf.script_objects.md).
Value lists: [SDF value lists for script objects](sdf.value_lists.md).

Generated catalogs: [catalog:sdf_workflowactionscript_object](../gen/catalogs/sdf_workflowactionscript_object.md),
[catalog:sdf_workflowactionscript_deployments](../gen/catalogs/sdf_workflowactionscript_deployments.md),
[catalog:sdf_workflowactionscript_script_fields](../gen/catalogs/sdf_workflowactionscript_script_fields.md).

The `workflowactionscript` object defines a custom workflow action backed by a script, plus the
record types it is deployed to.

## Entry function and return value

- The script's entry function is named in `defaultfunction`.
- `returntype` declares the type of value the action hands back to the workflow. It takes a field
  type from [generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md).
- When `returntype` is `SELECT`, `returnrecordtype` becomes required. It says which record type the
  returned value refers to, and accepts either a reference to a custom record type or a reference
  to a custom list (as well as the numeric IDs in
  [script_returnrecordtype](../gen/catalogs/sdf_list_script_returnrecordtype.md)).

## Deployments

Each deployment must give a `recordtype` and a `status`.

## Object fields

From [catalog:sdf_workflowactionscript_object](../gen/catalogs/sdf_workflowactionscript_object.md).
Paths are relative to the `workflowactionscript` root.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customscript_`; max 40 characters |
| `name` | single-select list | | Required | Max 40 characters |
| `scriptfile` | filereference | | Required | JavaScript file reference |
| `defaultfunction` | string | | Optional | Entry function name |
| `returntype` | single-select list | [generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md) | Optional | |
| `returnrecordtype` | single-select list | [script_returnrecordtype](../gen/catalogs/sdf_list_script_returnrecordtype.md) | Conditional | Required when `returntype` is `SELECT` |
| `description` | single-select list | | Optional | Max 999 characters |
| `isinactive` | boolean | | Optional | Default `F` |
| `notifyadmins` | boolean | | Optional | Default `F` |
| `notifyemails` | string | | Optional | |
| `notifygroup` | string | | Optional | No account-specific values |
| `notifyowner` | boolean | | Optional | Default `T` |
| `notifyuser` | boolean | | Optional | Default `F` |
| `customplugintypes.plugintype.plugintype` | single-select list | | Required in a `plugintype` | |
| `libraries.library.scriptfile` | filereference | | Required in a `library` | |

The catalog types `name` and `description` as single-select lists, which looks inconsistent for
text fields; they are shown as given.

## Deployment fields

From [catalog:sdf_workflowactionscript_deployments](../gen/catalogs/sdf_workflowactionscript_deployments.md).
Paths are relative to `workflowactionscript.scriptdeployments.scriptdeployment`.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | Default `TESTING` |
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

From [catalog:sdf_workflowactionscript_script_fields](../gen/catalogs/sdf_workflowactionscript_script_fields.md).
Paths are relative to `workflowactionscript.scriptcustomfields.scriptcustomfield`. Values from
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

A workflow action on support cases that returns a record from a custom list (the escalation tier it
picked), so the workflow can store it in a field. IDs are invented.

```xml
<workflowactionscript scriptid="customscript_acme_case_tier_pick">
  <name>Acme case tier picker</name>
  <scriptfile>[/SuiteScripts/acme_support/case_tier_pick.js]</scriptfile>
  <defaultfunction>onAction</defaultfunction>
  <returntype>SELECT</returntype>
  <returnrecordtype>[scriptid=customlist_acme_escalation_tier]</returnrecordtype>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_acme_case_tier_pick">
      <recordtype>SUPPORTCASE</recordtype>
      <status>RELEASED</status>
      <allroles>T</allroles>
      <loglevel>AUDIT</loglevel>
    </scriptdeployment>
  </scriptdeployments>
</workflowactionscript>
```
