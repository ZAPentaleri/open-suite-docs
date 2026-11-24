# SDF scheduledscript object
<!-- id: topic:sdf.scheduledscript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Sub-topics:

- [SDF scheduled deployment recurrence](sdf.recurrence.md)

Script type: [ScheduledScript](../script-types/ScheduledScript.md). Shared rules (ID prefixes,
defaults, status meanings): [SDF script objects](sdf.script_objects.md). Value lists:
[SDF value lists for script objects](sdf.value_lists.md).

Generated catalogs: [catalog:sdf_scheduledscript_object](../gen/catalogs/sdf_scheduledscript_object.md),
[catalog:sdf_scheduledscript_deployments](../gen/catalogs/sdf_scheduledscript_deployments.md),
[catalog:sdf_scheduledscript_script_fields](../gen/catalogs/sdf_scheduledscript_script_fields.md).

The `scheduledscript` object defines a scheduled script record and its deployments, including when
each deployment runs.

## Getting a starting object

You do not have to write the XML from nothing. Either create the script record in your IDE or in the
account and import it into the project, or start from the scheduled script template that the
SuiteCloud IDE plug-in or editor extension offers.

## Entry function and deployments

- The function to run is named in `defaultfunction`.
- Each deployment must give a `title` and a `status`.
- When a deployment runs is described by its `recurrence` element; the recurrence kinds and their
  fields are explained in [SDF scheduled deployment recurrence](sdf.recurrence.md).
- Scheduled deployments have no audience fields in the catalog.

## Object fields

From [catalog:sdf_scheduledscript_object](../gen/catalogs/sdf_scheduledscript_object.md). Paths are
relative to the `scheduledscript` root.

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
| `libraries.library.scriptfile` | filereference | | Required in a `library` | |
| `customplugintypes.plugintype.plugintype` | single-select list | | Required in a `plugintype` | |

The catalog has no `notifyuser` for this object. It types `name` and `description` as single-select
lists, which looks inconsistent for text fields; they are shown as given.

## Deployment fields

From [catalog:sdf_scheduledscript_deployments](../gen/catalogs/sdf_scheduledscript_deployments.md).
Paths are relative to `scheduledscript.scriptdeployments.scriptdeployment`.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | Default `TESTING` |
| `title` | string | | Required | |
| `isdeployed` | boolean | | Optional | Default `T` |
| `loglevel` | single-select list | [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) | Optional | Default `DEBUG` |

Recurrence fields (one recurrence kind per deployment; "Required" means required within that kind):

| Element | Value type | Value list | Use |
|---|---|---|---|
| `recurrence.single.startdate` | date | | Required |
| `recurrence.single.starttime` | time | | Required |
| `recurrence.single.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.daily.startdate` | date | | Required |
| `recurrence.daily.starttime` | time | | Required |
| `recurrence.daily.everyxdays` | integer | | Required |
| `recurrence.daily.enddate` | date | | Optional |
| `recurrence.daily.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.everyweekday.startdate` | date | | Required |
| `recurrence.everyweekday.starttime` | time | | Required |
| `recurrence.everyweekday.enddate` | date | | Optional |
| `recurrence.everyweekday.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.weekly.startdate` | date | | Required |
| `recurrence.weekly.starttime` | time | | Required |
| `recurrence.weekly.everyxweeks` | integer | | Required |
| `recurrence.weekly.sunday` ... `recurrence.weekly.saturday` (seven elements) | boolean | | Required |
| `recurrence.weekly.enddate` | date | | Optional |
| `recurrence.weekly.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.monthly.startdate` | date | | Required |
| `recurrence.monthly.starttime` | time | | Required |
| `recurrence.monthly.everyxmonths` | integer | | Required |
| `recurrence.monthly.enddate` | date | | Optional |
| `recurrence.monthly.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.monthlydayofweek.startdate` | date | | Required |
| `recurrence.monthlydayofweek.starttime` | time | | Required |
| `recurrence.monthlydayofweek.orderofweek` | single-select list | [generic_order_of_week](../gen/catalogs/sdf_list_generic_order_of_week.md) | Required |
| `recurrence.monthlydayofweek.dayofweek` | single-select list | [generic_day_of_week](../gen/catalogs/sdf_list_generic_day_of_week.md) | Required |
| `recurrence.monthlydayofweek.everyxmonths` | integer | | Required |
| `recurrence.monthlydayofweek.enddate` | date | | Optional |
| `recurrence.monthlydayofweek.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.yearly.startdate` | date | | Required |
| `recurrence.yearly.starttime` | time | | Required |
| `recurrence.yearly.enddate` | date | | Optional |
| `recurrence.yearly.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |
| `recurrence.yearlydayofweek.startdate` | date | | Required |
| `recurrence.yearlydayofweek.starttime` | time | | Required |
| `recurrence.yearlydayofweek.orderofweek` | single-select list | [generic_order_of_week](../gen/catalogs/sdf_list_generic_order_of_week.md) | Required |
| `recurrence.yearlydayofweek.dayofweek` | single-select list | [generic_day_of_week](../gen/catalogs/sdf_list_generic_day_of_week.md) | Required |
| `recurrence.yearlydayofweek.enddate` | date | | Optional |
| `recurrence.yearlydayofweek.repeat` | single-select list | [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md) | Optional |

## Script parameter fields

From [catalog:sdf_scheduledscript_script_fields](../gen/catalogs/sdf_scheduledscript_script_fields.md).
Paths are relative to `scheduledscript.scriptcustomfields.scriptcustomfield`. Values from
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

A clean-up job that starts every weekday at 01:15 UTC and repeats every four hours, until the end
of 2027. It is left in `TESTING` while it is being verified. IDs are invented.

```xml
<scheduledscript scriptid="customscript_acme_stale_quote_sweep">
  <name>Acme stale quote sweep</name>
  <scriptfile>[/SuiteScripts/acme_sales/stale_quote_sweep.js]</scriptfile>
  <defaultfunction>execute</defaultfunction>
  <notifyowner>T</notifyowner>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_acme_stale_quote_sweep">
      <title>Stale quote sweep (weekdays)</title>
      <status>TESTING</status>
      <loglevel>AUDIT</loglevel>
      <recurrence>
        <everyweekday>
          <startdate>2026-12-01</startdate>
          <starttime>01:15:00Z</starttime>
          <enddate>2027-12-31</enddate>
          <repeat>PT4H</repeat>
        </everyweekday>
      </recurrence>
    </scriptdeployment>
  </scriptdeployments>
</scheduledscript>
```
