# SDF clientscript object
<!-- id: topic:sdf.clientscript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [ClientScript](../script-types/ClientScript.md). Shared rules (ID prefixes, defaults,
audience fields, status meanings): [SDF script objects](sdf.script_objects.md). Value lists:
[SDF value lists for script objects](sdf.value_lists.md).

Generated catalogs: [catalog:sdf_clientscript_object](../gen/catalogs/sdf_clientscript_object.md),
[catalog:sdf_clientscript_deployments](../gen/catalogs/sdf_clientscript_deployments.md),
[catalog:sdf_clientscript_script_fields](../gen/catalogs/sdf_clientscript_script_fields.md).

A `clientscript` object describes one client script record in an SDF project, together with the
records the script is deployed on.

## Structure

Besides its flat fields, the object can contain four nested structures:

- `buttons`: custom buttons the script adds to the form;
- `libraries`: extra script files loaded with the main file;
- `scriptcustomfields`: the script's parameters;
- `scriptdeployments`: one `scriptdeployment` per record the script runs on.

> [!SOURCE-ERROR] At one point the source page for the client script object talks about the portlet
> object instead, apparently a slip carried over from the portlet page. Read everything here as
> applying to `clientscript` and disregard that stray portlet mention.
<!-- source-error: topic:sdf.clientscript | - | client_page_refers_to_portlet_object -->

### Entry function fields

The object binds each client event to a function in the script file by naming that function in a
dedicated field. Leave out the fields for events the script does not handle.

| Field | Client event |
|---|---|
| `pageinitfunction` | `pageInit` |
| `saverecordfunction` | `saveRecord` |
| `validatefieldfunction` | `validateField` |
| `fieldchangedfunction` | `fieldChanged` |
| `postsourcingfunction` | `postSourcing` |
| `lineinitfunction` | `lineInit` |
| `validatelinefunction` | `validateLine` |
| `recalcfunction` | `recalc` |
| `validateinsertfunction` | `validateInsert` |
| `validatedeletefunction` | `validateDelete` |

Client scripts have two more event types, `localizationContextEnter` and
`localizationContextExit`. The object catalog has no function field for either of them, and the
source does not state how (or whether) they are bound through the SDF object.

### Buttons

Inside `buttons`, each `button` element defines one button. Its `buttonlabel` (the caption) is
required; `buttonfunction` names the script function the button runs.

### Deployments

Every client script deployment has to give its `recordtype` (which record it runs on) and its
`status`. The `recordtype` value can be a standard record type ID from
[scriptdeployment_recordtype](../gen/catalogs/sdf_list_scriptdeployment_recordtype.md) (values
are also listed in [allrecord_script_deployment_recordtype](../gen/catalogs/sdf_list_allrecord_script_deployment_recordtype.md)),
or a reference to a custom record type or a custom transaction type defined as an SDF object.

## Object fields

Source: [catalog:sdf_clientscript_object](../gen/catalogs/sdf_clientscript_object.md). Paths start
at the `clientscript` element. Limits and defaults in the Notes column come from the shared rules in
[SDF script objects](sdf.script_objects.md).

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customscript_`; max 40 characters |
| `name` | single-select list | | Required | Max 40 characters |
| `scriptfile` | filereference | | Required | Reference to a JavaScript file |
| `description` | single-select list | | Optional | Max 999 characters |
| `isinactive` | boolean | | Optional | Default `F` |
| `notifyadmins` | boolean | | Optional | Default `F` |
| `notifyemails` | string | | Optional | |
| `notifygroup` | string | | Optional | Account-specific values are not supported |
| `notifyowner` | boolean | | Optional | Default `T` |
| `notifyuser` | boolean | | Optional | Default `F` |
| `pageinitfunction` | string | | Optional | |
| `saverecordfunction` | string | | Optional | |
| `validatefieldfunction` | string | | Optional | |
| `fieldchangedfunction` | string | | Optional | |
| `postsourcingfunction` | string | | Optional | |
| `lineinitfunction` | string | | Optional | |
| `validatelinefunction` | string | | Optional | |
| `recalcfunction` | string | | Optional | |
| `validateinsertfunction` | string | | Optional | |
| `validatedeletefunction` | string | | Optional | |
| `buttons.button.buttonlabel` | single-select list | | Required in each `button` | |
| `buttons.button.buttonfunction` | string | | Optional | |
| `libraries.library.scriptfile` | filereference | | Required in each `library` | |

The catalog gives `name`, `description` and `buttonlabel` the type "single-select list", which is
odd for free-text fields; the table repeats the catalog as written.

## Deployment fields

Source: [catalog:sdf_clientscript_deployments](../gen/catalogs/sdf_clientscript_deployments.md).
Paths start at `clientscript.scriptdeployments.scriptdeployment`. Multi-select values are written
as one string with `|` between the items.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | Default `TESTING` |
| `recordtype` | single-select list | [scriptdeployment_recordtype](../gen/catalogs/sdf_list_scriptdeployment_recordtype.md) | Required | Also [allrecord_script_deployment_recordtype](../gen/catalogs/sdf_list_allrecord_script_deployment_recordtype.md); accepts custom record type and custom transaction type references |
| `allemployees` | boolean | | Optional | Default `F` |
| `allpartners` | boolean | | Optional | Default `F`; manifest must declare feature `CRM` |
| `allroles` | boolean | | Optional | Default `F`; `T` selects every internal role |
| `auddepartment` | multi-select list | | Optional | Manifest must declare feature `DEPARTMENTS` |
| `audemployee` | multi-select list | | Optional | |
| `audgroup` | multi-select list | | Optional | |
| `audpartner` | multi-select list | | Optional | Manifest must declare feature `CRM` |
| `audslctrole` | multi-select list | [generic_role](../gen/catalogs/sdf_list_generic_role.md) | Optional | Adds external roles on top of `allroles` |
| `audsubsidiary` | multi-select list | | Optional | Manifest must declare feature `SUBSIDIARIES` |
| `eventtype` | single-select list | [script_eventtype](../gen/catalogs/sdf_list_script_eventtype.md) | Optional | |
| `isdeployed` | boolean | | Optional | Default `T` |
| `loglevel` | single-select list | [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) | Optional | Default `DEBUG` |
| `alllocalizationcontexts` | boolean | | Optional | Default `T` |
| `executioncontext` | multi-select list | [execution_context](../gen/catalogs/sdf_list_execution_context.md) | Optional | |
| `localizationcontext` | multi-select list | [countries](../gen/catalogs/sdf_list_countries.md) | Optional | Only takes effect with `alllocalizationcontexts` set to `F` |

The audience fields cannot hold account-specific IDs (see [SDF script objects](sdf.script_objects.md)).

## Script parameter fields

Source: [catalog:sdf_clientscript_script_fields](../gen/catalogs/sdf_clientscript_script_fields.md).
Paths start at `clientscript.scriptcustomfields.scriptcustomfield`. Some values in
`generic_standard_field` and `generic_customfield_selectrecordtype` depend on a feature, which the
project manifest must then declare (see [the feature rule](sdf.value_lists.md#the-feature-rule)).

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `custscript` |
| `fieldtype` | single-select list | [generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md) | Required | Default `TEXT` |
| `label` | single-select list | | Required | Max 200 characters |
| `selectrecordtype` | single-select list | [generic_customfield_selectrecordtype](../gen/catalogs/sdf_list_generic_customfield_selectrecordtype.md) | Conditional | Required when `fieldtype` is `SELECT` or `MULTISELECT` |
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
| `roleaccesses.roleaccess.role` | single-select list | [customrecordtype_permittedrole](../gen/catalogs/sdf_list_customrecordtype_permittedrole.md) | Required in each `roleaccess` | |
| `roleaccesses.roleaccess.accesslevel` | single-select list | [generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) | Required in each `roleaccess` | Default `0` |
| `roleaccesses.roleaccess.searchlevel` | single-select list | [generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) | Required in each `roleaccess` | Default `0` |
| `customfieldfilters.customfieldfilter.fldfilter` | single-select list | [generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) | Required in each `customfieldfilter` | |
| `customfieldfilters.customfieldfilter.fldfilterchecked` | boolean | | Optional | Default `F` |
| `customfieldfilters.customfieldfilter.fldfiltercomparetype` | single-select list | [generic_customfield_fldfiltercomparetype](../gen/catalogs/sdf_list_generic_customfield_fldfiltercomparetype.md) | Optional | Default `EQ` |
| `customfieldfilters.customfieldfilter.fldfiltersel` | multi-select list | | Optional | |
| `customfieldfilters.customfieldfilter.fldfilterval` | string | | Optional | |
| `customfieldfilters.customfieldfilter.fldfilternotnull` | boolean | | Optional | Default `F` |
| `customfieldfilters.customfieldfilter.fldfilternull` | boolean | | Optional | Default `F` |
| `customfieldfilters.customfieldfilter.fldcomparefield` | single-select list | [generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) | Optional | |

## Example

A client script for item fulfillments that checks a field on change, blocks saving when a rule
fails, and offers a button to recount packages. It runs only in the user interface and only for
two roles. All IDs and paths are invented.

```xml
<clientscript scriptid="customscript_nw_ifl_pack_guard">
  <name>NW fulfillment packing guard</name>
  <scriptfile>[/SuiteScripts/nw_shipping/ifl_pack_guard.js]</scriptfile>
  <fieldchangedfunction>onFieldChanged</fieldchangedfunction>
  <saverecordfunction>onSaveRecord</saverecordfunction>
  <notifyowner>F</notifyowner>
  <buttons>
    <button>
      <buttonlabel>Recount packages</buttonlabel>
      <buttonfunction>recountPackages</buttonfunction>
    </button>
  </buttons>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_nw_ifl_pack_guard">
      <recordtype>ITEMFULFILLMENT</recordtype>
      <status>TESTING</status>
      <audslctrole>WAREHOUSE_MANAGER|STORE_MANAGER</audslctrole>
      <executioncontext>USERINTERFACE</executioncontext>
      <loglevel>AUDIT</loglevel>
    </scriptdeployment>
  </scriptdeployments>
</clientscript>
```
