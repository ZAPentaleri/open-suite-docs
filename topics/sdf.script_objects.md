# SDF script objects
<!-- id: topic:sdf.script_objects -->

Part of: [SuiteCloud Development Framework](sdf.md)

See also: [SDF value lists for script objects](sdf.value_lists.md),
[SDF custom object files](sdf.object_files.md), [SDF manifest dependencies](sdf.manifest_dependencies.md),
[Script record](script_record.md), [Script deployment](script_deployment.md),
[Script parameters](script_parameters.md).

This page gathers the rules that all SDF script objects have in common. Each script type also has
its own page with full field tables. The reference listing for the shared fields here is the client
script set: [catalog:sdf_clientscript_object](../gen/catalogs/sdf_clientscript_object.md),
[catalog:sdf_clientscript_deployments](../gen/catalogs/sdf_clientscript_deployments.md) and
[catalog:sdf_clientscript_script_fields](../gen/catalogs/sdf_clientscript_script_fields.md) (see
[SDF clientscript object](sdf.clientscript.md)).

## Anatomy of a script object

A script object has two components:

1. the **script record**: the object's root element and its fields, and
2. a `scriptdeployments` structure with one `scriptdeployment` per deployment.

Script parameters, when the script has any, are defined in a `scriptcustomfields` structure on the
same object.

## Features the manifest must declare

Script objects depend on the `SERVERSIDESCRIPTING` feature, which the project manifest must declare.
Features are declared as `feature` elements inside `dependencies` > `features`; the `dependencies`
element comes after `frameworkversion`, and the `required` attribute (the string `true` or `false`)
decides whether a deploy fails when the feature is off in the target account (details on
[SDF manifest dependencies](sdf.manifest_dependencies.md)).

The IDs you may put in a `feature` element are the configurable feature IDs in
[catalog:sdf_list_configurable_features](../gen/catalogs/sdf_list_configurable_features.md): 265
upper-case tokens such as `SERVERSIDESCRIPTING`, `CRM`, `DEPARTMENTS`, `CUSTOMRECORDS` and
`WORKFLOW`. Some deployment audience fields need extra features (see
[Audience and filters on a deployment](#audience-and-filters-on-a-deployment)), and a value taken
from a feature-dependent value list needs the feature that list names
([the feature rule](sdf.value_lists.md#the-feature-rule)).

```xml
<dependencies>
  <features>
    <feature required="true">SERVERSIDESCRIPTING</feature>
    <feature required="false">DEPARTMENTS</feature>
  </features>
</dependencies>
```

This is only the `dependencies` fragment of `manifest.xml`; place it after the `frameworkversion`
element. The rest of the manifest is not described in the source used here.

## IDs, names and limits

| Item | Rule |
|---|---|
| Script record ID | Starts with `customscript_` |
| Deployment ID | Starts with `customdeploy_` |
| Script parameter ID | Starts with `custscript` |
| `scriptid` and `name` | At most 40 characters each |
| `description` | At most 999 characters |
| Script parameter `label` | At most 200 characters |

Two fields are always required on the script record: `name` and `scriptfile`. The `scriptfile`
value is a reference to a JavaScript file, written in the bracketed file form, for example
`[/SuiteScripts/harborline_ap/vendor_hold.js]`.

The catalogs type `name` and `description` (and, for parameters, `label` and `help`) as
single-select lists, which looks inconsistent for free-text fields; the tables show them that way
because that is how the source gives them.

## Defaults when you leave a field out

| Where | Field | Default |
|---|---|---|
| Script record | `notifyowner` | `T` |
| Script record | `notifyadmins` | `F` |
| Script record | `notifyuser` | `F` |
| Script record | `isinactive` | `F` |
| Deployment | `status` | `TESTING` |
| Deployment | `loglevel` | `DEBUG` |
| Deployment | `isdeployed` | `T` |
| Deployment | `allroles` | `F` |
| Deployment | `allemployees` | `F` |
| Deployment | `allpartners` | `F` |
| Deployment | `alllocalizationcontexts` | `T` |
| Script parameter | `fieldtype` | `TEXT` |
| Script parameter | `displaytype` | `NORMAL` |
| Script parameter | `accesslevel` | `2` |
| Script parameter | `searchlevel` | `2` |
| Script parameter | `storevalue` | `T` |
| Script parameter | `applyformatting` | `T` |
| Parameter role access | `accesslevel` | `0` |
| Parameter role access | `searchlevel` | `0` |
| Parameter filter | `fldfiltercomparetype` | `EQ` |
| Parameter filter | `fldfilterchecked` | `F` |
| Parameter filter | `fldfilternull` | `F` |
| Parameter filter | `fldfilternotnull` | `F` |

## Deployment status

The source gives seven values for a deployment's `status`: `TESTING`, `RELEASED`, `COMPLETED`,
`INPROGRESS`, `INQUEUE`, `NOTSCHEDULED` and `SCHEDULED` (the same set as
[catalog:sdf_list_script_status](../gen/catalogs/sdf_list_script_status.md)). A deployment without a
`status` element is `TESTING`.

- `RELEASED`: the script runs for the audience defined on the deployment.
- `TESTING`: the script runs only for the script owner and the deployment's audience.

The scheduling-style values (`COMPLETED`, `INPROGRESS`, `INQUEUE`, `NOTSCHEDULED`, `SCHEDULED`) are
part of the same list; the source does not say reliably which script types accept which of them
(see the warning below), so check the sub-topic for your script type.

> [!SOURCE-ERROR] The source first says only some of the status values are valid (its list of which
> values apply to non-scheduled scripts is garbled) and then lists all seven. Each script type's
> sub-topic gives the values the source confirms for that type; for any other value, verify in an
> account before relying on it.
<!-- source-error: topic:sdf.script_objects | - | says_only_some_status_values_valid_then_lists_all -->

> [!SOURCE-ERROR] This page of the source says `TESTING` limits execution to the owner and the
> audience, while the installation script material says `TESTING` means the owner only. The source
> does not say which is right, so verify who can run a `TESTING` deployment (especially for SDF
> installation scripts) before relying on either reading.
<!-- source-error: topic:sdf.script_objects | - | testing_status_owner_and_audience_vs_owner_only -->

## Audience and filters on a deployment

- Multi-select values are separated with a pipe: `<audslctrole>ADMINISTRATOR|CEO</audslctrole>`.
- Setting `allroles` to `T` selects every internal role. To reach external roles as well, list them
  in `audslctrole`.
- `localizationcontext` only takes effect when `alllocalizationcontexts` is `F`.
- Some audience fields depend on account features that the project manifest must declare (as shown
  above):

  | Field | Feature to declare |
  |---|---|
  | `allpartners` | `CRM` |
  | `audpartner` | `CRM` |
  | `auddepartment` | `DEPARTMENTS` |
  | `audsubsidiary` | `SUBSIDIARIES` |

> [!SOURCE-ERROR] The source tells you to declare a `SUBSIDIARIES` feature for `audsubsidiary`, but
> `SUBSIDIARIES` is not one of the IDs in the configurable feature list
> ([catalog:sdf_list_configurable_features](../gen/catalogs/sdf_list_configurable_features.md)), so a
> manifest that declares it may not validate. The source does not say which feature ID to use
> instead; validate the project against the target account before relying on either.
<!-- source-error: topic:sdf.script_objects | - | manifest_feature_SUBSIDIARIES_not_in_configurable_features -->

- Values that are specific to one account, such as internal IDs in `notifygroup` or in the audience
  fields, are not supported. Use script-ID references or the enumerated values instead.
- `recordtype` accepts a reference to a custom record type, a reference to a custom transaction
  type, or a standard record type ID.
- A deployment can also carry values for the parent script's own parameters (its custom fields), so
  each deployment can be configured differently. The source does not show the element that holds
  those values on a deployment.

## Script parameters

A parameter whose `fieldtype` is `SELECT` or `MULTISELECT` must also name a `selectrecordtype`.
Field-level values for parameters come from the value lists described in
[SDF value lists for script objects](sdf.value_lists.md).

| Element (under `scriptcustomfields.scriptcustomfield`) | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; starts with `custscript` |
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

The source gives defaults for `fieldtype` and for the role-access levels even though the catalogs
tag those fields as required; write them out explicitly to be safe.

## Example

A Suitelet object with one integer parameter and one released deployment. Every element comes from
the catalogs; the IDs are invented.

```xml
<suitelet scriptid="customscript_harborline_berth_view">
  <name>Harborline berth view</name>
  <scriptfile>[/SuiteScripts/harborline_ops/berth_view.js]</scriptfile>
  <description>Lists vessels expected per berth this week</description>
  <notifyowner>F</notifyowner>
  <scriptcustomfields>
    <scriptcustomfield scriptid="custscript_harborline_berth_days">
      <fieldtype>INTEGER</fieldtype>
      <label>Days to look ahead</label>
      <minvalue>1</minvalue>
      <maxvalue>30</maxvalue>
    </scriptcustomfield>
  </scriptcustomfields>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_harborline_berth_view">
      <title>Berth view (operations)</title>
      <status>RELEASED</status>
      <audslctrole>WAREHOUSE_MANAGER|ADMINISTRATOR</audslctrole>
      <loglevel>AUDIT</loglevel>
    </scriptdeployment>
  </scriptdeployments>
</suitelet>
```
