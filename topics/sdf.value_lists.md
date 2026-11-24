# SDF value lists for script objects
<!-- id: topic:sdf.value_lists -->

Part of: [SuiteCloud Development Framework](sdf.md)

See also: [SDF script objects](sdf.script_objects.md), [SDF custom object files](sdf.object_files.md).

Many fields on SDF script objects do not take free text; they take one value (or, for multi-select
fields, several pipe-separated values) from a fixed list. In the script-object catalogs, an
entry's `group` names the list that the field accepts. This page maps every list to what it holds
and to the fields that use it.

## The feature rule

Two of the lists carry a `group` on their own entries, and there the `group` means something
different: it names the **account feature** that the value depends on. Those lists are
[catalog:sdf_list_generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) and
[catalog:sdf_list_generic_customfield_selectrecordtype](../gen/catalogs/sdf_list_generic_customfield_selectrecordtype.md).

The rule: when a mandatory field holds a value whose entry names a feature, the project manifest
must declare that feature. Examples of feature names that appear in these lists are `CLASSES`,
`DEPARTMENTS`, `LOCATIONS`, `SFA`, `ACCOUNTINGPERIODS`, `BILLINGACCOUNTS` and `MULTICURRENCY`;
entries with no `group` depend on no feature. Look the value up on the generated page to find its
feature.

For instance, a select-type script parameter that points at locations uses `-103`, and that entry
belongs to `LOCATIONS`, so the manifest has to declare the `LOCATIONS` feature:

```xml
<scriptcustomfield scriptid="custscript_acme_staging_location">
  <fieldtype>SELECT</fieldtype>
  <label>Staging location</label>
  <selectrecordtype>-103</selectrecordtype>
</scriptcustomfield>
```

The spec does not give the manifest's element names, so the declaration itself is not shown.

## Lists used by script records and deployments

| List | What it holds | Used by | Link |
|---|---|---|---|
| `script_status` | The seven deployment status values, from `TESTING` and `RELEASED` to the scheduling states | Every deployment's `status` | [sdf_list_script_status](../gen/catalogs/sdf_list_script_status.md) |
| `script_loglevel` | Log levels: `DEBUG`, `AUDIT`, `ERROR`, `EMERGENCY`, plus `INTERNAL` and `SYSTEM` | Every deployment's `loglevel` | [sdf_list_script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) |
| `script_eventtype` | 27 event types such as `CREATE`, `EDIT`, `XEDIT`, `APPROVE`, `SHIP`, `GET`, `POST` | `eventtype` on clientscript, usereventscript and suitelet deployments | [sdf_list_script_eventtype](../gen/catalogs/sdf_list_script_eventtype.md) |
| `scriptdeployment_recordtype` | About 300 upper-case record type IDs (`SALESORDER`, `VENDORBILL`, ...) | `recordtype` on clientscript, massupdatescript and workflowactionscript deployments | [sdf_list_scriptdeployment_recordtype](../gen/catalogs/sdf_list_scriptdeployment_recordtype.md) |
| `generic_standard_recordtype` | About 300 upper-case standard record type IDs | `recordtype` on usereventscript deployments; the `value` of an eventsubscriber criterion parameter | [sdf_list_generic_standard_recordtype](../gen/catalogs/sdf_list_generic_standard_recordtype.md) |
| `allrecord_script_deployment_recordtype` | A single value, `RECORD` | No field in the script-object catalogs points at it; the spec does not say which field uses it | [sdf_list_allrecord_script_deployment_recordtype](../gen/catalogs/sdf_list_allrecord_script_deployment_recordtype.md) |
| `generic_role` | Standard role IDs (`ADMINISTRATOR`, `AP_CLERK`, `WAREHOUSE_MANAGER`, ...) | `audslctrole` and `runasrole` on deployments | [sdf_list_generic_role](../gen/catalogs/sdf_list_generic_role.md) |
| `execution_context` | 39 execution contexts (`USERINTERFACE`, `CSVIMPORT`, `RESTLET`, `MAPREDUCE`, ...) | `executioncontext` on clientscript and usereventscript deployments | [sdf_list_execution_context](../gen/catalogs/sdf_list_execution_context.md) |
| `countries` | Two-letter country codes | `localizationcontext` on clientscript and usereventscript deployments | [sdf_list_countries](../gen/catalogs/sdf_list_countries.md) |
| `script_portlettype` | `LIST`, `FORM`, `HTML`, `LINKS` | `portlettype` on the portlet object | [sdf_list_script_portlettype](../gen/catalogs/sdf_list_script_portlettype.md) |
| `script_returnrecordtype` | Numeric record type IDs (negative numbers such as `-10`) | `returnrecordtype` on the workflowactionscript object | [sdf_list_script_returnrecordtype](../gen/catalogs/sdf_list_script_returnrecordtype.md) |
| `generic_centercategory` | Center category IDs that say where a link appears (`BASICSETUPCUSTOM`, ...) | `linkcategory` in suitelet deployment links | [sdf_list_generic_centercategory](../gen/catalogs/sdf_list_generic_centercategory.md) |
| `suiteletdeployment_tasktype` | A single value, `SCRIPT` | `linktasktype` in suitelet deployment links | [sdf_list_suiteletdeployment_tasktype](../gen/catalogs/sdf_list_suiteletdeployment_tasktype.md) |
| `generic_repeat_time` | Repeat intervals as ISO 8601 durations, from `PT15M` to `PT12H` | `repeat` in every recurrence kind | [sdf_list_generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) |
| `generic_day_of_week` | `MONDAY` through `SUNDAY` | `dayofweek` in monthly and yearly day-of-week recurrences | [sdf_list_generic_day_of_week](../gen/catalogs/sdf_list_generic_day_of_week.md) |
| `generic_order_of_week` | `FIRST`, `SECOND`, `THIRD`, `FOURTH`, `LAST` | `orderofweek` in the same recurrences | [sdf_list_generic_order_of_week](../gen/catalogs/sdf_list_generic_order_of_week.md) |

## Lists used by script parameters

| List | What it holds | Used by | Link |
|---|---|---|---|
| `generic_customfield_fieldtype` | 22 field types (`TEXT`, `INTEGER`, `SELECT`, `MULTISELECT`, `DATE`, `CHECKBOX`, ...) | Parameter `fieldtype`; also `returntype` on the workflowactionscript object | [sdf_list_generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md) |
| `generic_customfield_selectrecordtype` | About 450 record type IDs, mostly negative numbers; entries may name a feature | Parameter `selectrecordtype` | [sdf_list_generic_customfield_selectrecordtype](../gen/catalogs/sdf_list_generic_customfield_selectrecordtype.md) |
| `generic_standard_field` | About 2,600 standard field IDs (`STD...`); entries may name a feature | Parameter `searchcomparefield`, and `fldfilter` / `fldcomparefield` in parameter filters | [sdf_list_generic_standard_field](../gen/catalogs/sdf_list_generic_standard_field.md) |
| `generic_customfield_displaytype` | `NORMAL`, `HIDDEN`, `LOCKED`, `SHOWASLIST`, `STATICTEXT` | Parameter `displaytype` | [sdf_list_generic_customfield_displaytype](../gen/catalogs/sdf_list_generic_customfield_displaytype.md) |
| `generic_customfield_dynamicdefault` | `ME`, `NOW`, `DEPARTMENT`, `LOCATION`, `SUBSIDIARY`, `SUPERVISOR` | Parameter `dynamicdefault` | [sdf_list_generic_customfield_dynamicdefault](../gen/catalogs/sdf_list_generic_customfield_dynamicdefault.md) |
| `generic_customfield_fldfiltercomparetype` | Comparison operators `EQ`, `NE`, `GT`, `GTE`, `LT`, `LTE`, `LIKE`, `NOTLIKE` | `fldfiltercomparetype` in parameter filters | [sdf_list_generic_customfield_fldfiltercomparetype](../gen/catalogs/sdf_list_generic_customfield_fldfiltercomparetype.md) |
| `generic_customfield_onparentdelete` | `NO_ACTION`, `SET_NULL` | Parameter `onparentdelete` | [sdf_list_generic_customfield_onparentdelete](../gen/catalogs/sdf_list_generic_customfield_onparentdelete.md) |
| `generic_accesslevel_searchlevel` | Levels `0`, `1`, `2` | Parameter `accesslevel` and `searchlevel`, and the same fields in role access entries | [sdf_list_generic_accesslevel_searchlevel](../gen/catalogs/sdf_list_generic_accesslevel_searchlevel.md) |
| `customrecordtype_permittedrole` | Role IDs that may be granted access | `role` in parameter role access entries | [sdf_list_customrecordtype_permittedrole](../gen/catalogs/sdf_list_customrecordtype_permittedrole.md) |
| `script_setting` | `COMPANY`, `USER`, `PORTLET` | Parameter `setting` | [sdf_list_script_setting](../gen/catalogs/sdf_list_script_setting.md) |

## Other lists

| List | What it holds | Used by | Link |
|---|---|---|---|
| `translationcollection_defaultlanguage` | Language codes such as `bg-BG` or `ar` | By its name, the `defaultlanguage` of a [translation collection](sdf.translation_collections.md); no script catalog refers to it | [sdf_list_translationcollection_defaultlanguage](../gen/catalogs/sdf_list_translationcollection_defaultlanguage.md) |

The value lists for script objects that the spec singles out by name are `script_eventtype`,
`scriptdeployment_recordtype`, `script_loglevel`, `script_portlettype`, `script_status`,
`generic_role`, `suiteletdeployment_tasktype`, `execution_context`, `generic_repeat_time`,
`script_setting`, `generic_standard_field` and `generic_customfield_selectrecordtype`; the others
in the tables above are reached through the `group` of a catalog field.
