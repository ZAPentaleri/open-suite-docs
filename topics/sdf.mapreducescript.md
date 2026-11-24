# SDF mapreducescript object
<!-- id: topic:sdf.mapreducescript -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [MapReduceScript](../script-types/MapReduceScript.md). Shared rules (ID prefixes,
defaults, status meanings): [SDF script objects](sdf.script_objects.md). Value lists:
[SDF value lists for script objects](sdf.value_lists.md). Scheduling:
[SDF scheduled deployment recurrence](sdf.recurrence.md). Related: [Map/reduce](map_reduce.md).

Generated catalogs: [catalog:sdf_mapreducescript_object](../gen/catalogs/sdf_mapreducescript_object.md),
[catalog:sdf_mapreducescript_deployments](../gen/catalogs/sdf_mapreducescript_deployments.md),
[catalog:sdf_mapreducescript_script_fields](../gen/catalogs/sdf_mapreducescript_script_fields.md).

The `mapreducescript` object defines a map/reduce script record together with its deployments,
parameters and plug-in references.

## What the object contains

- The script record fields, including `scriptfile`. The script file must sit in the project's File
  Cabinet folder and is referenced by its path.
- `scriptdeployments`, which can hold any number of `scriptdeployment` elements. You can also import
  deployments that already exist in an account into the project.
- `scriptcustomfields`, the structure for script parameters.
- `customplugintypes`, which lists the custom plug-in types the script uses, each in a `plugintype`
  element.

Only the script record fields are mandatory; deployments, parameters and plug-in references are
optional. Because this is a server-side script, the project manifest must declare the
`SERVERSIDESCRIPTING` feature as a dependency.

> [!SOURCE-ERROR] The source's prose calls the parameter structure `customscriptfields` [sic], while
> its code and the catalog use `scriptcustomfields`. Use `scriptcustomfields`.
<!-- source-error: topic:sdf.mapreducescript | - | prose_names_customscriptfields_code_uses_scriptcustomfields -->

## Deployments

- Each deployment must give a `title` and a `status`.
- Scheduling works the same way as for scheduled scripts: a `recurrence` element with exactly one
  recurrence kind (see [SDF scheduled deployment recurrence](sdf.recurrence.md)).
- Defaults when you leave a field out:

  | Field | Default |
  |---|---|
  | `concurrencylimit` | `1` |
  | `buffersize` | `1` |
  | `yieldaftermins` | `60` |
  | `queueallstagesatonce` | `T` |
  | `status` | `TESTING` |
  | `loglevel` | `DEBUG` |

### Concurrency limit

The highest usable `concurrencylimit` depends on the account and on how many SuiteCloud Plus
licenses it has. SDF adjusts the value against that ceiling:

- If the project asks for more than the account allows, the deployed value is lowered to the
  account's limit.
- If the account's limit later grows, the deployed value is raised again, until someone edits the
  value somewhere other than the project.
- Importing the object back from the account keeps the value written in the project's XML, not the
  adjusted value.

## Object fields

From [catalog:sdf_mapreducescript_object](../gen/catalogs/sdf_mapreducescript_object.md). Paths are
relative to the `mapreducescript` root.

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
| `customplugintypes.plugintype.plugintype` | single-select list | | Required in a `plugintype` | |

The catalog has no `notifyuser` or `libraries` entry for this object. It types `name` and
`description` as single-select lists, which looks inconsistent for text fields; they are shown as
given.

## Deployment fields

From [catalog:sdf_mapreducescript_deployments](../gen/catalogs/sdf_mapreducescript_deployments.md).
Paths are relative to `mapreducescript.scriptdeployments.scriptdeployment`.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `customdeploy_` |
| `status` | single-select list | [script_status](../gen/catalogs/sdf_list_script_status.md) | Required | Default `TESTING` |
| `title` | string | | Required | |
| `isdeployed` | boolean | | Optional | Default `T` |
| `loglevel` | single-select list | [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) | Optional | Default `DEBUG` |
| `runasrole` | single-select list | [generic_role](../gen/catalogs/sdf_list_generic_role.md) | Optional | |
| `buffersize` | string | | Optional | Default `1` |
| `concurrencylimit` | string | | Optional | Default `1`; capped by the account |
| `queueallstagesatonce` | boolean | | Optional | Default `T` |
| `yieldaftermins` | string | | Optional | Default `60` |

The catalog types `buffersize`, `concurrencylimit` and `yieldaftermins` as strings although their
defaults are numbers; they are shown as given.

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

From [catalog:sdf_mapreducescript_script_fields](../gen/catalogs/sdf_mapreducescript_script_fields.md).
Paths are relative to `mapreducescript.scriptcustomfields.scriptcustomfield`. Values from
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

A map/reduce job that recalculates customer credit exposure on the first Monday of each month,
asking for a concurrency limit of 4 (SDF lowers this if the account's limit is smaller). IDs are
invented.

```xml
<mapreducescript scriptid="customscript_acme_credit_exposure_mr">
  <name>Acme credit exposure rebuild</name>
  <scriptfile>[/SuiteScripts/acme_credit/credit_exposure_mr.js]</scriptfile>
  <description>Rebuilds exposure totals per customer</description>
  <scriptcustomfields>
    <scriptcustomfield scriptid="custscript_acme_cx_batch_note">
      <fieldtype>TEXT</fieldtype>
      <label>Batch note</label>
    </scriptcustomfield>
  </scriptcustomfields>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_acme_credit_exposure_mr">
      <title>Monthly exposure rebuild</title>
      <status>TESTING</status>
      <concurrencylimit>4</concurrencylimit>
      <yieldaftermins>30</yieldaftermins>
      <loglevel>AUDIT</loglevel>
      <recurrence>
        <monthlydayofweek>
          <orderofweek>FIRST</orderofweek>
          <dayofweek>MONDAY</dayofweek>
          <everyxmonths>1</everyxmonths>
          <startdate>2027-01-04</startdate>
          <starttime>03:00:00Z</starttime>
        </monthlydayofweek>
      </recurrence>
    </scriptdeployment>
  </scriptdeployments>
</mapreducescript>
```
