# SDF custom object files
<!-- id: topic:sdf.object_files -->

Part of: [SuiteCloud Development Framework](sdf.md)

See also: [SDF script objects](sdf.script_objects.md), [SDF deploy file](sdf.deploy_file.md),
[SDF manifest dependencies](sdf.manifest_dependencies.md),
[catalog:sdf_object_types](../gen/catalogs/sdf_object_types.md).

Each custom object in an SDF project is a single XML file. This page covers where those files go,
how they are named, and how values and references are written inside them.

## Where object files live and how they are named

- Object files belong in the project's `Objects` folder.
- An object's script ID and its XML file name must be the same: an object with the script ID
  `customscript_tidewater_lotage` is stored as `customscript_tidewater_lotage.xml`.
- A script ID must begin with the prefix of its object type.
  [catalog:sdf_object_types](../gen/catalogs/sdf_object_types.md) is the list to use: each entry's
  ID is the object type's XML root tag, and its Value column is the script ID prefix for that type
  (for example `customrecordtype` takes `customrecord_`, `savedsearch` takes `customsearch_`,
  `translationcollection` takes `custcollection_`, and every script object type such as `suitelet`
  takes `customscript_`).
- Include your company name and the application name in script IDs, for example
  `customscript_tidewater_wms_putaway` instead of `customscript_putaway`, so that objects from
  different publishers do not collide.

The source's list of object types has several suspect entries, and the catalog reproduces the
source's values as given, so treat the following with care:

> [!SOURCE-ERROR] For `addressform`, `customsegment`, `integration` and `workflowactionscript`, the
> example prefix and the example root tag in the source's list do not match each other. Take the
> root tag from the catalog's ID column and confirm the prefix against an object exported from an
> account before you rely on it.
<!-- source-error: topic:sdf.object_files | - | prefix_and_root_tag_example_disagree -->

> [!SOURCE-ERROR] Two root-tag examples in the source's list are misspelled, `centercentertab` [sic]
> and `crmcustomrield` [sic]. The catalog lists them as `centertab` and `crmcustomfield`, which look
> like the intended tags; do not copy the misspelled forms.
<!-- source-error: topic:sdf.object_files | - | root_tag_examples_misspelled -->

> [!SOURCE-ERROR] The source gives `othercustomfield` the prefix `customrecord_`, which is doubtful
> for a field object. Check the prefix on an exported object before using it.
<!-- source-error: topic:sdf.object_files | - | othercustomfield_prefix_customrecord_doubtful -->

> [!SOURCE-ERROR] The prefixes given for `bankstatementparserplugin` and `fiparserplugin` (plain
> `customscript`) appear to have lost their trailing underscore. Verify against an exported object
> rather than copying the source value.
<!-- source-error: topic:sdf.object_files | - | plugin_prefixes_missing_underscore -->

## Writing values

| Value kind | How to write it | Example |
|---|---|---|
| Boolean | `T` or `F` | `<isinactive>T</isinactive>` |
| System enumeration value | Upper case | `<loglevel>ERROR</loglevel>` |
| Standard record or object | Upper case ID | `<recordtype>VENDOR</recordtype>` |
| Date | `yyyy-MM-dd` | `2027-03-08` |
| Time with time zone | `HH:mm:00Z` | `18:15:00Z` |
| Time without time zone | `HH:mm:00` | `18:15:00` |
| Date and time | the date, a literal `T`, then the zoned time | `2027-03-08T18:15:00Z` |

Seconds are not supported: the seconds position is always `00`, and any other seconds value is
discarded when you deploy. Values given in UTC are converted to the account's time zone; for
example, `09:30:00Z` in an account set to UTC-7 means 02:30 in the morning local time.

> [!SOURCE-ERROR] The source says UTC 09:30 in a UTC-7 account is 2:30 PM. Subtracting seven hours
> gives 2:30 AM; rely on the conversion rule, not the source's wording.
<!-- source-error: topic:sdf.object_files | - | utc_0930_at_utc_minus_7_described_as_2_30_pm -->

> [!SOURCE-ERROR] One sample time value in the source has an extra seconds segment beyond the
> `HH:mm:00` pattern. Follow the pattern in the table above instead.
<!-- source-error: topic:sdf.object_files | - | sample_time_value_has_extra_seconds_segment -->

## Referencing other objects and files

References are written inside square brackets:

| Target | Form |
|---|---|
| Object in the same project or account | `[scriptid=<id>]` |
| Child of an object | `[scriptid=<parent>.<child>]` |
| Object owned by another SuiteApp | `[appid=<appid>, scriptid=<id>]` |
| Object from a bundle | `[bundleid=<n>, scriptid=<id>]` |
| Field of a custom segment | `[appid=<appid>, type=<type>, scriptid=<parent>.<child>]` |
| File in the File Cabinet | `[/path/to/file.js]` |

Notes:

- A bundle reference names exactly one bundle ID per object.
- The `type` descriptor is used only when you reference a field that belongs to a custom segment,
  and it relies on the segment's unified ID. One value the source gives for it is
  `customrecordcustomfield`. Using unified segment IDs also keeps objects portable between
  accounts.
- Objects that live outside the project (in the account, in another SuiteApp or in a bundle) also
  have to be declared in the manifest; see [SDF manifest dependencies](sdf.manifest_dependencies.md).

A short user event script object that uses several of these forms (all IDs invented):

```xml
<usereventscript scriptid="customscript_tidewater_lot_guard">
  <name>Tidewater lot guard</name>
  <scriptfile>[/SuiteScripts/tidewater_lots/lot_guard.js]</scriptfile>
  <libraries>
    <library>
      <scriptfile>[/SuiteScripts/tidewater_common/date_rules.js]</scriptfile>
    </library>
  </libraries>
  <isinactive>F</isinactive>
  <aftersubmitfunction>afterSubmit</aftersubmitfunction>
  <scriptdeployments>
    <scriptdeployment scriptid="customdeploy_tidewater_lot_guard">
      <status>TESTING</status>
      <recordtype>[scriptid=customrecord_tidewater_lot_hold]</recordtype>
      <loglevel>ERROR</loglevel>
    </scriptdeployment>
  </scriptdeployments>
</usereventscript>
```

> [!SOURCE-ERROR] A tutorial in the source names one example object ID in its text but uses a
> different ID in its code. Neither is canonical; pick your own IDs that follow the prefix and
> file-name rules.
<!-- source-error: topic:sdf.object_files | - | prose_names_example_id_custobject_tutorial_code_differs -->

## Things deploy and import will not do for you

- **Password fields** are ignored when you deploy, and they come back masked when you import an
  object from the account, so a deploy cannot set them.
- **Deleting objects**: an account customization project deploy cannot delete objects from the
  account; dropping an object from the project leaves the account copy in place.
- **Entry forms and transaction forms**: import these from the account rather than writing them by
  hand.
- **Datasets, workbooks and saved searches**: do not edit their XML; change them in the account and
  import them again.
