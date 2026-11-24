# SDF manifest dependencies
<!-- id: topic:sdf.manifest_dependencies -->

Part of: [SuiteCloud Development Framework](sdf.md)

Related: [SDF custom object files](sdf.object_files.md) (reference syntax),
[SDF value lists for script objects](sdf.value_lists.md) (feature-dependent values),
[catalog:sdf_list_configurable_features](../gen/catalogs/sdf_list_configurable_features.md)
(feature IDs you can declare).

An SDF project's `manifest.xml` declares what the project needs from outside itself: features
that must be enabled, objects owned by another SuiteApp, objects from bundles, and (in account
customization projects) objects and files that already exist in the target account. All of these
go in a single `dependencies` element.

## General rules

- The `dependencies` element is placed after the `frameworkversion` element in the manifest.
- Feature declarations can always share the same `dependencies` element with object, file or bundle
  declarations; you do not need a second `dependencies` block.
- After editing the dependencies, validate the project against the account you are going to deploy
  to, so missing objects or features show up before deployment.

The source does not describe the rest of the manifest (its root element and other children) in
this topic, so the examples below show only the `dependencies` element.

## Features

Applies to SuiteApp projects and account customization projects alike.

Path: `dependencies` > `features` > `feature`. Each `feature` element holds a feature ID as its text
and carries a `required` attribute whose value is the string `true` or `false`:

| `required` | Effect |
|---|---|
| `true` | Installation or deployment fails if the feature is disabled in the target account |
| `false` | Installation or deployment goes ahead without the feature; intended for objects that work without it and are only enhanced by it |

### Which feature IDs are valid

The text of a `feature` element must be one of the configurable feature IDs listed in
[catalog:sdf_list_configurable_features](../gen/catalogs/sdf_list_configurable_features.md). That
list holds 265 upper-case IDs, from `ACCOUNTING` to `WORKORDERS`, written as single tokens (some with
underscores, such as `BALANCING_SEGMENTS` or `SUITECOMMERCE_ADVANCED`). It includes the IDs that
script objects most often need, such as `SERVERSIDESCRIPTING`, `CRM`, `DEPARTMENTS`, `CUSTOMRECORDS`,
`CUSTOMSEGMENTS`, `CUSTOMTRANSACTIONS` and `WORKFLOW`. The list gives no labels or descriptions for
the IDs, so the source does not state which account feature each one switches on beyond its name.

When to declare which feature:

- `SERVERSIDESCRIPTING` for the script objects (see [SDF script objects](sdf.script_objects.md),
  [SDF toolset object](sdf.toolset.md), [SDF singlepageapp object](sdf.singlepageapp.md)).
- `CRM` or `DEPARTMENTS` for some deployment audience fields (see
  [SDF script objects](sdf.script_objects.md#audience-and-filters-on-a-deployment)). The subsidiary
  audience field is documented as needing a `SUBSIDIARIES` feature, but that ID is not in the
  configurable feature list; the script objects page explains this.
- Whatever feature a feature-dependent value names (see
  [the feature rule](sdf.value_lists.md#the-feature-rule)).

```xml
<dependencies>
  <features>
    <feature required="true">SERVERSIDESCRIPTING</feature>
    <feature required="false">MULTICURRENCY</feature>
  </features>
</dependencies>
```

## Objects owned by another SuiteApp

Use this when your project refers to custom objects that belong to a different SuiteApp.

Path: `dependencies` > `applications` > `application` > `objects` > `object`.

- The `application` element's `id` attribute identifies the other SuiteApp in the form
  `publisherid.projectid`: its publisher ID, a dot, then its project ID.
- Each `object` element contains the script ID of one SDF custom object from that SuiteApp.

> [!SOURCE-ERROR] In the source, the project ID mentioned in the explanatory text does not match the
> application ID used in its XML sample (one uses a `com.samples` form, the other `com.example`).
> Neither is meaningful for your project: use the actual publisher and project IDs of the SuiteApp
> you depend on.
<!-- source-error: topic:sdf.manifest_dependencies | - | prose_project_id_differs_from_xml_example_application_id -->

```xml
<dependencies>
  <applications>
    <application id="com.harborline.fleetcore">
      <objects>
        <object>customrecord_hl_vehicle</object>
        <object>customlist_hl_vehicle_class</object>
      </objects>
    </application>
  </applications>
</dependencies>
```

Inside your own object files, such objects are then referenced with the
`[appid=<appid>, scriptid=<id>]` form ([SDF custom object files](sdf.object_files.md#referencing-other-objects-and-files)).

## Objects and files already in the account (account customization projects)

This form applies to account customization projects only. Inside `dependencies` it uses two
children, `objects` and `files`:

- `dependencies` > `objects` > `object`: a custom object that already exists in the target account
  and is not part of the project.
- `dependencies` > `files` > `file`: a file or script that already exists in the target account.
  The path must begin with `/`, which stands for the root of the File Cabinet.

> [!SOURCE-ERROR] The source's explanation names one custom record script ID while its sample XML
> lists a different one. The sample IDs are only placeholders; declare the script IDs that actually
> exist in your target account.
<!-- source-error: topic:sdf.manifest_dependencies | - | prose_names_customrecord_sample_xml_shows_customrecord -->

```xml
<dependencies>
  <features>
    <feature required="true">CUSTOMRECORDS</feature>
  </features>
  <objects>
    <object>customrecord_ops_site_audit</object>
  </objects>
  <files>
    <file>/SuiteScripts/ops_shared/audit_helpers.js</file>
  </files>
</dependencies>
```

## Objects from bundles

Use this when the project relies on objects that come from bundles outside the project. The source
does not say which project types this form applies to. The bundle objects must be of a kind that SDF
supports.

Path: `dependencies` > `bundles` > `bundle` > `objects` > `object`.

- The `bundle` element's `id` attribute contains only digits. When several bundles are copies of the
  same bundle, list their IDs together in the attribute, separated by `|`.
- Each `object` element holds the script ID of an object in the bundle. A child object is written as
  `<parent>.<child>`.

> [!SOURCE-ERROR] In one of the source's procedures the element is spelled `bunde` [sic], while
> everywhere else (and in the path above) it is `bundle`. Use `bundle` and let project validation
> confirm it.
<!-- source-error: topic:sdf.manifest_dependencies | - | bundle_element_spelled_bunde_in_second_procedure -->

```xml
<dependencies>
  <bundles>
    <bundle id="48213|48977">
      <objects>
        <object>customrecord_tx_rate_table</object>
        <object>customrecord_tx_rate_table.custrecord_tx_rate_region</object>
      </objects>
    </bundle>
  </bundles>
</dependencies>
```

In object files, such objects are referenced with the `[bundleid=<n>, scriptid=<id>]` form.
